<script lang="ts">
	import Card from "$lib/Card.svelte";
	import FakeCard from "$lib/FakeCard.svelte";

	let clicked_index = $state(-1);
	// this token only has read perms don't worry
	let token = "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";
	// top and left position of the card that the user clicked
	let top = $state(0);
	let left = $state(0);
	// a list of all rendered cards
	let cards: any = $state([])

	// get a list of public repos on my github account
	async function get_repos() {
		let headers = new Headers();
		headers.append("Authorization", "Bearer " + token);
		headers.append("X-GitHub-Api-Version", "2022-11-28");
		let repos = await fetch('https://api.github.com/users/mythical84/repos', {
			method: 'GET',
			headers: headers
		});
		let json = await repos.json()
		return json;
	}

	function clicked(index: number) {
		clicked_index = index;
		// arbitrary adjustments to help the fake card line up better with the real one
		top = cards[index]?.getDomRect().top - 5;
		left = cards[index]?.getDomRect().left - 10;
	}

	function close() {
		clicked_index = -1;
	}

</script>

<svelte:head>
	<link href="./prism-xonokai.css" rel="stylesheet" />
</svelte:head>

<main>
	{#await get_repos()}
		<div id="placeholder"></div>
	{:then repos} 
		<!-- if a card has been clicked on, render a fake card in it's place and then expand it to fill the card holder -->
		{#if clicked_index != -1}
			<FakeCard {cards} {clicked_index} {left} {top} {close} />		
		{/if}
		<div id="container">
			{#each repos as repo, index}
				<Card name={repo.name} click={() => clicked(index)} bind:this={cards[index]} />
			{/each}
		</div>
	{/await}
</main>

<style>
	/* Just don't read any of the css it'll give you an aneurysm (warning for future Tyler) */

	main {
		position: absolute;
		left: 10px;
		top: calc(25vh - 10px);
		height: calc(71vh);
		width: calc(100vw - 20px);
		display: flex;
		justify-content: center;
		align-items: center;
		/* set color for border */
		--border-color: #7dc4e4;
		--track-color: #24273a;
	}

	#container {
		z-index: 0;
		width: 100%;
		height: 100%;
		border-image: conic-gradient(var(--border-color) var(--red-percentage),
			var(--track-color) var(--red-percentage), 
			var(--track-color) var(--red-percentage-2), 
			var(--border-color) var(--red-percentage-2)) 1;
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

	/* TODO: change these names from their debug counterparts */
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
</style>
