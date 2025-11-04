<script lang="ts">
	let { name, click } = $props()
	let card: any;

	export function getDomRect() {
		return card.getBoundingClientRect();
	}
	
	export function getName() {
		return name;
	}
</script>

<main bind:this={card}>
	<button onclick={click}>
		<p id="title">
			{name.replaceAll('-', ' ')}
		</p>
	</button>
</main>

<style>
	main {
		background: #181926;
		width: 23vw;
		height: 20vh;
		border-radius: 10px;
		overflow: scroll;
		opacity: 0;
		animation: fade-in ease-in-out .4s forwards;
		animation-delay: .1s;
		transition: transform ease-in-out .2s, box-shadow ease-in-out .2s;
		display: flex;
		align-items: center;
		justify-content: center;
		border: 4px solid #6e738d;
	}

	button {
		width: 100%;
		height: 100%;
		border: none;
		background: none;
	}
	
	#title {
		font-weight: bold;
		font-size: 25px;
		color: #cad3f5;
	}

	main:hover {
		transform: translate(-5px, -5px);
		box-shadow: 5px 5px black;
		transition: transform ease-in-out .2s, box-shadow ease-in-out .2s;
		border-color: #b7bdf8;
	}

	@keyframes fade-in {
		from {opacity: 0;}
		to {opacity: 1;}
	}
</style>
