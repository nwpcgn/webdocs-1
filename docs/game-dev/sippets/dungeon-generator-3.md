---
title: Dungeon-Generator-3
sidebar_position: 23
sidebar_label: Dungeon-Generator-3
---





# Dungeon Map with ROT.JS



## Generator

```typescript title="getMap.ts"
import * as ROT from 'rot-js'
import type { DungeonGenerator } from './DungeonGenerator'
function getMap(width = 40, height = 20, type = 'Uniform'): DungeonGenerator {
	const digger = new ROT.Map[type](width, height)

	const map: string[][] = Array.from({ length: height }, () =>
		Array(width).fill(' ')
	)

	// lists
	const freeCells: string[] = []
	const zeroCells: string[] = []
	const rooms = []
	const items = []
	const enemys = []
	let position = { x: 0, y: 0 }

	// symbols
	const sym = {
		floor: '.',
		wall: '#',
		room: '_',
		door: 'D'
	}

	const tiles = {
		'4': '═', // horizontal
		'5': '║', // vertical
		'6': 'o' // corner
	}

	// helpers
	const randIndex = (arr) => Math.floor(ROT.RNG.getUniform() * arr.length)
	const parseKey = (str) => str.split(',').map(Number)
	const randNum = (min = 0, max = 1) => {
		min = Math.ceil(min)
		max = Math.floor(max)
		return Math.floor(Math.random() * (max - min)) + min
	}
	// callback: digger modifies map
	const digCallback = (x, y, value) => {
		map[y][x] = value ? sym.wall : sym.floor
		const key = `${x},${y}`

		if (value) zeroCells.push(key)
		else freeCells.push(key)
	}

	// -------------------------
	// WALL GENERATION AROUND ROOM
	// -------------------------
	const addRoomWalls = (room) => {
		const { x1, y1, x2, y2, doors } = room

		// door lookup set
		const doorSet = new Set(doors.map((d) => `${d.x},${d.y}`))
		const isDoor = (x, y) => doorSet.has(`${x},${y}`)

		// ECKEN
		if (!isDoor(x1 - 1, y1 - 1)) map[y1 - 1][x1 - 1] = tiles[6]
		if (!isDoor(x2 + 1, y1 - 1)) map[y1 - 1][x2 + 1] = tiles[6]
		if (!isDoor(x2 + 1, y2 + 1)) map[y2 + 1][x2 + 1] = tiles[6]
		if (!isDoor(x1 - 1, y2 + 1)) map[y2 + 1][x1 - 1] = tiles[6]

		// OBERSTE WAND
		for (let x = x1; x <= x2; x++) {
			if (!isDoor(x, y1 - 1)) map[y1 - 1][x] = tiles[4]
		}

		// UNTERSTE WAND
		for (let x = x1; x <= x2; x++) {
			if (!isDoor(x, y2 + 1)) map[y2 + 1][x] = tiles[4]
		}

		// LINKE WAND
		for (let y = y1; y <= y2; y++) {
			if (!isDoor(x1 - 1, y)) map[y][x1 - 1] = tiles[5]
		}

		// RECHTE WAND
		for (let y = y1; y <= y2; y++) {
			if (!isDoor(x2 + 1, y)) map[y][x2 + 1] = tiles[5]
		}
	}

	// -------------------------
	// ROOM CREATION
	// -------------------------
	const createRooms = () => {
		digger.getRooms().forEach((room, roomId) => {
			// console.log(`Raum ${roomId + 1}: X=${room.getLeft()}, Y=${room.getTop()}, Breite=${room.getWidth()}, Höhe=${room.getHeight()}`);
			// convertRoom + marking rooms on map
			const result = convertRoom(room)
			const [centerX, centerY] = room.getCenter()

			const roomObj = { ...result, centerX, centerY, roomId }
			rooms.push(roomObj)

			// ADD WALLS AROUND ROOM
			addRoomWalls(roomObj)
		})
	}

	// convert ROT.Room to your own shape
	const convertRoom = (r) => {
		const doors = Object.keys(r._doors).map((key) => {
			const [x, y] = key.split(',').map(Number)
			map[y][x] = sym.door
			return { x, y }
		})

		for (let y = r._y1; y <= r._y2; y++) {
			for (let x = r._x1; x <= r._x2; x++) {
				map[y][x] = sym.room
			}
		}

		return {
			x1: r._x1,
			y1: r._y1,
			x2: r._x2,
			y2: r._y2,
			doors
		}
	}

	// -------------------------
	// PLACE BOXES + ENEMIES
	// -------------------------
	const generateBoxes = (free) => {
		let countItems = randNum(6, 10)
		for (let i = 0; i < countItems; i++) {
			const idx = randIndex(free)
			items.push(free.splice(idx, 1)[0])
		}

		let countEnemies = randNum(1, 3)
		for (let i = 0; i < countEnemies; i++) {
			const idx = randIndex(free)
			enemys.push(free.splice(idx, 1)[0])
		}
	}

	// -------------------------
	// PLACE PLAYER
	// -------------------------
	const placeFighter = (free) => {
		const idx = randIndex(free)
		const [x, y] = parseKey(free.splice(idx, 1)[0])
		position = { x, y }
	}

	// -------------------------
	// START PROCEDURE
	// -------------------------
	digger.create(digCallback)
	createRooms()
	generateBoxes(freeCells)
	placeFighter(freeCells)

	return { map, rooms, freeCells, items, enemys, position }
}
export default getMap
```
