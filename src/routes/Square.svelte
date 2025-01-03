<script lang="ts">
	export let emoji: string;
	export let selected: boolean;
	export let found: boolean;
</script>

<div class="square" class:flipped={selected}>
	<button aria-label="square" on:click></button>

	{#if !found}
		<span>{emoji}</span>
	{/if}
</div>

<style>
	.square {
		background-color: yellow;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	button {
		position: absolute;
		width: 100%;
		height: 100%;
	}
	.flipped button {
		background-color: red;
	}
	span {
		pointer-events: none;
		font-size: 5em;
	}
</style>
