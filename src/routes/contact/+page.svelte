<script lang="ts">
</script>

<main>
	<div id="border">
		<div id="container">
			<h3>I don't have a whole lot to put here</h3>
			<div id="links">
				<ul>
					<li><a href="mailto:tyler@mythical83.com">Send me an email</a></li>
					<li><a href="www.linkedin.com/in/tyler-artinger-139556397">reach me at linkedin</a></li>
					<li>Or fill out the form below</li>
				</ul>
			</div>
			<br/>
			<div id="form">
				<iframe title="questions" src="https://docs.google.com/forms/d/e/1FAIpQLSfN3IDhGemv2il4ipTJWGTI2XtdueTzFcgee29CcPpenHzuug/viewform?embedded=true" width="560" height="520" frameborder="0" marginheight="0" marginwidth="0">Loading…</iframe>
			</div>
		</div>
	</div>
</main>

<style>
	h3 {
		text-align: center;
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
		--border-color: #a6da95;
		--track-color: #24273a;
		color: #cad3f5;
	}

	a {
		color: #7dc4e4;
	}

	iframe {
		background: white;
	}

	#form {
		display: flex;
		justify-content: center;
		align-items: center;
	}

	#links {
		display: flex;
		align-items: center;
		justify-content: center;
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
