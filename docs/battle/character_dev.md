---
title: Battle Character
sidebar_position: 5
sidebar_label: Battle Char
---



```javascript title="fighter.js"
const enemyObj = {
	name: 'Eevee',
	health: 30,
	maxHealth: 30,
	hardAttackDamage: 5,
	hardAttackDice: 5,
	weakAttackDamage: 3,
	weakAttackDice: 3
}

const playObj = {
	name: 'Pikachu',
	health: 30,
	maxHealth: 30,
	hardAttackDamage: 5,
	hardAttackDice: 5,
	weakAttackDamage: 3,
	weakAttackDice: 3,
	attacks: {
		mace: {
			name: 'Mace Attack',
			damage: 6,
			dice: 3,
			msg: 'You are trying hit the enemy with a huge mace ...',
			txt: 'The enemy dodges your attack ...'
		},
		shield: {
			name: 'Shield Bash',
			damage: 3,
			dice: 3,
			msg: 'You are trying to bash the opponent away with your shield',
			txt: 'The enemy dodges your attack ...'
		}
	}
}

```

## Opponent

```javascript title="fighter.js"
const enemyObj = {
	name: 'Eevee',
	health: 30,
	maxHealth: 30,
	hardAttackDamage: 5,
	hardAttackDice: 5,
	weakAttackDamage: 3,
	weakAttackDice: 3
}

```
