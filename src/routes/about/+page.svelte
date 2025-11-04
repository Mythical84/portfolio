<script lang="ts">
	import { gql, request } from "graphql-request";
	import colors from "$lib/colors.json";
	import python from "$lib/python.png";

	// TODO: replace this with requests to the rest api, the graphql api is super slow 
	// honestly I should just make a single rest request to get all of the repos on page load and store the results
	async function languages() {
		const query = gql`
			query {
				user(login: "mythical84") {
					# fetch only owner repos & not forks
					repositories(ownerAffiliations: OWNER, isFork: false, first: 20) {
						nodes {
							languages(first: 6, orderBy: {field: SIZE, direction: DESC}) {
								edges {
									size
									node {
										name
									}
								}
							}
						}
					}
				}
			}
		`;

		let token = "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";
		let headers = new Headers();
		headers.append("Authorization", "Bearer " + token);
		headers.append("X-GitHub-Api-Version", "2022-11-28");
		let data = await request({
			url: "https://api.github.com/graphql",
			document: query,
			requestHeaders: headers
		});
		let repos = data.user.repositories.nodes;
		let total = 0;
		let lang_freq: any = {};
		for (let i = 0; i < repos.length; i++) {
			let l = repos[i].languages.edges;
			for (let j = 0; j < l.length; j++) {
				total += parseInt(l[j].size);
				if (!Object.keys(lang_freq).includes(l[j].node.name)) {
					lang_freq[l[j].node.name] = l[j].size;
				} else {
					lang_freq[l[j].node.name] += l[j].size;
				}
			}
		}
		
		for (let i = 0; i < Object.keys(lang_freq).length; i++) {
			let keys = Object.keys(lang_freq);
			lang_freq[keys[i]] = parseInt(lang_freq[keys[i]]) / total * 100;
		}

		lang_freq = Object.fromEntries(Object.entries(lang_freq).sort(([,a],[,b]) => b-a));
		lang_freq = Object.fromEntries(Object.entries(lang_freq).slice(0, 7));

		let str = "";
		let current_percent = 0;
		for (let i = 0; i < Object.keys(lang_freq).length; i++) {
			let keys = Object.keys(lang_freq);
			str += `${colors[keys[i]]} ${current_percent}%, `;
			current_percent += lang_freq[keys[i]];
			str += `${colors[keys[i]]} ${current_percent}%, `;
		}

		lang_freq['Other'] = 100 - current_percent;

		str += `yellow ${current_percent}%, yellow 100%`;

		return [str, Object.keys(lang_freq), Object.values(lang_freq)];
	}

	// calculate the number of years since I was born with 9 decimal places of precision 
	// this can probably be unified with the header calculations and put in a store
	const birth = new Date("May 12, 2007");
	const milli_year = 31556952000;
	let years = $state("0")
	$effect(() => {
		const interval = setInterval(() => {
			years = ((Date.now() - birth.getTime()) / milli_year).toFixed(9)
		}, 100);
		return () => clearInterval(interval);
	})
</script>

<main>
	<div id="border">
		{#await languages()}
			<div id="placeholder"></div>
		{:then data}
			<div id="container">
				<div id="content">
					<h3>About me</h3>
					<p>I'm Tyler, a computer science major at unc charlotte, with plans to double major in math. 
					I've been self teaching myself computer science for {Math.floor(years-13)} years now, with my favorite subjects having been web dev, backend development, and robotics.
					On top of my interest in computer science, I also enjoy learning about history, mythology, and math, and I like to play video games in my free time.</p>
					<br/>
					<h3>Tech Stack</h3>
					<!-- Honeslty this part looks terrible I need to find a better solution -->
					<div id="stack-container">
						<div class="stack-component">
							<h4>Programming Languages</h4>
							<div class="entries">
								<p class="stack-entry">python</p>
								<p class="stack-entry">java</p>
								<p class="stack-entry">rust</p>
								<p class="stack-entry">C</p>
							</div>
						</div>
						<div class="stack-component">
							<h4>Development Tools</h4>
							<div class="entries">
								<p class="stack-entry">git</p>
								<p class="stack-entry">bash</p>
								<p class="stack-entry">linux</p>
								<p class="stack-entry">vim</p>
							</div>
						</div>
						<div class="stack-component">
							<h4>Frontend Programming</h4>
							<div class="entries">
								<p class="stack-entry">svelte</p>
								<p class="stack-entry">html</p>
								<p class="stack-entry">css</p>
							</div>
						</div>
					</div>
					<br/>
					<h3>Professional Documents</h3>
					<ul id="documents">
						<li><a href="/">Resume</a></li>
					</ul>
				</div>

				<div id="language-wheel" style="background: conic-gradient({data[0]});">
					<p id="language-title">Most Used Languages</p>
					<div id="key">
						{#each data[1] as language, index}
							<div id="key-container">
								<div id="language-circle" style="background: {colors[language]}"></div>
								{language} {data[2][index].toFixed(2)}%
							</div>
						{/each}
					</div>
				</div>
			</div>
		{/await}
	</div>
</main>

<style>
	main {
		color: #cad3f5;
	}

	p {
		color: inherit;
		text-align: left;
	}

	#documents {
		display: flex;
		justify-content: center;
		transform: translateX(-8vw);
		font-size: 18px;
	}

	a {
		color: #7dc4e4;
	}

	.entries {
		display: grid;
		grid-template-columns: 8vw 8vw;
		margin-left: 3.5vw;
		row-gap: 1vh;
		transform: translateY(-1.5vh);
	}

	.stack-entry {
		text-align: center;
		margin-bottom: 0px;
		background: #181926;
		border-radius: 50px;
		width: 5vw;
		border: 1px solid #6e738d;
	}

	.stack-component {
		border: 2px #6e738d solid;
		width: 20vw;
		height: 15vh;
	}

	#stack-container {
		display: grid;
		grid-template-columns: auto auto;
		row-gap: 2vw;
		width: 44vw;
		margin-left: 2vw;
	}

	#content {
		position: absolute;
		top: 0;
		padding-left: 10vw;
		padding-right: 42vw;
		opacity: 0%;
		animation: fade-in .3s ease-in-out forwards;
		border-width: 4px;
		text-align: center;
	}

	#language-title {
		position: fixed;
		top: 24vh;
		font-size: 18px;
	}

	#language-wheel {
		width: 10vw;
		height: 10vw;
		position: fixed;
		left: 67vw; 
		top: 30vh;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		opacity: 0;
		animation: fade-in .3s ease-in-out forwards;
	}

	#key-container {
		display: flex;
		justify-content: flex-start;
		align-items: flex-start;
		width: 20vw;
		transform: translateX(16vw);
		margin-bottom: 3px;
	}

	#language-circle {
		width: 20px;
		height: 20px;
		margin-right: 10px;
	}

	main {
		position: absolute;
		left: 10px;
		top: calc(23vh - 10px);
		height: calc(75vh);
		width: calc(100vw - 20px);
		display: flex;
		justify-content: center;
		align-items: center;
		--border-color: #f5a97f;
		--track-color: #24273a;
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

	@keyframes fade-in {
		from {opacity: 0%;}
		to {opacity: 100%;}
	}

	#border {
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
	}

	@keyframes border-animation {
		from {--red-percentage: 0%; --red-percentage-2: 100%;}
		to {--red-percentage: 50%; --red-percentage-2: 50%;}
	}
</style>
