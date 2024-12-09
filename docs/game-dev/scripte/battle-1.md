---
title: Battle 1
sidebar_label: Battle 1
sidebar_position: 81
---


## Svelte Component



```html title="WildPokemonGame.svelte"
<script lang="ts">
  import { onMount } from "svelte";

  interface Sprites {
    front: string;
    back: string;
  }

  interface Message {
    text: string;
    style: string;
  }

  class WildPokemon {
    private type: string;
    private _level: number;
    private _health: number;
    private sprites: Sprites;

    constructor(type: string, level: number, health: number, sprites: Sprites) {
      this.type = type;
      this._level = level;
      this._health = health;
      this.sprites = sprites;
    }

    get level(): number {
      return this._level;
    }

    set level(newLevel: number) {
      this._level = Math.max(0, newLevel);
    }

    get health(): number {
      return this._health;
    }

    set health(newHealth: number) {
      this._health = Math.max(0, newHealth);
    }

    get frontSprite(): string {
      return this.sprites.front;
    }

    get backSprite(): string {
      return this.sprites.back;
    }
  }

  class Move {
    move: string;
    baseDmg: number;
    baseDice: number;

    constructor(move: string, baseDmg: number, baseDice: number) {
      this.move = move;
      this.baseDmg = baseDmg;
      this.baseDice = baseDice;
    }
  }

  const wildPokemon: WildPokemon[] = [
    new WildPokemon("Quilava", 6, 9, {
      front:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/156.png",
      back: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/156.png",
    }),
    new WildPokemon("Rhydon", 6, 9, {
      front:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/112.png",
      back: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/112.png",
    }),
    new WildPokemon("Makuhita", 7, 9, {
      front:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/296.png",
      back: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/296.png",
    }),
    new WildPokemon("Mew", 8, 9, {
      front:
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/151.png",
      back: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/back/151.png",
    }),
  ];

  const moves: Move[] = [
    new Move("Slam", 5, 6),
    new Move("Headbutt", 4, 4),
    new Move("Tackle", 3, 4),
    new Move("Cut", 4, 4),
  ];

  let wildPokemonId: number;
  let moveId: number;
  let damage: number = 0;
  let playerMove: number;
  let playerTurn = false;
  let pokemonHealth: number = 50;
  let messageBuffer: Message[] = [];
  let isProcessingMessages = false;

  let messages: Message[] = [];
  let actionButtons: { text: string; onClick: () => void }[] = [];

  function randomPokemon() {
    wildPokemonId = Math.floor(Math.random() * wildPokemon.length);
    moveId = Math.floor(Math.random() * moves.length);
  }

  function callMoveDamage() {
    damage = Math.floor(Math.random() * moves[moveId].baseDmg + 3);
  }

  function callPlayerMoveDamage() {
    damage = Math.floor(Math.random() * moves[playerMove].baseDmg + 3);
  }

  function attackLoop() {
    if (!playerTurn) {
      playerTurn = true;
      callMoveDamage();
      wildPokemonAttack();
    } else {
      playerTurn = false;
      selectMove();
    }
  }

  function selectMove() {
    actionButtons = moves.map((move, index) => ({
      text: move.move,
      onClick: () => {
        playerMove = index;
        callPlayerMoveDamage();
        playerAttack();
      },
    }));
  }

  function wildPokemonAttack() {
    if (pokemonHealth > 0) {
      pokemonHealth -= damage;
      updateMessages(
        `Wild ${wildPokemon[wildPokemonId].type} uses ${moves[moveId].move} dealing ${damage} damage!`,
        "text-error"
      );
      updateMessages(`Player has ${pokemonHealth} health remaining!`, "text-info");
      attackLoop();
    } else {
      wildPokemonKo();
    }
  }

  function playerAttack() {
    updateMessages(
      `Player uses ${moves[playerMove].move} dealing ${damage} damage!`,
      "font-bold"
    );
    if (wildPokemon[wildPokemonId].health > 0) {
      wildPokemon[wildPokemonId].health -= damage;
      updateMessages(
        `${wildPokemon[wildPokemonId].type} has ${wildPokemon[wildPokemonId].health} health remaining!`,
        "text-info"
      );
      playerTurn = false;
      attackLoop();
    } else {
      wildPokemonKo();
    }
  }

  function wildPokemonKo() {
    updateMessages(`${wildPokemon[wildPokemonId].type} is K.O.`, "text-success");
    actionButtons = [{ text: "Restart", onClick: restartFight }];
  }

  function updateMessages(text: string, style: string = "") {
    messageBuffer.push({ text, style });
    if (!isProcessingMessages) {
      processNextMessage();
    }
  }

  function processNextMessage() {
    if (messageBuffer.length === 0) {
      isProcessingMessages = false;
      return;
    }

    isProcessingMessages = true;
    const { text, style } = messageBuffer.shift()!;
    messages = [...messages, { text, style }];
    setTimeout(() => processNextMessage(), 1000);
  }

  function restartFight() {
    messages = [];
    actionButtons = [];
    pokemonHealth = 50;
    randomPokemon();
    startFight();
  }

  function startFight() {
    randomPokemon();
    updateMessages(`Wild ${wildPokemon[wildPokemonId].type} appears!`, "heading");
    updateMessages(
      `Level : ${wildPokemon[wildPokemonId].level} Health : ${wildPokemon[wildPokemonId].health}`,
      "sub-heading"
    );
    playerTurn = true;
    attackLoop();
  }

  onMount(() => {
    startFight();
  });
</script>

<div id="messages">
  {#each messages as { text, style }}
    <p class={style}>{text}</p>
  {/each}
</div>

<div id="actions">
  {#each actionButtons as { text, onClick }}
    <button on:click={onClick}>{text}</button>
  {/each}
</div>

<style>
  button {
    margin: 0.5rem;
  }
</style>
```