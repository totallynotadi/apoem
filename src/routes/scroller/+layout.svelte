<script>
	import '../styles.css';
</script>

<div class="app">
	<!-- <Header /> -->

	<main>
		<slot />
	</main>

	<!-- <footer>
		<p>visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to learn SvelteKit</p>
	</footer> -->
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		min-width: 100vw;
		height: 100vh;
		width: 100vw;
	}

	main {
		height: 100%;
		width: 100%;
		max-width: 70rem;
		padding: 4rem 1rem;
		margin: 0 auto;
	}
	/* @media (max-width: 812px) { */
	@media (max-width: 775px) or (max-height: 400px) {
		main {
			height: 100%;
			width: 100%;
			max-width: 70rem;
			padding: 2rem 1rem;
			margin: 0 auto;
		}
	}
</style>
