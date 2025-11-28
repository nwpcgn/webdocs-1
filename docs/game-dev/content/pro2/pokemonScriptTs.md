--- 
title: Pokemon Script
sidebar_label: Pkmn III
sidebar_position: 721
---





Hier ist das JavaScript-Skript in TypeScript umgewandelt, und die `alert`- und `prompt`-Funktionen wurden durch HTML-Elemente ersetzt. Wir verwenden Vite für das Projekt. Zuerst zeige ich das aktualisierte TypeScript und HTML, dann gebe ich die Struktur des Projekts an, einschließlich der notwendigen Änderungen für Vite.

### TypeScript (`main.ts`)
```typescript
class WildPokemon {
    type: string;

    constructor(type: string) {
        this.type = type;
    }
}

class Move {
    move: string;
    basedmg: number;

    constructor(move: string, basedmg: number) {
        this.move = move;
        this.basedmg = basedmg;
    }
}

const pokemonStats: WildPokemon[] = [
    new WildPokemon("Zubat"),
    new WildPokemon("Oddish"),
    new WildPokemon("Geodude"),
    new WildPokemon("Slowpoke")
];

const moves: Move[] = [
    new Move("Slam", 5),
    new Move("Headbutt", 4),
    new Move("Tackle", 3),
    new Move("Cut", 4)
];

let wildPokemonId: number;
let wildPokemonLevel: number;
let wildPokemonHealth: number;
let moveId: number;
let damage: number;
let playerMove: number;
let playerTurn = false;
let pokemonHealth = 50;

const messages = document.getElementById("messages") as HTMLDivElement;
const actions = document.getElementById("actions") as HTMLDivElement;

function updateMessages(text: string) {
    const message = document.createElement("p");
    message.textContent = text;
    messages.appendChild(message);
}

function clearActions() {
    while (actions.firstChild) {
        actions.removeChild(actions.firstChild);
    }
}

function createButton(text: string, onClick: () => void) {
    const button = document.createElement("button");
    button.textContent = text;
    button.addEventListener("click", onClick);
    actions.appendChild(button);
}

function callWildPokemonId() {
    wildPokemonId = Math.floor(Math.random() * pokemonStats.length);
}

function callWildPokemonLevel() {
    wildPokemonLevel = Math.floor(Math.random() * 6 + 1);
}

function callWildPokemonHealth() {
    wildPokemonHealth = Math.floor(Math.random() + wildPokemonLevel + 3);
}

function callMoveId() {
    moveId = Math.floor(Math.random() * moves.length);
}

function callMoveDamage() {
    damage = Math.floor(Math.random() * moves[moveId].basedmg + 3);
}

function callPlayerMoveDamage() {
    damage = Math.floor(Math.random() * moves[playerMove].basedmg + 3);
}

function selectMove() {
    clearActions();
    moves.forEach((move, index) => {
        createButton(move.move, () => {
            playerMove = index;
            callPlayerMoveDamage();
            playerAttack();
        });
    });
}

function wildPokemonAttack() {
    if (pokemonHealth > 0) {
        pokemonHealth -= damage;
        updateMessages(`Wild ${pokemonStats[wildPokemonId].type} uses ${moves[moveId].move} dealing ${damage} damage!`);
        attackLoop();
    } else {
        updateMessages("Pokemon fainted");
    }
}

function playerAttack() {
    updateMessages(`Player uses ${moves[playerMove].move} dealing ${damage} damage!`);
    if (wildPokemonHealth > 0) {
        wildPokemonHealth -= damage;
        updateMessages(`${pokemonStats[wildPokemonId].type} has ${wildPokemonHealth} health remaining!`);
        playerTurn = false;
        wildPokemonFaint();
    } else {
        updateMessages(`${pokemonStats[wildPokemonId].type} fainted`);
    }
}

function randomPokemon() {
    callWildPokemonId();
    callWildPokemonHealth();
    callWildPokemonLevel();
    callMoveId();
}

function startFight() {
    randomPokemon();
    updateMessages(`Wild ${pokemonStats[wildPokemonId].type} appears!`);
    updateMessages(`Level : ${wildPokemonLevel} Health : ${wildPokemonHealth}`);
    playerTurn = false;
    attackLoop();
}

function attackLoop() {
    if (playerTurn === false) {
        playerTurn = true;
        wildPokemonAttack();
    } else {
        playerTurn = false;
        selectMove();
    }
}

function wildPokemonFaint() {
    if (wildPokemonHealth < 1) {
        updateMessages(`${pokemonStats[wildPokemonId].type} has fainted`);
    } else {
        attackLoop();
    }
}

document.addEventListener("DOMContentLoaded", startFight);
```

### HTML (`index.html`)
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pokemon Battle</title>
</head>
<body>
    <div id="messages"></div>
    <div id="actions"></div>
    <script type="module" src="/main.ts"></script>
</body>
</html>
```

### Projektstruktur
Hier ist die Verzeichnisstruktur des Projekts:
```
pokemon-battle/
│
├── index.html
├── main.ts
├── tsconfig.json
└── vite.config.ts
```

### Vite-Konfiguration

#### `vite.config.ts`
```typescript
import { defineConfig } from 'vite';

export default defineConfig({
    root: '.',
    server: {
        open: true
    }
});
```

#### `tsconfig.json`
```json
{
    "compilerOptions": {
        "target": "esnext",
        "module": "esnext",
        "strict": true,
        "jsx": "preserve",
        "moduleResolution": "node",
        "esModuleInterop": true,
        "skipLibCheck": true,
        "forceConsistentCasingInFileNames": true,
        "isolatedModules": true
    },
    "include": ["./**/*.ts"],
    "exclude": ["node_modules"]
}
```

### Installation und Start des Projekts
1. Installiere die notwendigen Pakete:
    ```sh
    npm install
    ```
2. Starte den Entwicklungsserver:
    ```sh
    npm run dev
    ```

Das sollte das Spiel korrekt starten und die Benutzeroberfläche anzeigen. Die Meldungen und Angriffe werden nun in den HTML-Elementen angezeigt, anstatt `alert`- und `prompt`-Funktionen zu verwenden.

