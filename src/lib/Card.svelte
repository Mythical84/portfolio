<script lang="ts">
	let { name } = $props()
</script>

<main>
	<p id="title">
		{name}
	</p>
</main>

<style>
	main {
		background: gray;
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
	}

	#title {
		font-weight: bold;
		font-size: 25px;
	}

	main:hover {
		transform: translate(-5px, -5px);
		box-shadow: 5px 5px black;
		transition: transform ease-in-out .2s, box-shadow ease-in-out .2s;
	}

	@keyframes fade-in {
		from {opacity: 0;}
		to {opacity: 1;}
	}
</style>
