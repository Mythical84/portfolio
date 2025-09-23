<script lang="ts">
	import Card from "$lib/Card.svelte";

	async function get_repos() {
		let repos = await fetch('https://api.github.com/users/mythical84/repos')
		return await repos.json()
	}

</script>

<main>
	<div id="container">
		{#await get_repos()}
			<div id="placehold"></div>
		{:then repos} 
			{#each repos as repo}
				<Card name={repo.name}/>
			{/each}
		{/await}
	</div>
</main>

<style>
	main {
		position: absolute;
		left: 10px;
		top: calc(23vh - 10px);
		height: calc(75vh);
		width: calc(100vw - 20px);
		display: flex;
		justify-content: center;
		align-items: center;
	}

	@property --red-percentage {
		syntax: '<percentage>';
		initial-value: 0%;
		inherits: false;
	}

	@property --red-percentage-2 {
		syntax: '<percentage>';
		initial-value: 100%;
		inherits: false;
	}

	#container {
		width: 100%;
		height: 100%;
		border-image: conic-gradient(orange var(--red-percentage), #454545 var(--red-percentage), #454545 var(--red-percentage-2), orange var(--red-percentage-2)) 1;
		border-radius: 5px;
		border-width: 4px;
		border-style: solid;
		animation: border-animation ease-in-out .5s forwards;
		overflow: scroll;
		display: grid;
		grid-template-columns: auto auto auto auto;
		justify-items: center;
		row-gap: 2vh;
		padding-top: 2vh;
		padding-bottom: 2vh;
	}

	@keyframes border-animation {
		from {--red-percentage: 0%; --red-percentage-2: 100%;}
		to {--red-percentage: 50%; --red-percentage-2: 50%;}
	}
</style>
