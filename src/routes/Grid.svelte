<script lang="ts">
	import Square from './Square.svelte';
	interface Props {
		handleMatchingPair: (emoji: string) => void;
		grid: string[];
		found: string[];
	}
	let { handleMatchingPair, grid, found }: Props = $props();
	let a: number = $state(-1);
	let b: number = $state(-1);
	let resetTimeout: number;
</script>

<div class="grid">
	{#each grid as emoji, i}
		<Square
			{emoji}
			on:click={() => {
				if (a > -1 && b > -1) {
					clearTimeout(resetTimeout);
					a = i;
					b = -1;
				} else if (a > -1) {
					b = i;
					if (grid[a] === grid[b]) {
						// correct
						handleMatchingPair(emoji);
					} else {
						// incorrect
						resetTimeout = setTimeout(() => {
							a = b = -1;
						}, 1000);
					}
				} else {
					a = i;
				}
			}}
			group={grid.indexOf(emoji) === i ? 'a' : 'b'}
			selected={a === i || b === i}
			found={found.includes(emoji)}
		/>
	{/each}
</div>

<style>
	.grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		grid-template-rows: repeat(4, 1fr);
		grid-gap: 0.5em;
		height: 100%;
		perspective: 100vw;
	}
</style>
