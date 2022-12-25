<script>
	let line = null;
	$: {
		let rect = line?.getBoundingClientRect();
		console.log(rect);
	}
</script>

<div class="line" bind:this={line}>
	<slot />
</div>

<style>
	.line {
		padding: 1rem;
		color: white;
		font-weight: 600;
		font-size: 2rem;
	}
	@media (max-width: 700px) or (max-height: 400px) {
		.line {
			padding: 0.9rem;
			color: white;
			font-weight: 600;
			font-size: 1.4rem;
		}
	}
</style>
