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

	$inspect(a, b); // will console.log when `count` or `message` change
	let resetTimeout: number;
</script>

<div class="grid">
	{#each grid as emoji, i}
		<Square
			{emoji}
			on:click={() => {
				clearTimeout(resetTimeout);
				if (a === -1 && b === -1) {
					a = i;
				} else if (b === -1) {
					b = i;
					if (grid[a] === grid[b]) {
						// correct
						handleMatchingPair(emoji);
						a = b = -1;
					} else {
						// incorrect
						resetTimeout = setTimeout(() => {
							a = b = -1;
						}, 1000);
					}
				} else {
					a = -1;
					b = i;
				}
			}}
			selected={a === i || b === 1}
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
	}
</style>
