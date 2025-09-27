<script lang="ts">
	let elapsed = $state(0);
	let duration = $state(10);
	let interval: number | undefined;

	function clearCurrentInterval() {
		if (interval !== undefined) {
			clearInterval(interval);
			interval = undefined;
		}
	}

	function start() {
		clearCurrentInterval();
		const startTime = Date.now();
		const targetElapsed = elapsed;

		interval = setInterval(() => {
			const now: number = Date.now();
			const newElapsed = targetElapsed + (now - startTime) / 1000;

			if (newElapsed >= duration) {
				elapsed = duration;
				clearCurrentInterval();
			} else {
				elapsed = newElapsed;
			}
		}, 100);
	}

	function reset() {
		elapsed = 0;
		start();
	}

	$effect(() => {
		if (!duration) return;
		start();
		return () => clearInterval(interval);
	});
</script>

<div class="grid-gap">
	<div>
		<label>
			<span>Elapsed time:</span>
			<progress max={duration} value={elapsed}></progress>
		</label>
		<div>{elapsed.toFixed(1)}s</div>
	</div>

	<label>
		<span>Duration</span>
		<input type="range" bind:value={duration} min="1" max="10" />
	</label>
	<button onclick={reset}>Reset</button>
</div>

<style>
	.grid-gap {
		display: grid;
		gap: 1rem;
		max-width: 300px;
		padding: 1rem;
	}

	label {
		display: flex;
		flex-direction: column;
		gap: 0.5rem;
	}

	progress {
		width: 100%;
		height: 20px;
	}

	button {
		padding: 0.5rem 1rem;
		background: #007acc;
		color: white;
		border: none;
		border-radius: 4px;
		cursor: pointer;
	}

	button:hover {
		background: #005999;
	}
</style>
