<script context="module">
	/** @type {import('./__types/__layout').Load} */
	export async function load({ fetch }) {
		const res = await fetch('/tutorial.json');
		const { index } = await res.json();

		return {
			stuff: { index }
		};
	}
</script>

<script>
	import '@sveltejs/site-kit/base.css';
	import '@sveltejs/site-kit/code.css';
	import '../app.css';
	import { page, navigating } from '$app/stores';
	import { Icon, Icons, Nav, NavItem, SkipLink } from '@sveltejs/site-kit';
	import PreloadingIndicator from '$lib/components/PreloadingIndicator.svelte';
</script>

<Icons />

{#if $navigating && $navigating.to}
	<PreloadingIndicator />
{/if}

<SkipLink href="#main" />

<Nav {page} logo="/svelte-logo.svg">
	<svelte:fragment slot="nav-center">
		<strong class="large">스벨트 튜토리얼 - 베타!</strong>
		<!-- <NavItem href="/tutorial">Tutorial</NavItem>
		<NavItem href="/docs">Docs</NavItem>
		<NavItem href="/examples">Examples</NavItem>
		<NavItem href="/repl">REPL</NavItem>
		<NavItem href="/blog">Blog</NavItem>
		<NavItem href="/faq">FAQ</NavItem> -->
	</svelte:fragment>

	<svelte:fragment slot="nav-right">
		<NavItem external="https://kit.svelte.dev">SvelteKit</NavItem>

		<NavItem external="/chat" title="Discord Chat">
			<span class="small">Discord</span>
			<span class="large"><Icon name="message-square" /></span>
		</NavItem>

		<NavItem external="https://github.com/sveltejs/svelte" title="GitHub Repo">
			<span class="small">GitHub</span>
			<span class="large"><Icon name="github" /></span>
		</NavItem>
	</svelte:fragment>
</Nav>

<main id="main"><slot /></main>

<style>
	:global(body) {
		margin: 0;
		width: 100%;
		min-height: 100vh;
	}

	main {
		width: 100%;
		height: calc(100vh - var(--nav-h));
		position: relative;
		top: var(--nav-h);
	}

	.large {
		display: none;
	}

	@media (min-width: 800px) {
		.small {
			display: none;
		}

		.large {
			display: inline;
		}

		strong.large {
			height: 100%;
			display: flex;
			align-items: center;
			color: var(--second);
			text-transform: uppercase;
			font-size: 1.2rem;
			opacity: 1;
			white-space: nowrap;
		}
	}
</style>
