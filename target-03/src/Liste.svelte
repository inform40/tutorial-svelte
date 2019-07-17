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
