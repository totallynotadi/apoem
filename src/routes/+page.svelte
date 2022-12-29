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
	let onTapCallback;

	let lineBounds;
	let intendedPixel;

	let currentLine = null;
	let currentPixel = 0;

	const calculateScale = (index, line) => {
		lineBounds = line.getBoundingClientRect();
		// console.log(line, lineBounds);

		// intendedPixel = index * 88;
		// intendedPixel = 104;
		// intendedPixel = lineElements.at(0).getBoundingClientRect().top;

		currentPixel = lineBounds.top;

		let difference = intendedPixel - currentPixel;

		let scale = 1 - Math.min(Math.abs(difference) / 150, 0.2);
		// let scale = 1 - Math.min(Math.abs(difference) / 10, 0.2);

		// if (scale > 0.8) {
		// 	console.log('intended: ', intendedPixel, 'difference: ', difference, 'scale: ', scale);
		// }

		return scale;
	};

	const scrollCallback = (e) => {
		let lineElements = Array.from(document.getElementsByClassName('line'));

		currentPixel = sectionContainer.scrollTop;
		for (let [idx, line] of lineElements.entries()) {
			let scale = calculateScale(idx, line);

			// currentLine = scale > 0.8 ? line : null;

			let transformScale = scale > 0.8 ? 1 : 0.8;

			line.style.transform = `scale(${transformScale})`;

			line.style.marginBottom = transformScale > 0.8 ? '1rem' : '0rem';

			line.style.filter = transformScale > 0.8 ? `blur(0px)` : `blur(${scale + 2}px)`;

			if (scale > 0.8) {
				currentLine = line;
			}
		}
	};

	onMount(() => {
		window.addEventListener(
			'contextmenu',
			(e) => {
				e.preventDefault();
			},
			true
		);
		let lineElements = Array.from(document.getElementsByClassName('line'));
		intendedPixel = lineElements.at(0).getBoundingClientRect().top;
		let fullPadding = sectionContainer.clientHeight;

		let sectionPadding = fullPadding - lineElements.at(-1).clientHeight;

		scrollCallback();

		linesContainer.style.padding = `0 0 ${sectionPadding}px 0`;

		document.addEventListener('touchstart', onTapCallback);
		document.addEventListener('mousedown', onTapCallback);

		sectionContainer.addEventListener('scroll', scrollCallback, false);

		onTapCallback = (e) => {
			// console.log('current line height', currentLine.clientHeight);
			if (currentLine !== null) {
				// console.log('scrolling by current line');
				sectionContainer.scrollBy({ top: currentLine.clientHeight, left: 0 });
			} else {
				// console.log('scrolling by top line');
				sectionContainer.scrollBy({ top: lineElements.at(0).clientHeight, left: 0 });
			}
			if (currentLine.innerHTML === lineElements.at(-1).innerHTML) {
				sectionContainer.scrollTop = 0;
			}
		};
	});
</script>

<svelte:head>
	<title>a poem</title>
	<meta name="description" content="it's nice innit?" />
</svelte:head>

<div class="stack fluid blur-mask-top" on:mousedown={onTapCallback}>
	<section bind:this={sectionContainer} class="stack-item">
		<div class="lines fluid-column" id="lines" bind:this={linesContainer}>
			{#each lines as line}
				<div class="line">
					{line}
				</div>
			{/each}
		</div>
	</section>
</div>

<style>
	section {
		height: 100%;
		overflow: hidden;
		-webkit-mask-image: linear-gradient(to bottom, black 90%, transparent);
		mask-image: linear-gradient(to bottom, rgb(0, 0, 0) 90%, transparent);
		padding-top: 2rem;
	}

	.lines {
		height: max-content;
		align-items: start;
	}

	.line {
		padding: 1.6rem 1rem;
		color: #2a2e2f;
		font-weight: 700;
		font-size: 2.4rem;
		transform-origin: 0% 0%;
		user-select: none;
	}

	@media (max-width: 700px) or (max-height: 400px) {
		section {
			padding-top: 2rem;
			height: 100%;
			overflow: hidden;
			-webkit-mask-image: linear-gradient(to bottom, black 90%, gray);
			mask-image: linear-gradient(to bottom, rgb(0, 0, 0) 90%, gray);
		}

		.line {
			padding: 0.9rem;
			color: #2a2e2f;
			font-weight: 700;
			font-size: 1.2rem;
			transform-origin: 0% 0%;
		}
		.lines {
			align-items: start;
			height: min-content;
		}
	}
	.blur-mask-top {
		-webkit-mask-image: linear-gradient(to top, black 94%, transparent);
		mask-image: linear-gradient(to top, rgb(0, 0, 0) 94%, transparent);
	}
</style>
