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
<h1>
    Hello, {name}!
</h1>
<h1>
    {name} reversed is: {reversed}.
</h1>
