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
		<a href="/about" id="about">
			About
		</a>
		<a href="/projects" id="projects">
			Projects
		</a>
		<a href="/contact" id="contact">
			Contact
		</a>
		<div class="spacer"></div>
	</div>
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
	}

	main {
		color: white;
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

	.spacer {
		width: 35vw;
	}

	a {
		display: flex;
		color: white;
		text-decoration: none;
		font-size: 20px;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 10vh;
	}

	#about {
		color: cyan;
	}

	#projects {
		color: orange;
	}

	#contact {
		color: lightgreen; 
	}

</style>
