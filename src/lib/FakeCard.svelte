<script lang="ts">
	import { Base64 } from "js-base64";
	import { compile } from "mdsvex";
	import colors from "./colors.json"

	let { cards, clicked_index, left, top, close } = $props();

	// this token only has read perms don't worry
	let token = "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";

	// get readme markdown and return as html that sveltekit can render
	async function get_readme() {
		let repo = cards[clicked_index]?.getName()
		let headers = new Headers();
		headers.append("Authorization", "Bearer " + token);
		headers.append("X-GitHub-Api-Version", "2022-11-28");
		let readme = await fetch(`https://api.github.com/repos/mythical84/${repo}/contents/README.md`, {
			method: 'GET',
			headers: headers
		});
		let json = await readme.json();
		if (json.message == 'Not Found') return "";
		let md = Base64.decode(json.content.replace("\n", ""))
		let html = await compile(md).then(x =>
			x.code.replace(/>{@html `<code class="language-/g, '><code class="language-')
			.replace(/<\/code>`}<\/pre>/g, '</code></pre>'));

		let style = `
<style>
	a {
		color: #91d7e3;
	}

	h1 {
		position: sticky;
		top: 0;
		background: #181926;
	}
</style>
		`;
		return style + "\n" + html;
	}

	async function get_languages() {
		let repo = cards[clicked_index]?.getName()
		let headers = new Headers();
		headers.append("Authorization", "Bearer " + token);
		headers.append("X-GitHub-Api-Version", "2022-11-28");
		let languages = await fetch(`https://api.github.com/repos/mythical84/${repo}/languages`, {
			method: 'GET',
			headers: headers
		});

		let json = await languages.json();

		let percents: number[] = [];
		let names: string[] = [];
		let total = 0;

		for (const name in json) {
			names.push(name);
			percents.push(parseInt(json[name]))
			total += parseInt(json[name])
		}

		for (let i = 0; i < percents.length; i++) {
			percents[i] = percents[i] / total * 100;
		}

		let str = "";
		let current_percent = 0;
		for (let i = 0; i < percents.length; i++) {
			str += `${colors[names[i]]} ${current_percent}%, `;
			current_percent += percents[i];
			str += `${colors[names[i]]} ${current_percent}%, `;
		}

		return [str.substring(0, str.length - 2), names, percents];
	}

	async function get_data() {
		return [await get_readme(), await get_languages()]
	}

	async function get_file() {
		let repo = cards[clicked_index]?.getName()
		let headers = new Headers();
		headers.append("Authorization", "Bearer " + token);
		headers.append("X-GitHub-Api-Version", "2022-11-28");
		let languages = await fetch(`https://api.github.com/repos/mythical84/${repo}/tags`, {
			method: 'GET',
			headers: headers
		});

		let json = await languages.json();

		return json;
	}
</script>

<main>
	{#await get_data()}
		<div id="placeholder"></div>
	{:then data}
		<div id="selected_card" style="left: {left}px; top: calc({top}px - 22vh - 12px);">
			<p id="title">{cards[clicked_index]?.getName()}</p>
			{#if data[0] == ""}
				<div id="markdown"><h2>Sorry, but I haven't written a markdown file for this repo yet :(</h2></div>
			{:else}
				<div id="markdown">{@html data[0]}</div>
			{/if}
			<button id="close" onclick={close}>X</button>
			<div id="language-wheel" style="background: conic-gradient({data[1][0]});">
				<p id="language-title">Languages</p>
				<div id="key">
					{#each data[1][1] as language, index}
						<div id="key-container">
							<div id="language-circle" style="background: {colors[language]}"></div>
							{language} {data[1][2][index].toFixed(2)}%
						</div>
					{/each}
				</div>
				<div id='release'>
					<div id="file-container">
						{#await get_file()}
							<div id="placeholder"></div>
						{:then releases}
							<!--TODO: make this go away if there are no releases-->
							<div id="release-title">Releases</div>
							<ul>
							{#each releases as release}
								<li><a href={`https://github.com/Mythical84/${cards[clicked_index]?.getName()}/releases/tag/${release.name}`}>{release.name}</a></li>
							{/each}
							</ul>
						{/await}
					</div>
				</div>
			</div>
		</div>
	{/await}
</main>

<style lang="scss">
	#release {
		width: 20vw;
		height: 10vw;
		position: fixed;
		top: 55vh;
		left: 68vw;
	}

	#release-title {
		font-size: 18px;
	}

	#language-wheel {
		width: 10vw;
		height: 10vw;
		position: fixed;
		left: 67vw; 
		top: 30vh;
		border-radius: 50%;
		animation: fade-in .5s ease-in-out forwards .3s;
		opacity: 0%;
		display: flex;
		justify-content: center;
	}

	#language-title {
		position: fixed;
		top: 25vh;
		font-size: 18px;
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

	#selected_card {
		height: 21vh;
		width: 24vw;
		animation: expand .5s ease-in-out forwards;
		z-index: 100;
		position: absolute;
		background: #181926;
		border: 4px solid #6e738d;
		color: #cad3f5;
		overflow: scroll;
	}

	#title {
		animation: fade-out .25s ease-in-out forwards;
		opacity: 100%;
		font-size: 25px;
		font-weight: bold;
		text-align: center;
		vertical-align: middle;
		line-height: 20vh;
	}

	#markdown {
		position: absolute;
		top: 0;
		padding-left: 10vw;
		padding-right: 42vw;
		opacity: 0%;
		animation: fade-in .5s ease-in-out forwards .3s;
		border-width: 4px;
	}

	#close {
		width: 50px;
		height: 50px;
		font-size: 40px;
		position: fixed;
		opacity: 0;
		top: 0;
		left: 100%;
		transform: translate(-60px, 22vh);
		animation: fade-in .5s ease-in-out forwards .3s;
		color: #cad3f5;
		background: none;
		border: none;
	}

	#close:hover {
		color: #a5adcb;
		cursor: pointer;;
	}

	/* This entire animation is made up of magic numbers idk how tf it works */
	@keyframes expand {
		from {width: 23vw; height: 20vh}
		to {
			width: calc(100vw - 28px); 
			height: 76vh;
			/* idk why a negative top value works but it's making me suicidal */ 
			/* I now know why a negative top value works but i think it's stupid so I'm  the above comment */
			top: calc(-2vh - 4px);
			left: 0px;
		}
	}

	@keyframes fade-in {
		from {opacity: 0%}
		to {opacity: 100%}
	}

	@keyframes fade-out {
		from {opacity: 100%}
		to {opacity: 0%}
	}

</style>
