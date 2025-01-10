<script lang="ts">
	import { onMount } from 'svelte';
	import Grid from './Grid.svelte';
	import Found from './Found.svelte';
	import Countdown from './Countdown.svelte';

	import type { Level, State } from '$lib';

	import { shuffle } from '$lib/utils';

	let {
		setGameStatus
	}: {
		setGameStatus: (status: State) => void;
	} = $props();

	let size: number = $state(0);
	let grid: string[] = $state([]);
	let duration: number = $state(0);
	let found: string[] = $state([]);
	let remaining: number = $state(0);
	let playing: boolean = $state(false);

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

	export function start(level: Level) {
		console.log('level', level);
		size = level.size;
		grid = createGrid(level);
		remaining = duration = level.duration;
		resume();
	}

	function resume() {
		playing = true;
		setGameStatus('playing');
		countdown();
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
				setGameStatus('lost');
			}
		}
		loop();
	}

	// if (typeof window !== 'undefined') {
	// 	// stash the value...
	// 	const initial = createGrid(level);

	// 	// unset it...
	// 	grid = [];

	// 	$effect(() => {
	// 		// ...and reset after we've mounted
	// 		grid = initial;
	// 	});
	// }
</script>

<div class="game" style="--size: {size}">
	<div class="info">
		{#if playing}
			<Countdown
				toggleCountdown={() => {
					setGameStatus('paused');
				}}
				{remaining}
				{duration}
			/>
		{/if}
	</div>
	<div class="grid-container">
		<Grid
			{grid}
			handleMatchingPair={(emoji: string) => {
				found = [...found, emoji];
				if (found.length === (size * size) / 2) {
					setGameStatus('won');
				}
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
