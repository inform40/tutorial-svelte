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
