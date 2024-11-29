---
title: GameDev 2
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 2
---





Um ein Roguelike-Spiel in JavaScript zu programmieren, kannst du die Struktur in mehrere Klassen und Module unterteilen, die jeweils spezifische Aspekte des Spiels handhaben. Hier ist ein grundlegender Entwurf für die Strukturierung der Räume, des Spielerinventars und der Ereignisse:

### Projektstruktur

```
/roguelike-game
    /css
        - styles.css
    /js
        - game.js
        - player.js
        - room.js
        - event.js
        - utils.js
    - index.html

```

### index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Roguelike Game</title>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div id="game-container"></div>
    <script src="js/utils.js"></script>
    <script src="js/player.js"></script>
    <script src="js/room.js"></script>
    <script src="js/event.js"></script>
    <script src="js/game.js"></script>
</body>
</html>

```

### utils.js

Hilfsfunktionen, die im gesamten Spiel verwendet werden.

```javascript
// js/utils.js
function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

export { getRandomInt };

```

### player.js

Definiert den Spielercharakter und sein Inventar.

```javascript
// js/player.js
class Player {
    constructor(name) {
        this.name = name;
        this.health = 100;
        this.inventory = [];
    }

    addItem(item) {
        this.inventory.push(item);
    }

    removeItem(itemName) {
        this.inventory = this.inventory.filter(item => item.name !== itemName);
    }

    getInventory() {
        return this.inventory;
    }
}

export default Player;

```

### room.js

Definiert die Räume und deren Eigenschaften.

```javascript
// js/room.js
import { getRandomInt } from './utils.js';

class Room {
    constructor(id, description) {
        this.id = id;
        this.description = description;
        this.events = [];
    }

    addEvent(event) {
        this.events.push(event);
    }

    getRandomEvent() {
        if (this.events.length === 0) return null;
        return this.events[getRandomInt(0, this.events.length - 1)];
    }
}

export default Room;

```

### event.js

Definiert verschiedene Ereignisse, die in den Räumen auftreten können.

```javascript
// js/event.js
class Event {
    constructor(name, effect) {
        this.name = name;
        this.effect = effect;
    }

    trigger(player) {
        this.effect(player);
    }
}

export default Event;

```

### game.js

Hauptlogik des Spiels, einschließlich der Initialisierung der Räume, des Spielers und der Ereignisse.

```javascript
// js/game.js
import Player from './player.js';
import Room from './room.js';
import Event from './event.js';

const gameContainer = document.getElementById('game-container');

// Initialisierung des Spielers
const player = new Player('Hero');

// Initialisierung der Räume
const rooms = [
    new Room(1, 'A dark and damp cave.'),
    new Room(2, 'A bright and sunny meadow.'),
    new Room(3, 'A spooky haunted house.')
];

// Initialisierung der Ereignisse
const findTreasure = new Event('Find Treasure', (player) => {
    player.addItem({ name: 'Treasure', value: 100 });
    alert(`${player.name} found a treasure!`);
});

const encounterMonster = new Event('Encounter Monster', (player) => {
    player.health -= 20;
    alert(`${player.name} encountered a monster and lost 20 health.`);
});

// Ereignisse den Räumen hinzufügen
rooms[0].addEvent(findTreasure);
rooms[0].addEvent(encounterMonster);
rooms[1].addEvent(findTreasure);
rooms[2].addEvent(encounterMonster);

// Beispielhafte Funktion, um einen Raum zu betreten und ein Ereignis auszulösen
function enterRoom(roomId) {
    const room = rooms.find(r => r.id === roomId);
    if (!room) {
        console.error('Room not found');
        return;
    }

    gameContainer.innerHTML = `<p>${room.description}</p>`;
    const event = room.getRandomEvent();
    if (event) {
        event.trigger(player);
    } else {
        alert('Nothing happens...');
    }

    updatePlayerStatus();
}

// Aktualisiere den Spielerstatus
function updatePlayerStatus() {
    const statusDiv = document.createElement('div');
    statusDiv.innerHTML = `
        <p>Name: ${player.name}</p>
        <p>Health: ${player.health}</p>
        <p>Inventory: ${player.getInventory().map(item => item.name).join(', ')}</p>
    `;
    gameContainer.appendChild(statusDiv);
}

// Start des Spiels
function startGame() {
    gameContainer.innerHTML = '<h1>Welcome to the Roguelike Game</h1>';
    rooms.forEach(room => {
        const roomButton = document.createElement('button');
        roomButton.innerText = `Enter Room ${room.id}`;
        roomButton.onclick = () => enterRoom(room.id);
        gameContainer.appendChild(roomButton);
    });
    updatePlayerStatus();
}

startGame();

```

### styles.css

Einfache CSS-Datei, um das Aussehen der Seite zu steuern.

```css
/* css/styles.css */
body {
    font-family: Arial, sans-serif;
    background-color: #f0f0f0;
    color: #333;
    padding: 20px;
}

#game-container {
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    max-width: 600px;
    margin: 0 auto;
}

button {
    margin: 5px;
    padding: 10px;
    font-size: 16px;
}

```

### Erklärung des Codes

-   **Spieler**: Die `Player`-Klasse enthält Eigenschaften und Methoden für den Spielercharakter, einschließlich des Inventars.
-   **Räume**: Die `Room`-Klasse definiert Räume mit einer Beschreibung und einer Liste von möglichen Ereignissen.
-   **Ereignisse**: Die `Event`-Klasse definiert Ereignisse, die dem Spieler passieren können, und enthält eine Methode zur Auslösung dieser Ereignisse.
-   **Spiel**: Das `game.js`-Skript initialisiert den Spieler, die Räume und die Ereignisse. Es enthält die Hauptlogik für das Betreten von Räumen und das Auslösen von Ereignissen.

Mit diesem grundlegenden Framework kannst du ein einfaches Roguelike-Spiel erstellen und nach Belieben erweitern, indem du mehr Räume, Ereignisse und komplexere Spielmechaniken hinzufügst.


