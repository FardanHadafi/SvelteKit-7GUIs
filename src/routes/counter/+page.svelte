<script lang="ts">
	let count = $state(0);
</script>

<input type="number" bind:value={count} readonly />
<button onclick={() => count++}>Increment</button>
<button onclick={() => count--}>Decrement</button>
<button onclick={() => (count = 0)}>Reset</button>
