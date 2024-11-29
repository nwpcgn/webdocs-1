---
title: GameDev 7
tags: 
    - Gamedev
    - JS
sidebar_position: 32
sidebar_label: GameDev 7
---

Sicher, hier ist eine strukturierte Version des Codes, der die Initialisierung, das Rendering und das Update in separate Funktionen aufteilt. Die Variablen `canvas`, `context` und `camera` werden erst nach `window.onload` initialisiert:

```typescript
import { Tilemap, Layer, Tileset } from './types';

class Camera {
    x: number;
    y: number;
    width: number;
    height: number;
    worldWidth: number;
    worldHeight: number;

    constructor(x: number, y: number, width: number, height: number, worldWidth: number, worldHeight: number) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.worldWidth = worldWidth;
        this.worldHeight = worldHeight;
    }

    move(dx: number, dy: number) {
        this.x = Math.max(0, Math.min(this.x + dx, this.worldWidth - this.width));
        this.y = Math.max(0, Math.min(this.y + dy, this.worldHeight - this.height));
    }
}

async function loadImage(src: string): Promise<HTMLImageElement> {
    return new Promise((resolve, reject) => {
        const img = new Image();
        img.onload = () => resolve(img);
        img.onerror = (err) => reject(err);
        img.src = src;
    });
}

let canvas: HTMLCanvasElement | null = null;
let context: CanvasRenderingContext2D | null = null;
let camera: Camera | null = null;

const tilemap: Tilemap = {
    "compressionlevel": -1,
    "height": 10,
    "infinite": false,
    "layers": [
        {
            "data": [80, 81, 81, 81, 81, 81, 81, 81, 81, 82, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 103, 104, 104, 104, 104, 104, 104, 104, 104, 105, 126, 127, 127, 127, 127, 127, 127, 127, 127, 128],
            "height": 10,
            "id": 1,
            "name": "ebene1",
            "opacity": 1,
            "type": "tilelayer",
            "visible": true,
            "width": 10,
            "x": 0,
            "y": 0
        },
        {
            "data": [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 16, 16, 16, 16, 16, 16, 16, 16, 0, 0, 16, 18, 18, 18, 18, 18, 18, 16, 0, 0, 16, 18, 17, 17, 17, 17, 18, 16, 0, 0, 16, 18, 17, 19, 19, 17, 18, 16, 0, 0, 16, 18, 17, 19, 19, 17, 18, 16, 0, 0, 16, 18, 17, 17, 17, 17, 18, 16, 0, 0, 16, 18, 18, 18, 18, 18, 18, 16, 0, 0, 16, 16, 16, 16, 16, 16, 16, 16, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
            "height": 10,
            "id": 2,
            "name": "ebene2",
            "opacity": 1,
            "type": "tilelayer",
            "visible": true,
            "width": 10,
            "x": 0,
            "y": 0
        }
    ],
    "nextlayerid": 3,
    "nextobjectid": 1,
    "orientation": "orthogonal",
    "renderorder": "right-down",
    "tiledversion": "1.10.2",
    "tileheight": 64,
    "tilesets": [
        {
            "firstgid": 1,
            "columns": 23,
            "image": "towerDefense_tilesheet.png",
            "imageheight": 832,
            "imagewidth": 1472,
            "margin": 0,
            "name": "towerDefense_tilesheet",
            "spacing": 0,
            "tilecount": 299,
            "tiledversion": "1.10.2",
            "tileheight": 64,
            "tilewidth": 64,
            "type": "tileset",
            "version": "1.10"
        }
    ],
    "tilewidth": 64,
    "type": "map",
    "version": "1.10",
    "width": 10
};

async function initialize() {
    canvas = document.getElementById('myCanvas') as HTMLCanvasElement;
    context = canvas.getContext('2d');

    const mapWidth = tilemap.width * tilemap.tilewidth;
    const mapHeight = tilemap.height * tilemap.tileheight;
    const centerX = mapWidth / 2 - canvas.width / 2;
    const centerY = mapHeight / 2 - canvas.height / 2;

    camera = new Camera(centerX, centerY, canvas.width, canvas.height, mapWidth, mapHeight);

    await render();
    setupKeyboardListeners();
}

async function render() {
    if (!context || !camera) return;

    const tileset = tilemap.tilesets[0];
    const tilesetImage = await loadImage(tileset.image);

    const startCol = Math.floor(camera.x / tilemap.tilewidth);
    const endCol = Math.min(startCol + Math.ceil(camera.width / tilemap.tilewidth), tilemap.width);
    const startRow = Math.floor(camera.y / tilemap.tileheight);
    const endRow = Math.min(startRow + Math.ceil(camera.height / tilemap.tileheight), tilemap.height);

    const offsetX = -camera.x + startCol * tilemap.tilewidth;
    const offsetY = -camera.y + startRow * tilemap.tileheight;

    context.clearRect(0, 0, canvas.width, canvas.height);

    for (const layer of tilemap.layers) {
        if (layer.visible) {
            for (let y = startRow; y < endRow; y++) {
                for (let x = startCol; x < endCol; x++) {
                    const tileIndex = layer.data[y * tilemap.width + x];
                    if (tileIndex !== 0) {  // 0 bedeutet kein Tile
                        const tilesetIndex = tileIndex - tileset.firstgid;
                        const tileX = tilesetIndex % tileset.columns;
                        const tileY = Math.floor(tilesetIndex / tileset.columns);

                        context.drawImage(
                            tilesetImage,
                            tileX * tileset.tilewidth, tileY * tileset.tileheight, tileset.tilewidth, tileset.tileheight,
                            offsetX + x * tilemap.tilewidth, offsetY + y * tilemap.tileheight, tilemap.tilewidth, tilemap.tileheight
                        );
                    }
                }
            }
        }
    }
}

function updateCamera(dx: number, dy: number) {
    if (!camera) return;
    camera.move(dx, dy);
    render().catch(console.error);
}

function setupKeyboardListeners() {
    document.addEventListener('keydown', (event) => {
        const step = 10;
        switch(event.key) {
            case 'ArrowUp':
                updateCamera(0, -step);
                break;
            case 'ArrowDown':
                updateCamera(0, step);
                break;
            case 'ArrowLeft':
                updateCamera(-step, 0);
                break;
            case 'ArrowRight':
                updateCamera(step, 0);
                break;
        }
    });
}

window.onload = () => {
    initialize().catch(console.error);
};

```

### Erklärung:

-   **initialize**: Diese Funktion wird aufgerufen, sobald das Fenster geladen ist (`window.onload`). Sie initialisiert das `canvas`, den `context` und die `camera`, und ruft die `render`-Funktion auf.
-   **render**: Diese Funktion rendert die Tilemap basierend auf der aktuellen Position der Kamera.
-   **updateCamera**: Diese Funktion bewegt die Kamera und rendert die Karte erneut.
-   **setupKeyboardListeners**: Diese Funktion fügt Event-Listener für die Pfeiltasten hinzu,

um die Kamera zu bewegen.

Stellen Sie sicher, dass Sie die richtigen Importe und Typdefinitionen (`Tilemap`, `Layer`, `Tileset`) in der `types.ts` Datei haben.




