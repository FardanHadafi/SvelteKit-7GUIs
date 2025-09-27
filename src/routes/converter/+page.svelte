<script lang="ts">
	class Temperature {
		#C = $state(0);
		#F = $state(0);

		get C() {
			return this.#C;
		}

		set C(C) {
			this.#C = C;
			this.#F = C * (9 / 5) + 32;
		}

		get F() {
			return this.#F;
		}

		set F(F) {
			this.#F = F;
			this.#C = (F - 32) * (5 / 9);
		}
	}

	const temperature = new Temperature();
</script>

<div>
	<input type="number" bind:value={temperature.C} /> Celcius =
	<input type="number" bind:value={temperature.F} /> Fahrenheit
</div>
