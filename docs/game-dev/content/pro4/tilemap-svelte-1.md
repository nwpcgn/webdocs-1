---
title: Tilemap Svelte
tags: 
    - Gamedev
    - JS
sidebar_position: 33
sidebar_label: Tilemap Svelte
---


## Render Tilemap Canvas






```javascript title="tilemap.svelte"
import { onMount } from 'svelte';

let canvas;

const tileSize = 32; // Größe eines Tiles in Pixeln
const tilesetImage = new Image();
tilesetImage.src = 'path/to/tileset.png'; // Ersetze dies mit dem Pfad zu deinem Tileset

const tilemap = {
  width: 25,
  height: 25,
  layers: [
    {
      data: [
        /* Deine Tilemap-Daten hier einfügen */
      ],
      width: 25,
      height: 25
    }
  ]
};

const tilesetColumns = 10; // Anzahl der Spalten im Tileset (abhängig vom Bild)

function drawTilemap(ctx) {
  tilemap.layers.forEach(layer => {
    layer.data.forEach((tile, index) => {
      if (tile === 0) return; // 0 bedeutet leeres Feld
      
      const x = (index % tilemap.width) * tileSize;
      const y = Math.floor(index / tilemap.width) * tileSize;
      
      const tileX = ((tile - 1) % tilesetColumns) * tileSize;
      const tileY = Math.floor((tile - 1) / tilesetColumns) * tileSize;
      
      ctx.drawImage(tilesetImage, tileX, tileY, tileSize, tileSize, x, y, tileSize, tileSize);
    });
  });
}

onMount(() => {
  const ctx = canvas.getContext('2d');
  tilesetImage.onload = () => drawTilemap(ctx);
});
```



## Pokemon Combat System


```javascript
class Pokemon {
  constructor(data) {
    this.id = data.id;
    this.name = data.name.english;
    this.type = data.type;
    this.baseStats = data.base;
    this.currentHP = data.base.HP;
  }

  attack(target, move) {
    const attackPower = this.baseStats.Attack;
    const defensePower = target.baseStats.Defense;
    const damage = Math.max(1, attackPower - defensePower);
    target.currentHP = Math.max(0, target.currentHP - damage);
    
    console.log(`${this.name} used ${move}! ${target.name} took ${damage} damage.`);
    
    if (target.currentHP === 0) {
      console.log(`${target.name} fainted!`);
    }
  }
}

// Beispiel-Pokémon
const bulbasaur = new Pokemon({
  id: 1,
  name: { english: "Bulbasaur" },
  type: ["Grass", "Poison"],
  base: { HP: 45, Attack: 49, Defense: 49, "Sp. Attack": 65, "Sp. Defense": 65, Speed: 45 }
});

const charmander = new Pokemon({
  id: 4,
  name: { english: "Charmander" },
  type: ["Fire"],
  base: { HP: 39, Attack: 52, Defense: 43, "Sp. Attack": 60, "Sp. Defense": 50, Speed: 65 }
});

// Kampf-Simulation
bulbasaur.attack(charmander, "Tackle");
charmander.attack(bulbasaur, "Scratch");

```


## Battle System

```javascript title="battle.svelte"
function calculateDamage(attacker, defender, move) {
    if (Math.random() * 100 > move.accuracy) {
        console.log(`${attacker.name.english}'s attack missed!`);
        return 0;
    }
    
    const attackStat = attacker.base.Attack;
    const defenseStat = defender.base.Defense;
    
    const damage = Math.max(1, Math.floor(((2 * attackStat / defenseStat) * move.power) / 5));
    return damage;
}

function battle(attacker, defender, move) {
    console.log(`${attacker.name.english} uses ${move.ename}!`);
    const damage = calculateDamage(attacker, defender, move);
    defender.base.HP = Math.max(0, defender.base.HP - damage);
    
    console.log(`${defender.name.english} takes ${damage} damage. Remaining HP: ${defender.base.HP}`);
    
    if (defender.base.HP === 0) {
        console.log(`${defender.name.english} fainted!`);
    }
}

const moves = [{
    "accuracy": 100,
    "category": "Physical",
    "cname": "拍击",
    "ename": "Pound",
    "id": 1,
    "jname": "はたく",
    "power": 40,
    "pp": 35,
    "type": "Normal"
}];

const pokedex = [{
    "id": 1,
    "name": { "english": "Bulbasaur", "japanese": "フシギダネ", "chinese": "妙蛙种子", "french": "Bulbizarre" },
    "type": ["Grass", "Poison"],
    "base": { "HP": 45, "Attack": 49, "Defense": 49, "Sp. Attack": 65, "Sp. Defense": 65, "Speed": 45 }
}, {
    "id": 4,
    "name": { "english": "Charmander", "japanese": "ヒトカゲ", "chinese": "小火龙", "french": "Salamèche" },
    "type": ["Fire"],
    "base": { "HP": 39, "Attack": 52, "Defense": 43, "Sp. Attack": 60, "Sp. Defense": 50, "Speed": 65 }
}];

const items = [
    { "id": 1, "name": { "english": "Master Ball", "japanese": "マスターボール", "chinese": "大师球" } },
    { "id": 2, "name": { "english": "Ultra Ball", "japanese": "ハイパーボール", "chinese": "高级球" } },
    { "id": 3, "name": { "english": "Great Ball", "japanese": "スーパーボール", "chinese": "超级球" } },
    { "id": 4, "name": { "english": "Poké Ball", "japanese": "モンスターボール", "chinese": "精灵球" } }
];

// Beispielkampf
battle(pokedex[0], pokedex[1], moves[0]);

```

Hier ist eine erweiterte `battle`-Funktion, die Angriffs-Moves berücksichtigt. Der Spieler kann aus einer Liste von Moves wählen, und das System berechnet den Schaden basierend auf Stärke, Verteidigung und Genauigkeit. Zudem gibt es eine einfache Item-Liste für spätere Erweiterungen, z. B. Heilgegenstände oder Pokébälle:

Diese Funktion führt einen simplen Kampf zwischen zwei Pokémon durch und berechnet den Schaden basierend auf den Basiswerten. Weitere Erweiterungen könnten Statusveränderungen, Spezialangriffe oder verschiedene Item-Effekte sein.