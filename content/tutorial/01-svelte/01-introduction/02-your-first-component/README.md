---
title: 첫 번째 컴포넌트
---

스벨트에서는 애플리케이션을 하나 또는 여러 개의 _컴포넌트_ 로 구성합니다. 컴포넌트는 자신만의 HTML, CSS, Javascript 코드를 갖고 있으며, `.svelte`라는 확장자를 가집니다. 이렇게 컴포넌트를 이용하는 가장 큰 이유는 이것이 _재사용 가능_ 하기 때문입니다. 오른쪽에 보이는 코드는 `App.svelte` 파일로, 간단한 컴포넌트입니다. 

## 데이터 추가하기

오른쪽의 컴포넌트는 단순히 정적 마크업을 렌더링하네요. 스벨트가 이것만 한다면 그냥 예쁜 템플릿 언어밖에 되지 않을 겁니다. 여기에 데이터를 추가해 볼까요?

먼저, 컴포넌트에 `<script>` 태그를 추가하고, `name` 변수를 선언해 봅시다:

```svelte
+++<script>
	let name = 'Svelte';
</script>+++

<h1>Hello world!</h1>
```

그러면, `name` 변수를 마크업에서 사용할 수 있습니다:

```svelte
<h1>Hello +++{name}+++!</h1>
```

중괄호 안에는 어떠한 자바스크립트 표현식이라도 넣을 수 있습니다. `name` 을 `name.toUpperCase()` 로 바꿔서 조금 더 강렬하게 인사해볼까요?

```svelte
<h1>Hello {name+++.toUpperCase()+++}!</h1>
```
