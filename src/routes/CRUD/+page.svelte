<script lang="ts">
	interface Person {
		name: string;
		surname: string;
	}

	let people = $state([
		{ name: 'Guizhong', surname: 'Zhongli' },
		{ name: 'Raiden', surname: 'Ei' },
		{ name: 'Barbatos', surname: 'Venti' }
	]);

	let selected: Person = $state();
	let person = $state({ name: '', surname: '' });

	let prefix = $state('');
	const filteredPeople = $derived(
		prefix ? people.filter((p) => p.surname.toLowerCase().startsWith(prefix)) : people
	);

	$effect(() => {
		person = {
			name: selected?.name ?? '',
			surname: selected?.surname ?? ''
		};
	});

	function createPerson() {
		people = [...people, { ...person }];
		clearFields();
	}

	function updatePerson() {
		const index = people.findIndex((p) => p === selected);
		if (index !== 1) {
			people = people.map((p, i) => (i === index ? { ...person } : p));
		}
		selected = people[index];
	}

	function deletePerson() {
		if (selected) {
			people = people.filter((p) => p !== selected);
			clearFields();
			selected = undefined;
		}
	}

	function clearFields() {
		person = { name: '', surname: '' };
	}
</script>

<div class="surface-2 container">
	<div class="search">
		<label class="group">
			<span>Filter Prefix:</span>
			<input type="text" bind:value={prefix} />
		</label>
	</div>

	<label class="people">
		<span class="sr-only">Names:</span>
		<select bind:value={selected} size="5">
			{#each filteredPeople as person}
				<option value={person}>{person.surname}, {person.name}</option>
			{/each}
		</select>
	</label>

	<div class="details">
		<label class="group">
			<span>Name:</span>
			<input type="text" bind:value={person.name} />
		</label>
		<label class="group">
			<span>Surname:</span>
			<input type="text" bind:value={person.surname} />
		</label>
	</div>

	<div class="actions space-x">
		<button onclick={createPerson}>Create</button>
		<button onclick={updatePerson}>Update</button>
		<button onclick={deletePerson}>Delete</button>
	</div>
</div>

<style>
	.container {
		width: 500px;
		display: grid;
		grid-template-areas:
			'search.'
			'people details'
			'actions actions';
		grid-template-columns: 240px 1fr;
		grid-auto-rows: auto;
		gap: var(--size-3);
		padding: var(--size-3);

		.search {
			grid-area: search;
		}

		.group {
			display: grid;
			grid-template-columns: 2fr 1fr;
			align-items: baseline;
		}
	}

	.people {
		grid-area: people;
	}

	.details {
		grid-area: details;
		display: grid;
		grid-template-columns: auto 1fr;
		grid-template-rows: repeat(2, auto) 1fr;
		align-items: baseline;
		gap: var(--size-2);

		.group {
			display: contents;
		}
	}

	.actions {
		grid-area: actions;
	}
</style>
