---
title: Resource Manager
sidebar_label: Idle 1
sidebar_position: 92
---


## Svelte Component



```html title="ResourceManagement.svelte"
<script>
  import { onMount } from 'svelte';

  let resources = {
    wood: 3,
    food: 35,
  };

  let population = {
    ready: 15,
    hungry: 0,
    sick: 0,
    hurt: 0,
  };

  const achievements = {
    carpentry: {
      cost: {
        wood: 10,
        food: 10,
        people: 4,
      },
    },
    shipyard: {
      requires: ['carpentry'],
    },
  };

  const DAY = 10000;
  const date = new Date('1499/05/13');

  let logs = [];

  const log = (text, color) => {
    logs = [{ text, color }, ...logs];
  };

  const fetchWood = () => {
    population.ready -= 2;
    setTimeout(bring('wood', 2, 5, 0.05), DAY * 0.5);
    log('🌳👬 2 people set off to bring wood.');
    updateView();
  };

  const forage = () => {
    population.ready -= 2;
    setTimeout(bring('food', 2, 4, 0), DAY * 0.3);
    log('🌾👬 2 people have gone foraging.');
    updateView();
  };

  const hunt = () => {
    population.ready -= 4;
    setTimeout(bring('food', 4, 12, 0.1), DAY * 0.6);
    log('🏹👨‍👩‍👦‍👦 4 hunters left to bring food.');
    updateView();
  };

  const bring = (resource, partySize, amount, risk) => () => {
    if (Math.random() > risk) {
      log(`🌟 A party of ${partySize} has returned with ${amount} ${resource} successfully.`);
      resources[resource] += amount;
      population.ready += partySize;
    } else {
      log(`💀 A party of ${partySize} returned from fetching ${resource}, but got attacked by wild animals. 1 person died`, 'red');
      resources[resource] += Math.floor(amount / 2);
      population.ready += partySize - 1;
    }
    updateView();
  };

  const updateDate = () => {
    date.setDate(date.getDate() + 1);
  };

  const nextDay = () => {
    updateDate();

    if (population.ready + population.hungry < 1) {
      log('💀💀💀 Your population was decimated', 'red');
      stop();
    }

    if (population.hungry > 0) {
      population.hungry -= 1;
      log('💀 One person has died from starvation. +5 food.', 'red');
      resources.food += 5;
    }

    population.ready += population.hungry;
    population.hungry = 0;

    resources.food -= population.ready;
    if (resources.food < 0) {
      population.ready += resources.food;
      population.hungry += -resources.food;
      resources.food = 0;
      log(`😞 Due to lack of food, ${population.hungry} are starving and can't work.`, 'red');
    }

    updateView();
  };

  const updateView = () => {
    // Trigger Svelte reactivity
    resources = { ...resources };
    population = { ...population };
  };

  let dayInterval;

  const stop = () => {
    clearInterval(dayInterval);
  };

  onMount(() => {
    dayInterval = setInterval(nextDay, DAY);
    updateView();
    return stop;
  });
</script>

<style>
  .log {
    color: inherit;
    font-size: 0.9em;
    margin: 5px 0;
  }

  .log.red {
    color: red;
  }
</style>

<div>
  <h1>Resource Management</h1>

  <div id="resources">
    <p id="wood">Wood: {resources.wood}</p>
    <p id="food">Food: {resources.food}</p>
  </div>

  <div id="population">
    <p>Population Ready: {population.ready}</p>
    <p id="hungry">Hungry: {population.hungry}</p>
  </div>

  <div>
    <button id="chop-wood" on:click={fetchWood} disabled={population.ready < 2}>Chop Wood</button>
    <button id="forage" on:click={forage} disabled={population.ready < 2}>Forage</button>
    <button id="hunt" on:click={hunt} disabled={population.ready < 4}>Hunt</button>
  </div>

  <div id="log">
    {#each logs as log}
      <p class="log {log.color}">{log.text}</p>
    {/each}
  </div>
</div>
```