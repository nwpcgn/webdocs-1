---
title: Dungeon-Generator-2
sidebar_position: 22
sidebar_label: Dungeon-Generator-2
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


## App




## 🔍 Ziel

Statt das gesamte Spielfeld zu rendern, renderst du nur den sichtbaren Ausschnitt (`viewport`), basierend auf der Position des Helden und der definierten Kamera-Größe.

---

## ✅ Schritte zur Umsetzung

### 1. Kamera-Konfiguration

Definiere eine Kamera-Größe (z. B. 15x10 Tiles) und eine Kamera-Position (`cameraX`, `cameraY`), die sich mit dem Helden mitbewegt.

```ts
let cameraWidth = 15
let cameraHeight = 10

let cameraX = Math.max(0, hero.x - Math.floor(cameraWidth / 2))
let cameraY = Math.max(0, hero.y - Math.floor(cameraHeight / 2))
```

Clamp die Kamera, damit sie nicht über den Rand hinausgeht:

```ts
cameraX = Math.min(cameraX, cols - cameraWidth)
cameraY = Math.min(cameraY, rows - cameraHeight)
```

---

### 2. Sichtbares Grid berechnen

In deinem `<Dungeon.svelte>` änderst du die `each`-Schleifen:

```svelte
{#each value.tileMap.slice(cameraY, cameraY + cameraHeight) as row, yOffset}
  {#each row.slice(cameraX, cameraX + cameraWidth) as col, xOffset}
    {#if col !== '#'}
      <GridTile
        onclick={tileclick}
        x={xOffset}
        y={yOffset}
        color={col === '#' ? '#1a170f' : '#ee82cf'}
        col={col}
        tileSize={tileSize} />
    {/if}
  {/each}
{/each}

<HeroTile
  onclick={tileclick}
  hero={{
    x: hero.x - cameraX,
    y: hero.y - cameraY
  }}
  color="#450b26"
  col="@"
  tileSize={tileSize}
/>
```

---

### 3. Canvas-Größe begrenzen

Statt `cols * tileSize`, render nur den Ausschnitt:

```svelte
<Canvas
  layerEvents
  width={cameraWidth * tileSize}
  height={cameraHeight * tileSize}
  class="border-accent-500 border">
```

---

### 4. Bewegung aktualisiert Kamera

Wenn dein Held sich bewegt (z. B. per Tastendruck), aktualisiere auch `cameraX` und `cameraY` mit dem gleichen Logik wie oben.

---

## ✳️ Ergebnis

Du bekommst:

* Scrollbare Welt
* Fixes Canvas mit begrenzter Sicht
* Spieler bleibt im Fokus

---


`spring` aus `svelte/motion` ist ideal für eine **smooth-follow Kamera**, weil es automatisch Übergänge animiert, z. B. beim Verschieben der Kamera in Richtung des Helden.

---

## 🔁 Smooth Kamera mit `spring`

### 1. Importiere `spring` in `Dungeon.svelte`

```ts
import { spring } from 'svelte/motion'
```

---

### 2. Erstelle eine Spring-Variable für die Kamera-Position

```ts
const camera = spring({ x: 0, y: 0 }, { stiffness: 0.1, damping: 0.4 })
```

---

### 3. Reagiere auf Heldenbewegung

Immer wenn sich der Held bewegt, aktualisierst du das Ziel der Kamera:

```ts
$: {
  let targetX = Math.max(0, Math.min(hero.x - Math.floor(cameraWidth / 2), cols - cameraWidth))
  let targetY = Math.max(0, Math.min(hero.y - Math.floor(cameraHeight / 2), rows - cameraHeight))
  camera.set({ x: targetX, y: targetY })
}
```

---

### 4. Verwende `$camera` in der `slice`

```svelte
{#each value.tileMap.slice(Math.floor($camera.y), Math.floor($camera.y) + cameraHeight) as row, yOffset}
  {#each row.slice(Math.floor($camera.x), Math.floor($camera.x) + cameraWidth) as col, xOffset}
    <GridTile
      x={xOffset}
      y={yOffset}
      color={col === '#' ? '#1a170f' : '#ee82cf'}
      col={col}
      tileSize={tileSize} />
  {/each}
{/each}

<HeroTile
  hero={{
    x: hero.x - Math.floor($camera.x),
    y: hero.y - Math.floor($camera.y)
  }}
  color="#450b26"
  col="@"
  tileSize={tileSize}
/>
```

---

### ✅ Optional: Sanftes Bewegen im Subpixelbereich

Wenn du auch z. B. parallax oder Kamerabewegung rendern willst (z. B. bei einem `canvas.drawImage`), kannst du den genauen Wert aus `$camera.x` verwenden (ohne `Math.floor`), um sanft in Subpixeln zu scrollen.

---

