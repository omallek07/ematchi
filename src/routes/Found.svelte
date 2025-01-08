<script lang="ts">
	interface Props {
		found: string[];
	}

	import { getTwemojiUrl } from '$lib';

	let { found }: Props = $props();
</script>

<div class="found">
	{#each found as emoji (emoji)}
		<img alt={emoji} src={getTwemojiUrl(emoji)} />
	{/each}
</div>

<style>
	.found {
		display: flex;
		justify-content: center;
		align-items: center;
		gap: 0.5em;
	}
	img {
		width: 4em;
		height: 4em;
	}
</style>
