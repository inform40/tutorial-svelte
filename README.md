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
Siehe target Verzeichnis
