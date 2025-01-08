<script lang="ts">
	interface Props {
		remaining: number;
		duration: number;
	}

	let { remaining, duration }: Props = $props();
</script>

<div class="countdown">
	<button>
		<img alt="Pause Countdown" src="pause_circle.svg" />
	</button>
	<div class="duration">
		<div class="remaining" style="--p: {remaining / duration}" />
	</div>
</div>

<style>
	.countdown {
		display: flex;
		align-items: center;
		gap: 1em;
	}
	button {
		font-size: 8em;
		width: 1em;
		height: 1em;
		background: transparent;
		border: none;
	}
	img {
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
	}
	.duration {
		flex: 1;
		height: 1em;
		background-color: #ccc;
	}
	.remaining {
		position: absolute;
		height: 100%;
		background-color: #999;
		width: calc(100% * var(--p));
	}
</style>
