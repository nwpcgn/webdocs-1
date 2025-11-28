---
title: Battleship
sidebar_label: Battle 2
sidebar_position: 71
---






## Ship-Arranger

### Svelte Component



```html title="c.svelte"
<script>
	import { onMount } from 'svelte'
	let {
		width = 10,
		height = 10,
		size = 50,
		ships = [
			{ name: 'battleship', size: 4, cnt: 1 },
			{ name: 'cruiser', size: 3, cnt: 2 },
			{ name: 'destroyer', size: 2, cnt: 3 },
			{ name: 'submarine', size: 1, cnt: 4 }
		],
		colorScheme = ['#fff', '#0ea5e9', '#bae6fd'],
		playerId = 'player'
	} = $props()

	let map = $state([])
	let ships = $state([])
	const map1 = new Map()

	function init() {
		map = Array(height)
			.fill()
			.map(() => Array(width).fill(0))
	}

	function createShip(size, name) {
		const ship = []
		const dir = Math.floor(Math.random() * 2)
		let x = Math.floor(Math.random() * (height - size + 1))
		let y = Math.floor(Math.random() * (width - size + 1))

		for (let i = 0; i < size; i++) {
			ship.push([x, y])
			if (dir === 0) x++
			else y++
		}
		// console.log({ ship, name })
		return ship
	}

	function isRegionFree(ship) {
		return ship.every(([x, y]) => map[x][y] === 0)
	}

	function placeShip(ship) {
		const [startX, startY] = ship[0]
		const [finX, finY] = ship[ship.length - 1]

		for (let x = startX - 1; x <= finX + 1; x++) {
			for (let y = startY - 1; y <= finY + 1; y++) {
				if (x >= 0 && y >= 0 && x < height && y < width) {
					map[x][y] =
						x >= startX && x <= finX && y >= startY && y <= finY ? 1 : 2
				}
			}
		}
	}

	function arrange(ships) {
		const newMap = map

		for (const { size, cnt, name } of ships) {
			let placed = 0
			let overflow = 0

			while (placed < cnt) {
				const ship = createShip(size, name)
				map1.set(`${name}_${placed}`, ship)
				if (isRegionFree(ship)) {
					placeShip(ship)
					placed++
				}

				overflow++

				if (overflow > 100) {
					throw new Error(
						'The number of ships is too high or dimensions of board are too small'
					)
				}

				// console.log({ ship, name })
			}
		}

		return newMap
	}

	function run() {
		init()
		arrange(ships)
		console.log([...map1])
	}

	onMount(() => {
		init()
	})
</script>

<div id={playerId}>
	<div class="arena" style="--s: {width}; --w: {size}px; --h: {size}px;">
		{#each map as row, i}
			{#each row as cell, j}
				<button
					class="col-{cell} transition-all ease-in-out duration-300 active:opacity-40"
					onclick={() => {
						console.log({
							id: i * width + j,
							cell,
							r: i + 1,
							c: j + 1,
							i,
							j
						})
					}}><span class="sr-only">{i * width + j}</span></button>
			{/each}
		{/each}
	</div>
</div>
<div class="flex justify-center py-2 gap-1">
	<button class="btn btn-neutral" onclick={run}>Run</button>
</div>

<style>
	.arena {
		--s: 10;
		--w: 40px;
		--h: 40px;
		--color-0: rgba(255, 255, 255, 1);
		--color-1: rgba(14, 165, 233, 1);
		--color-2: rgba(186, 230, 253, 1);
		padding: 1px;
		gap: 1px;
		display: inline-grid;
		grid-template-columns: repeat(var(--s), minmax(0, 1fr));
		grid-template-rows: repeat(var(--s), minmax(0, 1fr));
		background-color: var(--gray-200);
	}
	.arena > * {
		width: var(--w);
		height: var(--h);
	}
	.col-0 {
		background-color: var(--color-0);
	}
	.col-1 {
		background-color: var(--color-1);
	}
	.col-2 {
		background-color: var(--color-2);
	}
</style>
```


---

## Helper Functions


```javascript
const shipPosition = [ [ 0, 1 ], [ 1, 1 ], [ 2, 1 ], [ 3, 1 ] ]

function checkUniformity(arr) {
  const firstEqual = arr.every(pos => pos[0] === arr[0][0]);
  const secondEqual = arr.every(pos => pos[1] === arr[0][1]);
  return { firstEqual, secondEqual };
}

const result = checkUniformity(shipPosition);
console.log(result);
// { firstEqual: false, secondEqual: true }
```


---

Um zu überprüfen, ob entweder der erste Wert oder der zweite Wert in allen Unterarrays von `shipPosition` gleich ist, kannst du die JavaScript-Methode `every` verwenden. Hier ist eine Lösung:

### Für den ersten Wert (Index 0):
```javascript
const allFirstEqual = shipPosition.every(pos => pos[0] === shipPosition[0][0]);
console.log(allFirstEqual); // true oder false
```

### Für den zweiten Wert (Index 1):
```javascript
const allSecondEqual = shipPosition.every(pos => pos[1] === shipPosition[0][1]);
console.log(allSecondEqual); // true oder false
```

### Zusammen in einer Funktion:
Falls du beide Prüfungen auf einmal machen möchtest:
```javascript
function checkUniformity(arr) {
  const firstEqual = arr.every(pos => pos[0] === arr[0][0]);
  const secondEqual = arr.every(pos => pos[1] === arr[0][1]);
  return { firstEqual, secondEqual };
}

const result = checkUniformity(shipPosition);
console.log(result);
// { firstEqual: false, secondEqual: true }
```

### Erklärung
- `every` prüft, ob alle Elemente im Array die Bedingung erfüllen.
- `pos[0] === shipPosition[0][0]` prüft, ob der erste Wert in allen Unterarrays gleich dem ersten Wert des ersten Unterarrays ist.
- Ähnliches gilt für `pos[1] === shipPosition[0][1]`.

Hier wird das Ergebnis entweder `true` oder `false` sein, je nachdem, ob alle Werte gleich sind.