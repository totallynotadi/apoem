<script>
	import { onMount } from 'svelte';

	const lines = [
		'Deep inside my heart was everything shattered,',
		'I realised, it was just a break, that mattered!',
		'on my way upto a library at the palm street',
		'I looked forward to just read and repeat!',
		'Staring at the books, I retrieved my lost connections,',
		'Oh, my heart was filled with ineffable emotions',
		'I searched through the genres like novels and Romance',
		"And grabbed 'the tale of 2 Cities'",
		'set up in England and france',
		'I took my cornered seat and flippped through the pages',
		'It was a story of love, of sacrifice',
		'In the background of revolution fought since ages',
		"I wondered at the end, 'Why Sidney Carton had to die?'",
		'I could do nothing at that moment, but just whine and cry',
		'The book ended',
		'it was not as easy as it seemed to be',
		'Coming back to reality',
		'that day, was not my cup of tea'
	];

	let linesContainer;
	let sectionContainer;
	let currentPixel = 0;

	const calculateScale = (index) => {
		let intendedPixel = index * 80;
		let difference = intendedPixel - currentPixel - (sectionContainer.clientHeight % 8);
		// let difference = intendedPixel - currentPixel;

		let scale = 1 - Math.min(Math.abs(difference) / 150, 0.2);
		// let scale = 1 - Math.min(Math.abs(difference) / 1000, 0.3);

		return scale;
	};

	const scrollCallback = (e) => {
		currentPixel = sectionContainer.scrollTop;
		let lineElements = Array.from(document.getElementsByClassName('line'));
		for (let [idx, line] of lineElements.entries()) {
			let scale = calculateScale(idx);
			// console.log(line, idx, scale);
			let transformScale = scale > 0.8 ? 1 : 0.8;

			line.style.transform = `scale(${transformScale})`;

			// line.style.marginTop = transformScale > 0.8 ? '-2rem' : '0rem';
			line.style.marginBottom = transformScale > 0.8 ? '1rem' : '0rem';

			line.style.filter = transformScale > 0.8 ? `blur(0px)` : `blur(${scale + 2}px)`;
		}
	};

	// $: currentPixel = linesContainer.scrollTop;

	onMount(() => {
		let lineElements = Array.from(document.getElementsByClassName('line'));
		let fullPadding = sectionContainer.clientHeight;
		// let sectionPadding = fullPadding - (lineElements.at(-1).clientHeight + lineElements.at(-2).clientHeight);
		let sectionPadding = fullPadding - lineElements.at(-1).clientHeight;

		linesContainer.style.padding = `0 0 ${sectionPadding}px 0`;
		// console.log(lineElements.at(-1).clientHeight);

		sectionContainer.addEventListener('scroll', scrollCallback, false);
		sectionContainer.addEventListener('touchmove', scrollCallback, false);
		sectionContainer.addEventListener('pointermove', scrollCallback, false);

		// let topMask = document.getElementById('blur-mask-top');
		// let bottomMask = document.getElementById('blur-mask-bottom');

		// topMask.style.WebkitMaskImage = 'linear-gradient(to top, black 97%, transparent);';
		// topMask.style.maskImage = 'linear-gradient(to top, black 97%, transparent);';
		// topMask.style.height = `${sectionContainer.clientHeight}px`;

		// bottomMask.style.maskImage = 'linear-gradient(to bottom, black 97%, transparent);';
		// bottomMask.style.WebkitMaskImage = 'linear-gradient(to bottom, black 97%, transparent);';
		// bottomMask.style.height = `${sectionContainer.clientHeight}px`;
	});
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<div class="stack fluid">
	<section bind:this={sectionContainer} class="stack-item">
		<div class="lines fluid-column" id="lines" bind:this={linesContainer}>
			{#each lines as line}
				<div class="line">
					{line}
				</div>
			{/each}
		</div>
	</section>
	<div id="blur-mask-top" class="stack-item fluid" />
	<div id="blur-mask-bottom" class="stack-item fluid" />
</div>

<style>
	section {
		height: 100%;
		overflow: scroll;
		-webkit-mask-image: linear-gradient(to bottom, black 90%, transparent);
		mask-image: linear-gradient(to bottom, rgb(0, 0, 0) 90%, transparent);
		margin-top: 2rem;
	}

	.lines {
		height: max-content;
		align-items: start;
	}

	.line {
		padding: 1.6rem 1rem;
		color: #2a2e2f;
		font-weight: 600;
		font-size: 2rem;
		transform-origin: 0% 0%;
	}

	/* iPhone */
	@media (max-width: 700px) or (max-height: 400px) {
		section {
			margin-top: 2rem;

			height: 100%;
			overflow: scroll;
			-webkit-mask-image: linear-gradient(to bottom, black 90%, gray);
			mask-image: linear-gradient(to bottom, rgb(0, 0, 0) 90%, gray);
		}

		.line {
			padding: 0.9rem;
			color: #2a2e2f;
			font-weight: 600;
			font-size: 1.4rem;
			transform-origin: 0% 0%;
		}
		.lines {
			/* min-height: 240vh; */
			/* padding: 0 0 60vh 0; */
			align-items: start;
			height: min-content;
		}
	}
	#blur-mask-top {
		-webkit-mask-image: linear-gradient(to top, black 90%, gray);
		mask-image: linear-gradient(to top, rgb(0, 0, 0) 90%, gray);
		pointer-events: none;
	}
	#blur-mask-bottom {
		-webkit-mask-image: linear-gradient(to bottom, black 97%, gray);
		mask-image: linear-gradient(to bottom, rgb(0, 0, 0) 97%, gray);
		pointer-events: none;
	}
</style>
