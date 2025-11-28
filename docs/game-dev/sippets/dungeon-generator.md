---
title: Dungeon-Generator-1
sidebar_position: 20
sidebar_label: Dungeon-Generator-1
---





# Dungeon Map with ROT.JS



## Generator

```typescript title="dungeonGenerator.ts"
import * as ROT from 'rot-js'
type Door = { x: number; y: number }
export type Room = {
	x1: number
	y1: number
	x2: number
	y2: number
	centerX: number
	centerY: number
	roomId: number
	doors: Door[]
}

export function generateDungeon(width = 40, height = 20) {
	const digger = new ROT.Map.Digger(width, height)
	const tileMap: string[][] = Array.from({ length: height }, () =>
		Array(width).fill(' ')
	)
	const freeCells: string[] = []
	const rooms: Room[] = []

	const digCallback = (x, y, value) => {
		tileMap[y][x] = value ? '#' : '.'
		if (value) return
		const key = `${x},${y}`
		freeCells.push(key)
	}

	const createRooms = () => {
		digger.getRooms().forEach((room, roomId) => {
			const result = convertRoom(room)
			const [centerX, centerY] = room.getCenter()
			rooms.push({ ...result, centerX, centerY, roomId })
		})
	}
	const convertRoom = (r) => {
		const doors = Object.keys(r._doors).map((key) => {
			const [x, y] = key.split(',').map(Number)
			return { x, y }
		})

		doors.forEach(({ x, y }) => {
			// const key = `${x},${y}`
			tileMap[y][x] = 'D'
		})

		return {
			x1: r._x1,
			y1: r._y1,
			x2: r._x2,
			y2: r._y2,
			doors
		}
	}
	const generateBoxes = (freeCells) => {
		for (let i = 0; i < 10; i++) {
			const index = Math.floor(ROT.RNG.getUniform() * freeCells.length)
			const key = freeCells.splice(index, 1)[0]
			const [x, y] = key.split(',')
			tileMap[y][x] = '*'
		}
		// console.log(freeCells);
	}

	digger.create(digCallback)
	createRooms()
	generateBoxes(freeCells)

	return { rooms, freeCells, tileMap }
}

```


## Usage



```html title="Dungeon.svelte"
<script lang="ts">
	import './dungeon.scss'
	import Loading from './Loading.svelte'
	import {
		generateDungeon,
		sleep,
		randNum,
		type Room
	} from './dungeonGenerator'

	let elem: HTMLDivElement | null = $state()
	let frame = $state({ w: 0, h: 0 })
	let cols = $state.raw(50)
	let rows = $state.raw(40)
	let scale = $state.raw(0.9)
	let maxH = $derived(`${Math.floor((frame.h / rows) * scale)}`)
	let maxW = $derived(`${Math.floor((frame.w / cols) * scale)}`)
	let gStyle = $derived(`--gg-gap: 1px; --gg-size: ${Math.min(maxH, maxW)}px;`)

	let grid: string[][] | null = $state()
	let hero = $state({ x: 0, y: 0 })
	let rooms: Room[] | null = $state()
	let freeCells: string[] = $state()

	const getDungeon = async () => {
		const data = generateDungeon(cols, rows)
		grid = data.tileMap
		freeCells = data.freeCells
		rooms = data.rooms
		const randId = randNum(0, rooms.length - 1)
		const { centerX, centerY } = rooms[randId]

		hero = { x: centerX, y: centerY }
		await sleep(1200)
		return data
	}
	const recreate = () => {
		promise = getDungeon()
	}

	let promise = $state(getDungeon())
</script>

<section
	class="nwp page center"
	bind:clientWidth={frame.w}
	bind:clientHeight={frame.h}>
	<aside class="absolute top-4 right-4">
		<nav class="grid gap-2">
			<button onclick={recreate}>Button</button>
			<div>
				<span>Rooms:</span>
				<span>{rooms.length}</span>
			</div>
		</nav>
	</aside>

	{#await promise}
		<Loading></Loading>
	{:then value}
		{#if value?.tileMap}
			<div class="border-accent-500 border">
				<table class="rogue" style={gStyle}>
					{#each value.tileMap as row, y}
						<tr>
							{#each row as col, x}
								<td data-col={col} data-x={x} data-y={y}>
									<div>
										{#if col === '#'}
											<div class="opacity-0">
												<span class="sr-only">{col}</span>
											</div>
										{:else if col === '.'}
											<div class="bg-accent-400">
												<span class="sr-only">{col}</span>
											</div>
										{:else if col === 'D'}
											<div class="bg-accent-600">
												<span class="sr-only">{col}</span>
											</div>
										{:else}
											<div class="text-accent-800 bg-accent-400">
												<span style="font-size: calc(var(--gg-size) * 0.6);"
													>{col}</span>
											</div>
										{/if}
										{#if hero.x === x && hero.y === y}
											<div class="player"></div>
										{/if}
									</div>
								</td>
							{/each}
						</tr>
					{/each}
				</table>
			</div>
		{:else if value}
			<div class="flex justify-center">
				<pre>{JSON.stringify(value, null, 2)}</pre>
			</div>
		{/if}
	{:catch error}
		<div class="text-center">
			<h2>Error</h2>
		</div>
	{/await}
</section>
```



### Style

```css title="dungeon.scss"
table.rogue {
    --gg-gap: 1px; 
    --gg-size: 20px;
	margin: 0;
	padding: 0;
	border: 0;

	td {
		margin: 0;
		padding: 0;
		border-width: var(--gg-gap);
		border: 0;
	}
	div {
		overflow: hidden;
		width: var(--gg-size, 20px);
		aspect-ratio: 1/1;
		display: grid;
		grid-template-areas: 'stack';
		> * {
			grid-area: stack;
			display: grid;
			place-content: center;
		}
		span {
			font-size: calc(var(--gg-size) * 0.6);
		}
	}
	.floor {
		background-color: pink;
	}
	.wall {
		background-color: yellowgreen;
	}
	.room {
		background-color: var(--color-red-500);
	}
	.door {
		background-color: var(--color-blue-600);
	}
	.player {
		background-color: rebeccapurple;
	}
}

.custom-loader {
	width: 100px;
	height: 100px;
	display: grid;
	border: 8px solid #0000;
	border-radius: 50%;
	border-color: var(--color-accent-600) #0000;
	animation: animationS6 1s infinite linear;

	&::before,
	&::after {
		content: '';
		grid-area: 1/1;
		margin: 4px;
		border: inherit;
		border-radius: 50%;
	}

	&::before {
		border-color: var(--color-accent-300) #0000;
		animation: inherit;
		animation-duration: 1s;
		animation-direction: reverse;
	}

	&::after {
		margin: 16px;
	}
}
@keyframes animationS6 {
	100% {
		transform: rotate(1turn);
	}
}

```