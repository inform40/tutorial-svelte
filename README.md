# WebTech Tutorial: SvelteJS

## 1. Einführung
## 2. Hello World
### Template
Um Zeit zu sparen, nutzen wir ausnahmsweise mal das Einsteiger Template von SvelteJs
```
cd 01-components
npm install
```
### Eigene Komponente
* `Greeting.svelte` anlegen
* Komponente füllen
  * Property beachten!
```svelte
<script>
  export let name;
</script>

<h1>
  Hello, {name}!
</h1>
```
* Komponente benutzen
```svelte
<script>
    import Greeting from './Greeting.svelte' ;
</script>

<h1>Hello world!</h1>
<Greeting name="Finn"></Greeting>
```
* Styling anlegen
  * Wie CSS Modules
```svelte
<style>
	h1 {
		color: #1e88e5;
	}
</style>
```
* Property durch Children ersetzen
```svelte
<style>
	h1 {
		color: #1e88e5;
	}
</style>

<h1>
    Hello, <slot></slot>!
</h1>
```

* Named Slots
```svelte
<h1>
    Hello, <slot name="name"></slot>, <slot name="title" />!
</h1>
```
```svelte
<Greeting>
    <span slot="name">Finn</span>
    <span slot="title">du alter Zerstörer</span>
</Greeting>
```
* 404 Slots
```svelte
<h1>
    Hello, <slot name="name"></slot>, <slot name="title">coole Socke</slot>!
</h1>
```
```svelte
<Greeting>
    <span slot="name">Finn</span>
</Greeting>
```

## 3. $ Operator
* Bindings
```svelte
<script>
    let name = '';
</script>

<input placeholder="What is your name?" type="text" bind:value="{name}">
<h1>
    Hello, {name}!
</h1>

```
```svelte
<Greeting />
```
* Following Changes
```svelte
<script>
    let name = '';
    $: reversed = name.split("").reverse().join("");
</script>

<input placeholder="What is your name?" type="text" bind:value="{name}">
<h1>
    Hello, {name}!
</h1>
<h1>
    {name} reversed is: {reversed}.
</h1>
```
## 4. Verzweigungen & Schleifen
* if
```svelte
{#if name!==''}
<h1>
    Hello, {name}!
</h1>
<h1>
    {name} reversed is: {reversed}.
</h1>
{/if}
```
* if .. else
```svelte
{#if name!==''}
<h1>
    Hello, {name}!
</h1>
<h1>
    {name} reversed is: {reversed}.
</h1>
{:else}
<h2>
<i>
Bitte geben Sie einen Namen ein.
</i>
</h2>
{/if}
```
* if .. else if
```svelte
{#if name==='Holger'}
<iframe width="560" height="315" src="https://www.youtube.com/embed/cvuxeVpxPUU" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
{:else if name!==''}
<h1>
    Hello, {name}!
</h1>
<h1>
    {name} reversed is: {reversed}.
</h1>
{:else}
<h2>
<i>
Bitte geben Sie einen Namen ein.
</i>
</h2>
{/if}
```
* Schleifen
  * Neue Datei `Liste.svelte`
```svelte
<style>
	h1 {
		color: #1e88e5;
	}
</style>
<script>
    let Liste = [
        "Andreas",
        "Christian",
        "Tim",
        "Alex"
    ] ;
</script>

 <h1>
    F7 Entwicklung
</h1>
{#each Liste as Entwickler, i}
    <h2>
        {i+1}. {Entwickler}
    </h2>
{/each}
```
```svelte
<script>
    import Greeting from './Greeting.svelte' ;
    import Liste from './Liste.svelte';
</script>
<h1>Hello world!</h1>
<Greeting />
<Liste />
```
## 6. Transitions
* Neue Datei `Visible.svelte`
```svelte
<style>
	h1 {
		color: #1e88e5;
	}
</style>
<script>
    import { fly, fade } from 'svelte/transition';
    let visible = true;
</script>

<label>
	<input type="checkbox" bind:checked={visible}>
	visible
</label>
{#if visible}
    <h1 transition:fly={{y: 250}}>
        Borussia Dortmund > Sch**** 05
    </h1>
    <h1 in:fly={{y: 250}} out:fade>
        Ballspielverein Borussia Dortmund von 1909
    </h1>
{/if}
```

```svelte
<script>
    import Greeting from './Greeting.svelte' ;
    import Liste from './Liste.svelte';
    import Visible from './Visible.svelte';
</script>
<h1>Hello world!</h1>
<Greeting />
<Liste />
<Visible />
```

## 7. ToDo-Liste
```svelte
<style>
	.new-todo {
		font-size: 1.4em;
		width: 100%;
		margin: 2em 0 1em 0;
	}

	.board {
		max-width: 36em;
		margin: 0 auto;
	}

	.left, .right {
		float: left;
		width: 50%;
		padding: 0 1em 0 0;
		box-sizing: border-box;
	}

	h2 {
		font-size: 2em;
		font-weight: 200;
		user-select: none;
	}

	label {
		top: 0;
		left: 0;
		display: block;
		font-size: 1em;
		line-height: 1;
		padding: 0.5em;
		margin: 0 auto 0.5em auto;
		border-radius: 2px;
		background-color: #eee;
		user-select: none;
	}

	input { margin: 0 }

	.right label {
		background-color: rgb(180,240,100);
	}

	button {
		float: right;
		height: 1em;
		box-sizing: border-box;
		padding: 0 0.5em;
		line-height: 1;
		background-color: transparent;
		border: none;
		color: rgb(170,30,30);
		opacity: 0;
		transition: opacity 0.2s;
	}

	label:hover button {
		opacity: 1;
	}
</style>

<script>
	import { quintOut } from 'svelte/easing';
	import { crossfade } from './transitions/crossfade.js';
	import { flip } from './transitions/flip';

	const [send, receive] = crossfade({
		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 2000,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let todos = [
		{ id: 1, done: false, description: 'Unit Tests schreiben' },
		{ id: 2, done: false, description: 'Mittagessen' },
		{ id: 3, done: true, description: 'In der Nase bohren' },
		{ id: 4, done: false, description: 'Den Azubi beleidigen' },
		{ id: 5, done: false, description: 'Kafeepause' },
		{ id: 6, done: false, description: 'Case bearbeiten' },
	];

	let uid = todos.length + 1;

	function add(input) {
		const todo = {
			id: uid++,
			done: false,
			description: input.value
		};

		todos = [todo, ...todos];
		input.value = '';
	}

	function remove(todo) {
		todos = todos.filter(t => t !== todo);
	}

	function mark(todo, done) {
		todo.done = done;
		remove(todo);
		todos = todos.concat(todo);
	}

	function handleKeydown(event) {
		if (event.which === 13) {
			add(event.target);
		}
	}
</script>

<div class='board'>
	<input
		class="new-todo"
		placeholder="Was ist zu tun?"
		on:keydown={handleKeydown}
	>

	<div class='left'>
		<h2>todo</h2>
		{#each todos.filter(t => !t.done) as todo (todo.id)}
			<label
			    in:receive="{{key:todo.id}}"
			    out:send="{{key:todo.id}}"
			    animate:flip>
				<input type=checkbox on:change={() => mark(todo, true)}>
				{todo.description}
				<button on:click="{() => remove(todo)}">x</button>
			</label>
		{/each}
	</div>

	<div class='right'>
		<h2>done</h2>
		{#each todos.filter(t => t.done) as todo (todo.id)}
			<label
			    in:receive="{{key:todo.id}}"
			    out:send="{{key:todo.id}}"
			    animate:flip>
				<input type=checkbox checked on:change={() => mark(todo, false)}>
				{todo.description}
				<button on:click="{() => remove(todo)}">x</button>
			</label>
		{/each}
	</div>
</div>
```
