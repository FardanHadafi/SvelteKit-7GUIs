<script lang="ts">
	type Options = 'one-way' | 'return';

	function getDate() {
		const date = new Date();
		let [year, month, day] = date
			.toLocaleDateString('en-US', { year: 'numeric', month: '2-digit', day: '2-digit' })
			.split('/');
		return `${year}-${month}-${day}`;
	}

	function handleSubmit(e: Event) {
		e.preventDefault();
		alert(`You have booked a ${selected} flight on ${startDate}`);
	}

	let selected = $state<Options>('one-way');
	let startDate = $state(getDate());
	let returnDate = $state(getDate());

	$inspect({ selected, startDate, returnDate });
</script>

<form onsubmit={handleSubmit} class="grid-gap">
	<select bind:value={selected}>
		<option value="one-way">one-way flight</option>
		<option value="return">return flight</option>
	</select>

	<label>
		<span class="sr-only">Select the start date:</span>
		<input type="date" bind:value={startDate} required min={getDate()} />
	</label>

	<label>
		<span class="sr-only">Select the return date:</span>
		<input
			type="date"
			bind:value={returnDate}
			required
			disabled={selected !== 'return'}
			min={getDate()}
		/>
	</label>

	<button type="submit" disabled={!startDate || (selected === 'return' && returnDate < startDate)}
		>Book</button
	>
</form>
