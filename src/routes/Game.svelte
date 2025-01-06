<script lang="ts">
	import Grid from './Grid.svelte';
	import { levels } from '$lib/levels';
	import type { Level } from '$lib/levels';
	import { shuffle } from '$lib/utils';

	const level = levels[0];

	let size: number = level.size;
	let grid: string[] = createGrid(level);

	let found: string[] = $state([]);

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
</script>

<div class="game">
	<div class="info"></div>
	<div class="grid-container">
		<Grid
			{grid}
			handleMatchingPair={(emoji: string) => {
				found = [...found, emoji];
			}}
			{found}
		/>
	</div>
	<div class="info"></div>
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
		background-color: purple;
	}
	.grid-container {
		width: 80em;
		height: 80em;
		background: teal;
	}
</style>
