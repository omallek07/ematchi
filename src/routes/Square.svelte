<script lang="ts">
	export let emoji: string;
</script>

<div class="square">
	<span>{emoji}</span>
</div>

<style>
	.square {
		background-color: yellow;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	span {
		font-size: 5em;
	}
</style>
