<script lang="ts">
	import { getTwemojiUrl, send } from '$lib';
	interface Props {
		emoji: string;
		selected: boolean;
		found: boolean;
		group: 'a' | 'b';
	}

	let { emoji, selected, found, group }: Props = $props();
</script>

<div class="square" class:flipped={selected || found}>
	<button aria-label="square" on:click />
	<div class="background" />

	{#if !found}
		<img
			out:send={{
				key: `${emoji}:${group}`
			}}
			alt={emoji}
			src={getTwemojiUrl(emoji)}
		/>
	{/if}
</div>

<style>
	.square {
		display: flex;
		justify-content: center;
		align-items: center;
		transition: transform 0.4s;
		transform-style: preserve-3d;
	}
	button {
		position: absolute;
		width: 100%;
		height: 100%;
		backface-visibility: hidden;
		background: #eee;
		border: 0;
		border-radius: 1em;
		font-size: inherit;
	}
	.background {
		position: absolute;
		background: white;
		border: 0.5em solid #eee;
		transform: rotateY(180deg);
		backface-visibility: hidden;
		width: 100%;
		height: 100%;
	}
	.flipped {
		transform: rotateY(180deg);
	}
	img {
		height: 6em;
		width: 6em;
		pointer-events: none;
		transform: rotateY(180deg);
		backface-visibility: hidden;
	}
</style>
