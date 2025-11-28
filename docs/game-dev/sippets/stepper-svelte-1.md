---
title: Stepper I
sidebar_position: 30
sidebar_label: Stepper I
---


## Clamp Stepper


```html title="stepper.svelte"
<script lang="ts">
	class Stepper {
		#min: number = $state(0)
		#max: number = $state(0)
		#step: number = $state(1)
		#current: number = $state(0)
		constructor(min, max, step = 1, initial = min) {
			this.#min = min
			this.#max = max
			this.#step = step
			this.#current = this.clamp(initial)
		}

		get min() {
			return this.#min
		}
		get max() {
			return this.#max
		}
		get step() {
			return this.#step
		}
		get current() {
			return this.#current
		}
		set min(value: number) {
			this.#min = this.clamper(value, 0, 10)
		}
		set max(value: number) {
			this.#max = this.clamper(value, this.#min, 100)
		}
		set step(value: number) {
			this.#step = this.clamper(value, 1, 10)
		}

		clamp(v) {
			return Math.min(Math.max(v, this.#min), this.#max)
		}

		clamper(value, min, max) {
			return Math.min(Math.max(value, min), max)
		}

		up() {
			this.#current = this.clamp(this.#current + this.#step)
			return this.#current
		}

		down() {
			this.#current = this.clamp(this.#current - this.#step)
			return this.#current
		}

		set(v) {
			this.#current = this.clamp(v)
			return this.#current
		}
	}

	let { views = [] } = $props()
	const steps = new Stepper(0, views.length - 1, 1)
</script>

<div>
	<fieldset>
		<legend>Stepper Component</legend>

		<div class="gap-2 grid grid-cols-3">
			<div>
				<label for="f-min">min:</label>
				<input
					type="number"
					id="f-min"
					name="min"
					bind:value={steps.min}
					placeholder="min" />
			</div>
			<div>
				<label for="f-max">max:</label>
				<input
					type="number"
					id="f-max"
					name="max"
					bind:value={steps.max}
					placeholder="max" />
			</div>
			<div>
				<label for="f-step">step:</label>
				<input
					type="number"
					id="f-step"
					name="step"
					bind:value={steps.step}
					placeholder="step" />
			</div>

			<div>
				<label for="f-current">current:</label>
				<input
					type="number"
					id="f-current"
					name="current"
					bind:value={steps.current}
					placeholder="current" />
			</div>

			<span></span>
			<div>
				<label>Action</label>

				<button
					disabled={steps.current == steps.min}
					onclick={() => steps.down()}
					type="button">&#8592;</button>
				<button
					disabled={steps.current == steps.max}
					onclick={() => steps.up()}
					type="button">&#8594;</button>
				<span class="button">{steps.current.toString().padStart(2, '0')}</span>
			</div>
		</div>
	</fieldset>
</div>
<header>
	<h1>{views[steps.current]?.title}</h1>
	<p>{views[steps.current]?.text}</p>
</header>
```


