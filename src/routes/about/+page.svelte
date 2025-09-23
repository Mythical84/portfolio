<script lang="ts">
</script>

<main>
	<div id="border">
		<div id="container">
		</div>
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

	#border {
		width: 100%;
		height: 100%;
		border-image: conic-gradient(cyan var(--red-percentage), #454545 var(--red-percentage), #454545 var(--red-percentage-2), cyan var(--red-percentage-2)) 1;
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
