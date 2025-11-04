<script lang="ts">
	import Header from "$lib/Header.svelte";
	let { children } = $props()
</script>

<main>
	<Header />
	<div id="container">
		{@render children()}
	</div>
</main>

<style>
	:global(body) {
		background-color: #24273a;
		margin: 0;
		padding: 0;
	}

	#container {
		margin-top: 0;
		height: 75vh;
		width: 100vw;
	}
</style>
