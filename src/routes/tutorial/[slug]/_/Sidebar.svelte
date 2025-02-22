<script>
	import { createEventDispatcher } from 'svelte';
	import { afterNavigate } from '$app/navigation';
	import Modal from '$lib/components/Modal.svelte';
	import Menu from './Menu/Menu.svelte';

	/** @type {import('$lib/types').PartStub[]} */
	export let index;

	/** @type {import('$lib/types').Section} */
	export let section;

	const dispatch = createEventDispatcher();

	const namespace = 'learn.svelte.dev';
	const copy_enabled = `${namespace}:copy_enabled`;

	/** @type {HTMLElement} */
	let sidebar;

	let show_modal = false;

	afterNavigate(async () => {
		// TODO ideally we would associate scroll state with
		// history. That's a little tricky to do right now,
		// so for now just always reset sidebar scroll
		sidebar.scrollTop = 0;
	});
</script>

<Menu {index} current={section} />

<div
	bind:this={sidebar}
	class="text"
	on:copy={(e) => {
		if (sessionStorage[copy_enabled]) return;

		/** @type {HTMLElement | null} */
		let node = /** @type {HTMLElement} */ (e.target);

		while (node && node !== e.currentTarget) {
			if (node.nodeName === 'PRE') {
				show_modal = true;

				e.preventDefault();
				return;
			}

			node = /** @type {HTMLElement | null} */ (node.parentNode);
		}
	}}
>
	<div
		on:click={(e) => {
			const node = /** @type {HTMLElement} */ (e.target);

			if (node.nodeName === 'CODE') {
				const { file } = node.dataset;
				if (file) {
					dispatch('select', { file });
				}
			}
		}}
	>
		{@html section.html}
	</div>

	{#if section.next}
		<p><a href="/tutorial/{section.next.slug}">다음 챕터: {section.next.title}</a></p>
	{/if}
</div>

<footer>
	<a class="edit" href="https://github.com/sveltejs/learn.svelte.dev/tree/main/{section.dir}">
		이 페이지 수정하기
	</a>
</footer>

{#if show_modal}
	<Modal on:close={() => (show_modal = false)}>
		<div class="modal-contents">
			<h2>복사/붙혀넣기가 비활성화 됐어요!</h2>

			<p>
				코드를 직접 치는 것이 실력 향상에 도움이 됩니다.
			</p>
			<label>
				<input
					type="checkbox"
					on:change={(e) => {
						sessionStorage[copy_enabled] = e.currentTarget.checked ? 'true' : '';
					}}
				/>
				잠시 동안 복사/붙혀넣기 사용하기
			</label>

			<button on:click={() => (show_modal = false)}>OK</button>
		</div>
	</Modal>
{/if}

<style>
	.text {
		flex: 1 1;
		overflow-y: auto;
		padding: 2.2rem 3rem;
		color: var(--second);
		border-right: 1px solid var(--border-color);
	}

	.text :global(a) {
		color: inherit;
		text-decoration: underline;
	}

	.text :global(h2) {
		font-size: 2.8rem;
		font-weight: normal;
		margin: 1.5em 0 0.5em 0;
	}

	.text :global(ul) {
		padding: 0 0 0 2rem;
	}

	.text :global(code) {
		background: hsl(206, 44%, 92%);
		padding: 0.2em 0.4em 0.3em;
		white-space: nowrap;
		position: relative;
		top: -0.1em;
	}

	.text :global(code[data-file]) {
		padding-right: 1.8em;
		cursor: pointer;
	}

	.text :global(code[data-file])::after {
		content: '';
		position: absolute;
		width: 1em;
		height: 1em;
		top: calc(50% - 0.55em);
		right: 0.5em;
		background: url(./file-edit.svg);
		background-size: 100% 100%;
	}

	.text :global(pre) {
		background: white;
		padding: 1rem 1.5rem;
		margin: 0 0 1.6rem 0;
		line-height: 1.3;
		border-radius: 0.5rem;
		box-shadow: inset 1px 1px 3px hsl(206deg 20% 93%);
	}

	.text :global(pre) :global(code) {
		background: none;
		color: var(--code-base);
		padding: 0;
		top: 0;
		white-space: pre;
	}

	.text :global(pre) :global(code)::before,
	.text :global(pre) :global(code)::after {
		content: none;
	}

	.text :global(pre) :global(.highlight) {
		--color: rgba(220, 220, 0, 0.2);
		background: var(--color);
		outline: 2px solid var(--color);
		border-radius: 2px;
	}

	.text :global(pre) :global(.highlight.add) {
		--color: rgba(0, 255, 0, 0.2);
	}

	.text :global(pre) :global(.highlight.remove) {
		--color: rgba(255, 0, 0, 0.2);
	}

	.text :global(blockquote) {
		margin: 2rem 0;
		padding: 2rem;
		border-radius: 0.5rem;
		border: 1.5px solid var(--flash);
		color: hsl(204, 100%, 40%);
	}

	.text :global(blockquote)::before {
		content: '!';
		position: relative;
		top: -0.1rem;
		right: -0.1rem;
		float: right;
		color: var(--flash);
		width: 2rem;
		height: 2rem;
		display: block;
		align-items: center;
		justify-content: center;
		border: 1.5px solid var(--flash);
		background: var(--flash);
		color: var(--light-blue);
		border-radius: 50%;
		text-align: center;
		font-size: 1.2rem;
		font-weight: bold;
		line-height: 1.9;
		margin: 0 0 1rem 1rem;
		opacity: 0.8;
	}

	footer {
		padding: 1rem 3rem;
		display: flex;
		justify-content: space-between;
		border-top: 1px solid var(--border-color);
		border-right: 1px solid var(--border-color);
	}

	footer .edit {
		color: var(--second);
		font-size: 1.4rem;
		padding: 0 0 0 1.4em;
		background: url(./file-edit.svg) no-repeat 0 calc(50% - 0.1em);
		background-size: 1em 1em;
	}

	.modal-contents h2 {
		font-size: 2.4rem;
		margin: 0 0 0.5em 0;
	}

	.modal-contents label {
		user-select: none;
	}

	.modal-contents button {
		display: block;
		background: var(--prime);
		color: white;
		padding: 1rem;
		width: 10em;
		margin: 1em 0 0 0;
		border-radius: var(--border-r);
		line-height: 1;
	}
</style>
