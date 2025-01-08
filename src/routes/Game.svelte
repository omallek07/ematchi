<script lang="ts">
	import { onMount } from 'svelte';
	import Grid from './Grid.svelte';
	import Found from './Found.svelte';
	import Countdown from './Countdown.svelte';

	import { levels } from '$lib/levels';
	import type { Level } from '$lib/levels';
	import { shuffle } from '$lib/utils';

	const level = levels[0];

	let size: number = level.size;
	let grid: string[] = $state([]);

	let found: string[] = $state([]);
	let remaining: number = $state(level.duration);
	let playing: boolean = $state(true);

	function createGrid(level: Level) {
		const copy = level.emojis.slice();
		const pairs: string[] = [];

		for (let i = 0; i < size ** 2 / 2; i += 1) {
			const index = Math.floor(Math.random() * copy.length);
			const emoji = copy[index];
			copy.splice(index, 1);
			pairs.push(emoji);
		}

		pairs.push(...pairs);
		return shuffle(pairs);
	}

	function countdown() {
		const start = Date.now();
		let remainingAtStart = remaining;

		function loop() {
			if (!playing) return;
			requestAnimationFrame(loop);
			remaining = remainingAtStart - (Date.now() - start);

			if (remaining <= 0) {
				playing = false;
				// The game has been lost
				// stop the countdown
			}
		}
		loop();
	}

	onMount(countdown);

	if (typeof window !== 'undefined') {
		// stash the value...
		const initial = createGrid(level);

		// unset it...
		grid = [];

		$effect(() => {
			// ...and reset after we've mounted
			grid = initial;
		});
	}
</script>

<div class="game">
	<div class="info">
		<Countdown {remaining} duration={level.duration} />
	</div>
	<div class="grid-container">
		<Grid
			{grid}
			handleMatchingPair={(emoji: string) => {
				found = [...found, emoji];
			}}
			{found}
		/>
	</div>
	<div class="info">
		<Found {found} />
	</div>
</div>

<style>
	.game {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100%;
		font-size: min(1vmin, 0.5rem);
	}
	.info {
		width: 80em;
		height: 10em;
	}
	.grid-container {
		width: 80em;
		height: 80em;
	}
</style>
