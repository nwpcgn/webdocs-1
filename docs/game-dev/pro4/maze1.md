---
title: GameDev 1
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 1
---


Ein Maze-Game (Labyrinthspiel) in JavaScript zu erstellen, ist ein tolles Projekt, um grundlegende Programmierkenntnisse und die Arbeit mit dem DOM zu erlernen. Hier ist eine Schritt-für-Schritt-Anleitung, um ein einfaches Maze-Game zu erstellen:

### Schritt 1: HTML-Grundstruktur

Erstelle eine HTML-Datei, die das Spielfeld und die notwendigen Elemente enthält.

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Maze Game</title>
    <style>
        #gameBoard {
            display: grid;
            grid-template-columns: repeat(10, 30px);
            grid-template-rows: repeat(10, 30px);
            gap: 2px;
        }
        .cell {
            width: 30px;
            height: 30px;
            background-color: lightgrey;
        }
        .wall {
            background-color: black;
        }
        .start {
            background-color: green;
        }
        .end {
            background-color: red;
        }
        .player {
            background-color: blue;
        }
    </style>
</head>
<body>
    <div id="gameBoard"></div>
    <script src="maze.js"></script>
</body>
</html>

```

### Schritt 2: Maze-Layout definieren

Definiere das Layout des Labyrinths in einer 2D-Array-Form. Speichere diese Daten in einer JavaScript-Datei (`maze.js`).

```javascript
const maze = [
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1],
    [1, 0, 0, 0, 1, 0, 0, 0, 0, 1],
    [1, 0, 1, 0, 1, 0, 1, 1, 0, 1],
    [1, 0, 1, 0, 0, 0, 0, 1, 0, 1],
    [1, 0, 1, 1, 1, 1, 0, 1, 0, 1],
    [1, 0, 0, 0, 0, 0, 0, 1, 0, 1],
    [1, 1, 1, 1, 1, 1, 0, 1, 0, 1],
    [1, 0, 0, 0, 0, 1, 0, 0, 0, 1],
    [1, 1, 1, 1, 0, 1, 1, 1, 0, 1],
    [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
];

const start = { x: 1, y: 1 };
const end = { x: 8, y: 8 };
let player = { ...start };

function createBoard() {
    const gameBoard = document.getElementById('gameBoard');
    for (let y = 0; y < maze.length; y++) {
        for (let x = 0; x < maze[y].length; x++) {
            const cell = document.createElement('div');
            cell.classList.add('cell');
            if (maze[y][x] === 1) {
                cell.classList.add('wall');
            }
            if (x === start.x && y === start.y) {
                cell.classList.add('start');
            }
            if (x === end.x && y === end.y) {
                cell.classList.add('end');
            }
            gameBoard.appendChild(cell);
        }
    }
    updatePlayerPosition();
}

function updatePlayerPosition() {
    const cells = document.querySelectorAll('.cell');
    cells.forEach(cell => cell.classList.remove('player'));
    const playerCell = document.querySelector(`#gameBoard > div:nth-child(${player.y * 10 + player.x + 1})`);
    playerCell.classList.add('player');
}

function movePlayer(dx, dy) {
    const newX = player.x + dx;
    const newY = player.y + dy;
    if (maze[newY][newX] !== 1) {
        player.x = newX;
        player.y = newY;
        updatePlayerPosition();
        if (newX === end.x && newY === end.y) {
            alert('You win!');
        }
    }
}

document.addEventListener('keydown', (e) => {
    switch (e.key) {
        case 'ArrowUp':
            movePlayer(0, -1);
            break;
        case 'ArrowDown':
            movePlayer(0, 1);
            break;
        case 'ArrowLeft':
            movePlayer(-1, 0);
            break;
        case 'ArrowRight':
            movePlayer(1, 0);
            break;
    }
});

createBoard();

```

### Erklärung der Schritte

1.  **HTML-Grundstruktur**:
    
    -   Ein `div`-Element mit der ID `gameBoard` dient als Container für das Spielfeld.
    -   Ein eingebettetes Stylesheet definiert das Layout und die Farben der Zellen (Spieler, Wände, Start, Ziel).
2.  **JavaScript**:
    
    -   Das `maze`-Array definiert das Layout des Labyrinths. `1` steht für Wände, `0` für freie Wege.
    -   `start` und `end` definieren die Start- und Endpositionen des Spielers.
    -   Die Funktion `createBoard` erzeugt das Spielfeld und fügt die entsprechenden Klassen zu den Zellen hinzu.
    -   `updatePlayerPosition` aktualisiert die Position des Spielers auf dem Spielfeld.
    -   `movePlayer` bewegt den Spieler in die gewünschte Richtung und prüft, ob die Bewegung möglich ist.
    -   Ein `keydown`-Event-Listener ermöglicht die Steuerung des Spielers mit den Pfeiltasten.

### Weitere Ideen zur Erweiterung

-   Hinzufügen von Hindernissen oder beweglichen Elementen.
-   Implementierung eines Timer-Systems.
-   Erstellung verschiedener Level mit zunehmendem Schwierigkeitsgrad.
-   Hinzufügen von Soundeffekten und Animationen.

Dieses Grundgerüst bietet eine solide Basis, auf der du weiter aufbauen und das Spiel nach Belieben erweitern kannst. Viel Spaß beim Programmieren!


----

Das Generieren von Maps (Labyrinthe) für ein Maze-Spiel kann auf verschiedene Weisen erfolgen, je nach gewünschtem Schwierigkeitsgrad und Stil des Spiels. Hier sind einige der häufigsten und effektivsten Methoden zur Maze-Generierung:

### 1. Rekursive Tiefensuche (Depth-First Search, DFS)

Die rekursive Tiefensuche ist eine einfache und effektive Methode zur Generierung von Labyrinthen. Sie funktioniert durch das Backtracking-Verfahren.

```javascript
function generateMaze(width, height) {
    const maze = Array.from({ length: height }, () => Array(width).fill(1));
    const stack = [];
    const startX = 1;
    const startY = 1;
    maze[startY][startX] = 0;
    stack.push({ x: startX, y: startY });

    const directions = [
        { x: 0, y: -2 }, // up
        { x: 2, y: 0 },  // right
        { x: 0, y: 2 },  // down
        { x: -2, y: 0 }  // left
    ];

    while (stack.length > 0) {
        const current = stack.pop();
        const shuffledDirections = directions.sort(() => Math.random() - 0.5);

        for (const direction of shuffledDirections) {
            const nx = current.x + direction.x;
            const ny = current.y + direction.y;

            if (nx > 0 && ny > 0 && nx < width - 1 && ny < height - 1 && maze[ny][nx] === 1) {
                maze[ny][nx] = 0;
                maze[current.y + direction.y / 2][current.x + direction.x / 2] = 0;
                stack.push({ x: nx, y: ny });
            }
        }
    }

    return maze;
}

const maze = generateMaze(21, 21); // Beispiel für ein 21x21 Maze
console.log(maze);

```

### 2. Prim's Algorithmus

Ein weiterer Ansatz zur Maze-Generierung ist der Algorithmus von Prim. Er erzeugt einen minimalen Spannbaum (Minimum Spanning Tree), der als Maze interpretiert wird.

```javascript
function generateMazePrims(width, height) {
    const maze = Array.from({ length: height }, () => Array(width).fill(1));
    const walls = [];
    const startX = 1;
    const startY = 1;

    maze[startY][startX] = 0;
    walls.push({ x: startX + 1, y: startY, dir: 'H' });
    walls.push({ x: startX, y: startY + 1, dir: 'V' });

    while (walls.length > 0) {
        const index = Math.floor(Math.random() * walls.length);
        const wall = walls.splice(index, 1)[0];
        const { x, y, dir } = wall;

        if (x > 0 && y > 0 && x < width - 1 && y < height - 1) {
            if (dir === 'H' && maze[y][x - 1] !== maze[y][x + 1]) {
                maze[y][x] = 0;
                if (maze[y][x - 1] === 1) {
                    walls.push({ x: x - 1, y: y, dir: 'H' });
                    walls.push({ x: x, y: y - 1, dir: 'V' });
                    walls.push({ x: x, y: y + 1, dir: 'V' });
                } else {
                    walls.push({ x: x + 1, y: y, dir: 'H' });
                    walls.push({ x: x, y: y - 1, dir: 'V' });
                    walls.push({ x: x, y: y + 1, dir: 'V' });
                }
            } else if (dir === 'V' && maze[y - 1][x] !== maze[y + 1][x]) {
                maze[y][x] = 0;
                if (maze[y - 1][x] === 1) {
                    walls.push({ x: x, y: y - 1, dir: 'V' });
                    walls.push({ x: x - 1, y: y, dir: 'H' });
                    walls.push({ x: x + 1, y: y, dir: 'H' });
                } else {
                    walls.push({ x: x, y: y + 1, dir: 'V' });
                    walls.push({ x: x - 1, y: y, dir: 'H' });
                    walls.push({ x: x + 1, y: y, dir: 'H' });
                }
            }
        }
    }

    return maze;
}

const maze = generateMazePrims(21, 21); // Beispiel für ein 21x21 Maze
console.log(maze);

```

### 3. Randomized Kruskal's Algorithm

Ein weiterer interessanter Ansatz ist der randomisierte Algorithmus von Kruskal. Er erzeugt ein Maze, indem er zufällige Kanten in einem Gitter verbindet, ohne Zyklen zu bilden.

### 4. Recursive Division Method

Die Methode der rekursiven Teilung teilt das Maze in kleinere Bereiche und fügt dann Öffnungen in den Wänden hinzu, um ein begehbares Labyrinth zu erstellen.

### Zusammenfassung

Die obigen Algorithmen bieten verschiedene Ansätze zur Maze-Generierung, jedes mit seinen eigenen Vor- und Nachteilen. Hier sind einige Aspekte, die du bei der Auswahl eines Algorithmus berücksichtigen kannst:

-   **DFS**: Einfach zu implementieren, erzeugt aber oft lange Korridore und wenige Sackgassen.
-   **Prim's Algorithmus**: Erzeugt eher verzweigte Labyrinthe mit vielen Sackgassen.
-   **Kruskal's Algorithmus**: Gut für unregelmäßige und komplexe Labyrinthe, kann jedoch etwas schwieriger zu implementieren sein.
-   **Recursive Division**: Erzeugt symmetrische und oft herausfordernde Labyrinthe.

Alle diese Methoden können in JavaScript implementiert werden und bieten eine gute Grundlage für die Erstellung dynamischer und herausfordernder Labyrinthe in deinem Maze-Spiel.



Ja, genau! Das Erkunden unbekannter Gebiete und das Aufeinandertreffen sowohl guter als auch schlechter Ereignisse sind zentrale Aspekte von Roguelike-Spielen. Hier sind einige der typischen Erfahrungen, die du in einem Roguelike-Spiel erwarten kannst:

### Erkunden unbekannter Gebiete

-   **Zufällig generierte Levels**: Jeder Spieldurchlauf erzeugt neue Karten, die dir ein frisches und unvorhersehbares Spielerlebnis bieten.
-   **Verborgene Räume und Schätze**: Du könntest versteckte Schätze oder geheime Räume entdecken, die wertvolle Gegenstände oder Boni enthalten.
-   **Vielfalt an Umgebungen**: Die Spiele bieten oft verschiedene Arten von Umgebungen wie Höhlen, Wälder, Tempel oder andere einzigartige Landschaften.

### Gute Ereignisse

-   **Wertvolle Gegenstände**: Du könntest mächtige Waffen, Rüstungen, Zauber oder Tränke finden, die dir im weiteren Spielverlauf helfen.
-   **Verbesserungen und Level-Ups**: Das Besiegen von Feinden und Lösen von Rätseln kann dir Erfahrungspunkte und Level-Ups einbringen, wodurch dein Charakter stärker wird.
-   **Freundliche NPCs**: Manchmal triffst du auf freundliche Nicht-Spieler-Charaktere, die dir hilfreiche Tipps geben, dich heilen oder dir Gegenstände verkaufen.

### Schlechte Ereignisse

-   **Gefährliche Gegner**: Du kannst auf mächtige Feinde oder Monster treffen, die dich herausfordern und schwer zu besiegen sind.
-   **Tödliche Fallen**: Viele Roguelikes enthalten versteckte Fallen, die deinem Charakter Schaden zufügen oder ihn sogar töten können.
-   **Flüche und negative Effekte**: Bestimmte Gegenstände oder Ereignisse können deinem Charakter negative Status-Effekte wie Vergiftungen, Flüche oder andere Debuffs zufügen.

### Beispielhafter Ablauf in einem Roguelike-Spiel

1.  **Start des Spiels**:
    
    -   Du beginnst mit einem grundlegenden Ausrüstungsset in einem prozedural generierten Dungeon.
2.  **Erkunden**:
    
    -   Du bewegst dich durch die zufällig generierten Räume und Gänge, deckst dabei die Karte auf und stößt auf verschiedene Ereignisse.
3.  **Kampf und Überleben**:
    
    -   Du kämpfst gegen zufällige Monster und sammelst dabei Beute. Jeder Schritt muss überlegt sein, da Ressourcen wie Gesundheit und Vorräte begrenzt sind.
4.  **Unvorhersehbare Begegnungen**:
    
    -   Du könntest einen mächtigen Gegenstand finden, der dir hilft, oder auf eine tödliche Falle stoßen, die deinen Spieldurchlauf abrupt beendet.
5.  **Weiterkommen oder Neustart**:
    
    -   Wenn du erfolgreich bist, bewegst du dich tiefer in das Dungeon oder die Spielwelt hinein. Wenn du stirbst, startest du von vorne, möglicherweise mit neuen Erkenntnissen oder Strategien.

### Fazit

Roguelike-Spiele bieten eine Mischung aus strategischem Denken, Glück und Anpassungsfähigkeit. Der Reiz dieser Spiele liegt in der Unvorhersehbarkeit und dem hohen Wiederspielwert. Jedes Spiel ist eine neue Herausforderung, und selbst erfahrene Spieler können durch unerwartete Ereignisse überrascht werden.

