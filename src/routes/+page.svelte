<script lang="ts">
	import '../styles.css';
	import Game from './Game.svelte';
	import Modal from './Modal.svelte';

	import { levels } from '$lib';
	import type { State } from '$lib';

	let state: State = 'waiting';
	let game: Game;

	function setGameStatus(status: State) {
		state = status;
	}
</script>

<Game bind:this={game} {setGameStatus} />

{#if state !== 'playing'}
	<Modal>
		<header>
			<h1>e<span>match</span>i</h1>
			<p>the emoji matching game</p>
		</header>
		{#if state === 'won' || state === 'lost'}
			<p>you {state} the game!</p>
		{:else if state === 'paused'}
			<p>game paused</p>
		{:else if state === 'waiting'}
			<p>Choose a level</p>
		{/if}
		<div class="buttons">
			{#if state === 'paused'}
				<button onclick={() => setGameStatus('playing')}>resume</button>
				<button onclick={() => setGameStatus('lost')}>quit</button>
			{:else}
				{#each levels as level}
					<button
						onclick={() => {
							game.start(level);
						}}>{level.label}</button
					>
				{/each}
			{/if}
		</div>
	</Modal>
{/if}

<style>
	h1 {
		font-size: 4em;
	}
	h1 span {
		color: purple;
	}
	p {
		font-family: Grandstander;
	}
</style>
