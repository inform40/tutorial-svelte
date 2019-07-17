<style>
	h1 {
		color: #1e88e5;
	}
</style>
<script>
    let name = '';
    $: reversed = name.split("").reverse().join("");
</script>

<input placeholder="What is your name?" type="text" bind:value="{name}">
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
