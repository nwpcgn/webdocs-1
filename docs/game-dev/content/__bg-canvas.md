---
title: Bg Canvas
sidebar_position: 22
sidebar_label: Bg-Canvas
---



## Background City Skyline Canvas


```html
<!DOCTYPE html>
<html lang="de">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <link rel="icon" type="image/svg+xml" href="data:image/svg+xml,<svg xmlns=%22http://www.w3.org/2000/svg%22 width=%22256%22 height=%22256%22 viewBox=%220 0 100 100%22><rect width=%22100%22 height=%22100%22 rx=%2220%22 fill=%22%230ea5e9%22></rect><path fill=%22%23fff%22 d=%22M24.98 67.77L30.86 67.77L30.86 32.23L24.95 32.23L24.90 56.59L13.43 32.23L7.47 32.23L7.47 67.77L13.40 67.77L13.43 43.31L24.98 67.77ZM35.40 32.23L40.04 67.77L45.58 67.77L49.15 46.02L49.41 44.34L49.66 46.02L53.27 67.77L58.81 67.77L63.50 32.23L58.03 32.23L55.57 54.05L55.40 55.54L55.15 54.08L51.61 32.23L47.12 32.23L43.70 53.98L43.46 55.44L43.29 54.05L40.84 32.23L35.40 32.23ZM74.22 67.77L74.22 54.22L80.25 54.22Q82.93 54.22 85.19 53.44Q87.45 52.66 89.09 51.22L89.09 51.22Q90.72 49.80 91.63 47.81Q92.53 45.83 92.53 43.36L92.53 43.36Q92.53 40.80 91.63 38.75Q90.72 36.69 89.09 35.25L89.09 35.25Q87.45 33.81 85.19 33.03Q82.93 32.25 80.25 32.23L80.25 32.23L68.36 32.23L68.36 67.77L74.22 67.77ZM80.25 49.44L74.22 49.44L74.22 37.01L80.25 37.01Q81.74 37.04 82.92 37.50Q84.11 37.96 84.94 38.82L84.94 38.82Q85.77 39.67 86.19 40.83Q86.62 41.99 86.62 43.41L86.62 43.41Q86.62 44.70 86.19 45.80Q85.77 46.90 84.94 47.71L84.94 47.71Q84.11 48.51 82.92 48.96Q81.74 49.41 80.25 49.44L80.25 49.44Z%22></path></svg>" />
    <title>nwp</title>
    <style id="jsbin-css">
        body {
            height: 100vh;
            width: 100vw;
            display: flex;
            flex-direction: column;
            margin: 0;
            padding: 0;
            overflow-x: hidden;
        }

        .main {
            flex: 1;
            position: relative;
            overflow: hidden;
        }

        .layer {
            width: 100%;
            height: 100%;
            position: absolute;
            top: 0;
            left: 0;
            transition: opacity 600ms cubic-bezier(0.42, -0, 0.58, 1) 110ms,
                transform 500ms cubic-bezier(0.42, -0, 0.58, 1) 10ms;
            overflow-x: hidden;
            overflow-y: auto;
            flex-direction: column;
            display: flex;
            z-index: var(--z, auto);
        }
    </style>
</head>

<body>
    <main class="main">
        <section class="layer"><canvas class="border" id="game" /></section>
    </main>
    <script id="jsbin-javascript">
        let state = {};
        let canvas;
        let ctx;

        function newGame() {
            // Reset game state
            state = {
                phase: "aiming", // aiming | in flight | celebrating
                currentPlayer: 1,
                bomb: {
                    x: undefined,
                    y: undefined,
                    rotation: 0,
                    velocity: {
                        x: 0,
                        y: 0
                    },
                },
                // Buildings
                backgroundBuildings: [],
                buildings: [],
                blastHoles: [],
                scale: 1,
            };
            // Generate background buildings
            for (let i = 0; i < 11; i++) {
                generateBackgroundBuilding(i);
            }
            // Generate buildings
            for (let i = 0; i < 8; i++) {
                generateBuilding(i);
            }
            calculateScale();
            initializeBombPosition();
            // ...
            draw();
        }

        function generateBackgroundBuilding(index) {
            const previousBuilding = state.backgroundBuildings[index - 1];
            const x = previousBuilding ? previousBuilding.x + previousBuilding.width + 4 : -30;
            const minWidth = 60;
            const maxWidth = 110;
            const width = minWidth + Math.random() * (maxWidth - minWidth);
            const minHeight = 80;
            const maxHeight = 350;
            const height = minHeight + Math.random() * (maxHeight - minHeight);
            state.backgroundBuildings.push({
                x,
                width,
                height
            });
        }

        function generateBuilding(index) {
            const previousBuilding = state.buildings[index - 1];
            const x = previousBuilding ? previousBuilding.x + previousBuilding.width + 4 : 0;
            const minWidth = 80;
            const maxWidth = 130;
            const width = minWidth + Math.random() * (maxWidth - minWidth);
            const platformWithGorilla = index === 1 || index === 6;
            const minHeight = 40;
            const maxHeight = 300;
            const minHeightGorilla = 30;
            const maxHeightGorilla = 150;
            const height = platformWithGorilla ? minHeightGorilla + Math.random() * (maxHeightGorilla - minHeightGorilla) : minHeight + Math.random() * (maxHeight - minHeight);
            // Generate an array of booleans to show if the light is on or off in a room
            const lightsOn = [];
            for (let i = 0; i < 50; i++) {
                const light = Math.random() <= 0.33 ? true : false;
                lightsOn.push(light);
            }
            state.buildings.push({
                x,
                width,
                height,
                lightsOn
            });
        }

        function calculateScale() {
            const lastBuilding = state.buildings.at(-1);
            const totalWidthOfTheCity = lastBuilding.x + lastBuilding.width;
            state.scale = window.innerWidth / totalWidthOfTheCity;
        }

        function initializeBombPosition() {
            const building = state.currentPlayer === 1 ? state.buildings.at(1) // Second building
                : state.buildings.at(-2); // Second last building
            const gorillaX = building.x + building.width / 2;
            const gorillaY = building.height;
            const gorillaHandOffsetX = state.currentPlayer === 1 ? -28 : 28;
            const gorillaHandOffsetY = 107;
            state.bomb.x = gorillaX + gorillaHandOffsetX;
            state.bomb.y = gorillaY + gorillaHandOffsetY;
            state.bomb.velocity.x = 0;
            state.bomb.velocity.y = 0;
            // ...
        }

        function draw() {
            ctx.save();
            // Flip coordinate system upside down
            ctx.translate(0, window.innerHeight);
            ctx.scale(1, -1);
            ctx.scale(state.scale, state.scale);
            // Draw scene
            drawBackground();
            drawBackgroundBuildings();
            drawBuildings();
            ctx.restore();
        }

        function drawBackground() {
            const gradient = ctx.createLinearGradient(0, 0, 0, window.innerHeight / state.scale);
            gradient.addColorStop(1, "#F8BA85");
            gradient.addColorStop(0, "#FFC28E");
            // Draw sky
            ctx.fillStyle = gradient;
            ctx.fillRect(0, 0, window.innerWidth / state.scale, window.innerHeight / state.scale);
            // Draw moon
            ctx.fillStyle = "rgba(255, 255, 255, 0.6)";
            ctx.beginPath();
            ctx.arc(300, 350, 60, 0, 2 * Math.PI);
            ctx.fill();
        }

        function drawBackgroundBuildings() {
            state.backgroundBuildings.forEach((building) => {
                ctx.fillStyle = "#947285";
                ctx.fillRect(building.x, 0, building.width, building.height);
            });
        }

        function drawBuildings() {
            state.buildings.forEach((building) => {
                // Draw building
                ctx.fillStyle = "#4A3C68";
                ctx.fillRect(building.x, 0, building.width, building.height);
                // Draw windows
                const windowWidth = 10;
                const windowHeight = 12;
                const gap = 15;
                const numberOfFloors = Math.ceil(
                    (building.height - gap) / (windowHeight + gap));
                const numberOfRoomsPerFloor = Math.floor(
                    (building.width - gap) / (windowWidth + gap));
                for (let floor = 0; floor < numberOfFloors; floor++) {
                    for (let room = 0; room < numberOfRoomsPerFloor; room++) {
                        if (building.lightsOn[floor * numberOfRoomsPerFloor + room]) {
                            ctx.save();
                            ctx.translate(building.x + gap, building.height - gap);
                            ctx.scale(1, -1);
                            const x = room * (windowWidth + gap);
                            const y = floor * (windowHeight + gap);
                            ctx.fillStyle = "#EBB6A2";
                            ctx.fillRect(x, y, windowWidth, windowHeight);
                            ctx.restore();
                        }
                    }
                }
            });
        }

        function handleResize() {
            initCanvas().then(() => {
                calculateScale();
                draw();
            });
        }

        function initCanvas() {
            return new Promise((resolve, reject) => {
                let elem = document.querySelector("main.main");
                let rect = elem.getBoundingClientRect();
                canvas = document.getElementById("game");
                if (!canvas) reject;
                canvas.width = rect.width;
                canvas.height = rect.height;
                ctx = canvas.getContext("2d");
                resolve();
            });
        }
        const run = () => {
            console.log("app run");
            initCanvas().then(() => newGame());
        };
        window.addEventListener("resize", handleResize);
        window.addEventListener("DOMContentLoaded", () => {
            console.log("DOMContentLoaded");
            run();
        });
    </script>
</body>

</html>
```
