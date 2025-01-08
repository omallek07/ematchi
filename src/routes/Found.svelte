<script lang="ts">
	interface Props {
		found: string[];
	}

	import { getTwemojiUrl, recieve } from '$lib';

	let { found }: Props = $props();
</script>

<div class="found">
	{#each found as emoji (emoji)}
		<div class="pair">
			<img
				in:recieve={{
					key: `${emoji}:a`
				}}
				alt={emoji}
				src={getTwemojiUrl(emoji)}
			/>
			<img
				in:recieve={{
					key: `${emoji}:b`
				}}
				alt={emoji}
				src={getTwemojiUrl(emoji)}
			/>
		</div>
	{/each}
</div>

<style>
	.found {
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.5em;
		filter: drop-shadow(0.2em 0.2em 0.5em rgba(0, 0, 0, 0.2));
	}

	.pair {
		display: flex;
		justify-content: center;
		align-items: center;
		width: 4em;
		height: 4em;
		background: white;
		padding: 1em;
		border-radius: 50%;
	}
	img {
		position: absolute;
		width: 2em;
		height: 2em;
	}
</style>
