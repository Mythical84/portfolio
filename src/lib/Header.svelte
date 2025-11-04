<script lang="ts">
	import { page } from "$app/state";
	
	// calculate the number of years since I was born with 9 decimal places of precision 
	const birth = new Date("May 12, 2007");
	const milli_year = 31556952000;
	let years = $state("0")
	$effect(() => {
		const interval = setInterval(() => {
			years = ((Date.now() - birth.getTime()) / milli_year).toFixed(9)
		}, 100);
		return () => clearInterval(interval);
	})

	let path = $derived(page.url.pathname);
</script>

<main>
	<div id="text-container">
		<h2>Hi! I'm Tyler, {parseFloat(years) < 19 ? "an" : "a"} {years} year old developer <br/>
			studying computer science at UNC Charlotte.</h2>
	</div>

	<div id="link-container">
		<div class="spacer"></div>
		{#if path == '/about'}
			<a href="/about" id="about" class="underline">About</a>
		{:else}
			<a href="/about" id="about">About</a>
		{/if}

		{#if path == '/projects'}
			<a href="/projects" id="projects" class="underline">Projects</a>
		{:else}
			<a href="/projects" id="projects">Projects</a>
		{/if}

		{#if path == '/contact'}
			<a href="/contact" id="contact" class="underline">Contact</a>
		{:else}
			<a href="/contact" id="contact">Contact</a>
		{/if}
		<div class="spacer"></div>
	</div>
</main>

<style>
	main {
		color: #cad3f5;
		text-align: center;
		height: 20vh;
	}

	#text-container {
		margin-bottom: 2vw;
	}

	#link-container {
		display: flex;
		width: 100vw;
		justify-content: space-between;
	}

	.underline {
		text-decoration: underline;
		text-decoration-thickness: 4px;
		text-underline-position: under;
	}

	.spacer {
		width: 35vw;
	}

	a {
		display: flex;
		color: white;
		font-size: 20px;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 10vh;
		text-decoration: none;
	}

	#about {
		color: #f5a97f;
	}

	#projects {
		color: #7dc4e4;
	}

	#contact {
		color: #a6da95; 
	}

</style>
