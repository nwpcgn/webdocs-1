---
title: Loot-Generator
sidebar_position: 16
sidebar_label: Loot-Gen
---





## Loot-Kategorien und Beispiel-Items

Hier ist eine gute Basis, die man für die meisten Rollenspiele, Action- oder Fantasy-Games verwenden kann:

---

### ⚔️ Waffen
- **Rusty Sword** *(common)*
- **Iron Axe** *(uncommon)*
- **Flame Bow** *(rare)*
- **Shadow Dagger** *(epic)*
- **Blade of Eternity** *(legendary)*

---

### 🛡️ Rüstung
- **Leather Armor**
- **Iron Plate**
- **Magic Robe**
- **Dragon Scale Armor**
- **Aegis of the Ancients**

---

### 🧪 Tränke (Consumables)
- **Health Potion**
- **Mana Potion**
- **Stamina Elixir**
- **Antidote**
- **Potion of Invincibility**

---

### 💎 Materialien / Ressourcen
- **Iron Ore**
- **Enchanted Wood**
- **Soul Shard**
- **Mystic Crystal**
- **Void Essence**

---

### 🎁 Sonstige / Spezialitems
- **Treasure Map**
- **Mystery Key**
- **Teleport Scroll**
- **Pet Egg**
- **Ancient Relic**

---

## ⚙️ Struktur für einen Generator

```ts
type Rarity = 'common' | 'uncommon' | 'rare' | 'epic' | 'legendary';

type LootItem = {
  name: string;
  rarity: Rarity;
  category: 'weapon' | 'armor' | 'potion' | 'material' | 'special';
  dropChance: number;
};
```

Dann kannst du pro Kategorie eine eigene `lootTable` anlegen und den Generator z. B. so aufrufen:

```ts
function generateLootByCategory(category: LootItem['category']): LootItem | null {
  const table = masterLootTable.filter(item => item.category === category);
  return getLoot(table); // gleiche getLoot-Logik wie oben
}
```





## Universal-Loot-Generator

### LootItem-Typ & Kategorien

```ts
type Rarity = 'common' | 'uncommon' | 'rare' | 'epic' | 'legendary';

type Category = 'weapon' | 'armor' | 'potion' | 'material' | 'special';

type LootItem = {
  name: string;
  rarity: Rarity;
  category: Category;
  dropChance: number; // in Prozent
};
```

---

### Master-Loot-Tabelle

```ts
const masterLootTable: LootItem[] = [
  // Weapons
  { name: 'Rusty Sword', rarity: 'common', category: 'weapon', dropChance: 40 },
  { name: 'Iron Axe', rarity: 'uncommon', category: 'weapon', dropChance: 30 },
  { name: 'Flame Bow', rarity: 'rare', category: 'weapon', dropChance: 20 },
  { name: 'Shadow Dagger', rarity: 'epic', category: 'weapon', dropChance: 7 },
  { name: 'Blade of Eternity', rarity: 'legendary', category: 'weapon', dropChance: 3 },

  // Armor
  { name: 'Leather Armor', rarity: 'common', category: 'armor', dropChance: 50 },
  { name: 'Iron Plate', rarity: 'uncommon', category: 'armor', dropChance: 25 },
  { name: 'Magic Robe', rarity: 'rare', category: 'armor', dropChance: 15 },
  { name: 'Dragon Scale Armor', rarity: 'epic', category: 'armor', dropChance: 7 },
  { name: 'Aegis of the Ancients', rarity: 'legendary', category: 'armor', dropChance: 3 },

  // Potions
  { name: 'Health Potion', rarity: 'common', category: 'potion', dropChance: 60 },
  { name: 'Mana Potion', rarity: 'common', category: 'potion', dropChance: 20 },
  { name: 'Stamina Elixir', rarity: 'uncommon', category: 'potion', dropChance: 10 },
  { name: 'Potion of Invincibility', rarity: 'epic', category: 'potion', dropChance: 5 },
  { name: 'Elixir of Time', rarity: 'legendary', category: 'potion', dropChance: 5 },

  // Materials
  { name: 'Iron Ore', rarity: 'common', category: 'material', dropChance: 40 },
  { name: 'Enchanted Wood', rarity: 'uncommon', category: 'material', dropChance: 30 },
  { name: 'Soul Shard', rarity: 'rare', category: 'material', dropChance: 20 },
  { name: 'Mystic Crystal', rarity: 'epic', category: 'material', dropChance: 7 },
  { name: 'Void Essence', rarity: 'legendary', category: 'material', dropChance: 3 },

  // Specials
  { name: 'Mystery Key', rarity: 'uncommon', category: 'special', dropChance: 50 },
  { name: 'Treasure Map', rarity: 'rare', category: 'special', dropChance: 25 },
  { name: 'Teleport Scroll', rarity: 'rare', category: 'special', dropChance: 15 },
  { name: 'Pet Egg', rarity: 'epic', category: 'special', dropChance: 7 },
  { name: 'Ancient Relic', rarity: 'legendary', category: 'special', dropChance: 3 },
];
```

---

### 🎰 Die Drop-Logik

```ts
function getLoot(table: LootItem[]): LootItem | null {
  const roll = Math.random() * 100;
  let accumulated = 0;

  for (const item of table) {
    accumulated += item.dropChance;
    if (roll <= accumulated) {
      return item;
    }
  }

  return null; // falls nichts passt
}
```

---

### 🔮 Generator: Zufällige Kategorie & Drop

```ts
function generateRandomLoot(): LootItem | null {
  const categories: Category[] = ['weapon', 'armor', 'potion', 'material', 'special'];
  const randomCategory = categories[Math.floor(Math.random() * categories.length)];

  const filteredTable = masterLootTable.filter(item => item.category === randomCategory);

  return getLoot(filteredTable);
}
```

---

### 🧪 Beispielausgabe

```ts
const loot = generateRandomLoot();
if (loot) {
  console.log(`🎉 Du hast ${loot.name} (${loot.rarity}) aus der Kategorie ${loot.category} gefunden!`);
} else {
  console.log('😢 Kein Loot diesmal...');
}
```


