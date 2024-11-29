---
title: GameDev 5
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 5
---


# 0



Hier ist eine ES6-Klasse, die mit dem `resourcen`-Objekt arbeitet und die `total`-Werte bei einem Klick um den entsprechenden `clickInc` erhöht:

```javascript
class ResourceManager {
  constructor(resources) {
    this.resources = resources;
  }

  click(resourceKey) {
    const resource = this.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return;
    }

    if (resource.total + resource.clickInc <= resource.max) {
      resource.total += resource.clickInc;
      console.log(`${resource.name}: Total is now ${resource.total}`);
    } else {
      console.log(`${resource.name} has reached its maximum capacity.`);
    }
  }

  getResourceTotal(resourceKey) {
    const resource = this.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return null;
    }
    return resource.total;
  }
}

// Beispielnutzung
const resourceManager = new ResourceManager(resourcen);

// Klick auf Ressourcen
resourceManager.click('wood'); // Wood: Total is now 1
resourceManager.click('stone'); // Stone: Total is now 1

// Gesamtwert einer Ressource abrufen
console.log(resourceManager.getResourceTotal('iron')); // 0

```

### Erklärung:

1.  **Konstruktor**:
    
    -   Nimmt das `resourcen`-Objekt entgegen und speichert es.
2.  **`click`-Methode**:
    
    -   Findet die Ressource mit dem übergebenen Key.
    -   Erhöht den `total`-Wert der Ressource um `clickInc`, solange die `max`-Kapazität nicht überschritten wird.
    -   Gibt eine Fehlermeldung aus, falls der Key nicht existiert.
3.  **`getResourceTotal`-Methode**:
    
    -   Liefert den aktuellen `total`-Wert einer Ressource zurück.

Wenn du Erweiterungen oder Änderungen benötigst, lass es mich wissen!












# 1

Um die Klasse `ResourceManager` in einem Svelte 5-Projekt zu nutzen, kannst du sie einfach in einer separaten JavaScript-Datei definieren und dann in deinen Svelte-Komponenten importieren. Hier ist eine Anleitung, wie du das machen kannst:

### 1. **Ressourcenklasse exportieren**

Speichere die `ResourceManager`-Klasse in einer eigenen Datei, z. B. `ResourceManager.js`:

```javascript
// ResourceManager.js
export default class ResourceManager {
  constructor(resources) {
    this.resources = resources;
  }

  click(resourceKey) {
    const resource = this.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return;
    }

    if (resource.total + resource.clickInc <= resource.max) {
      resource.total += resource.clickInc;
      console.log(`${resource.name}: Total is now ${resource.total}`);
    } else {
      console.log(`${resource.name} has reached its maximum capacity.`);
    }
  }

  getResourceTotal(resourceKey) {
    const resource = this.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return null;
    }
    return resource.total;
  }
}

```

### 2. **Svelte-Komponente erstellen**

In deiner Svelte-Datei (z. B. `App.svelte`) kannst du die Klasse importieren und nutzen:

```svelte
<script>
  import ResourceManager from './ResourceManager.js';
  import { onMount } from 'svelte';

  // Ressourcen-Objekt importieren oder definieren
  import { resourcen } from './resourcen.js';

  let resourceManager;

  // ResourceManager initialisieren
  onMount(() => {
    resourceManager = new ResourceManager(resourcen);
  });

  // Funktion für Klicks
  const handleClick = (resourceKey) => {
    resourceManager.click(resourceKey);
  };
</script>

<main>
  <h1>Clicker Game</h1>
  <div>
    {#each Object.keys(resourcen) as resourceKey}
      <div>
        <h2>{resourcen[resourceKey].name}</h2>
        <p>{resourcen[resourceKey].description}</p>
        <p>Total: {resourcen[resourceKey].total}</p>
        <button on:click={() => handleClick(resourceKey)}>
          {resourcen[resourceKey].action || 'Collect'}
        </button>
      </div>
    {/each}
  </div>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
  }

  button {
    margin: 5px 0;
    padding: 5px 10px;
    background-color: #3498db;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }

  button:hover {
    background-color: #2980b9;
  }
</style>

```

### 3. **Ressourcen-Datei bereitstellen**

Wenn dein `resourcen`-Objekt (aus deiner hochgeladenen Datei) nicht direkt verfügbar ist, speichere es in `resourcen.js` und exportiere es:

```javascript
// resourcen.js
export const resourcen = {
  wood: {
    name: 'Wood',
    description: 'Brown stuff that grows on trees.',
    action: 'Chop',
    total: 0,
    clickInc: 1,
    autoInc: 0,
    max: 100,
  },
  stone: {
    name: 'Stone',
    description: 'Hard stuff.',
    action: 'Mine',
    total: 0,
    clickInc: 1,
    autoInc: 0,
    max: 100,
  },
  iron: {
    name: 'Iron',
    description: 'Even harder stuff. Bit rusty.',
    total: 0,
    clickInc: 1,
    autoInc: 0,
    max: 100,
  },
  food: {
    name: 'Food',
    description: 'Can be cooked to provide nutrition.',
    action: 'Hunt',
    total: 0,
    clickInc: 1,
    autoInc: 0,
    max: 100,
  },
};

```

### 4. **Nutzung**

-   Starte dein Svelte-Projekt (z. B. mit Vite).
-   Das Clicker-Game zeigt die Ressourcen an und aktualisiert den `total`-Wert, wenn du auf die Buttons klickst.

Falls du noch Tailwind CSS nutzt, kannst du die Styles leicht anpassen, um das Layout und Design weiter zu verbessern.




# 2

Verstanden! Wenn du `$state()` direkt im Konstruktor nutzen möchtest, um einen reaktiven Zustand zu erstellen, kann das folgendermaßen umgesetzt werden:

### Angepasste `ResourceManager`-Klasse mit `$state()`

```javascript
export default class ResourceManager {
  // Reaktiver Zustand mit $state
  $state = {
    resources: {}
  };

  constructor(initialResources) {
    this.$state.resources = initialResources;
  }

  click(resourceKey) {
    const resource = this.$state.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return;
    }

    if (resource.total + resource.clickInc <= resource.max) {
      resource.total += resource.clickInc;
    } else {
      console.log(`${resource.name} has reached its maximum capacity.`);
    }
  }

  getResourceTotal(resourceKey) {
    const resource = this.$state.resources[resourceKey];
    if (!resource) {
      console.error(`Resource "${resourceKey}" not found.`);
      return null;
    }
    return resource.total;
  }
}

```

### Nutzung in Svelte 5

Du kannst die Klasse und `$state` direkt in einer Svelte-Komponente nutzen. So sieht ein Beispiel aus:

#### `App.svelte`

```svelte
<script>
  import ResourceManager from './ResourceManager.js';
  import { resourcen } from './resourcen.js';

  let resourceManager = new ResourceManager(resourcen);

  // Funktion, um auf Ressourcen zu klicken
  const handleClick = (resourceKey) => {
    resourceManager.click(resourceKey);
  };
</script>

<main>
  <h1>Clicker Game</h1>
  <div>
    {#each Object.keys(resourceManager.$state.resources) as resourceKey}
      <div>
        <h2>{resourceManager.$state.resources[resourceKey].name}</h2>
        <p>{resourceManager.$state.resources[resourceKey].description}</p>
        <p>Total: {resourceManager.$state.resources[resourceKey].total}</p>
        <button on:click={() => handleClick(resourceKey)}>
          {resourceManager.$state.resources[resourceKey].action || 'Collect'}
        </button>
      </div>
    {/each}
  </div>
</main>

<style>
  main {
    font-family: Arial, sans-serif;
    max-width: 600px;
    margin: 0 auto;
  }

  button {
    margin: 5px 0;
    padding: 5px 10px;
    background-color: #3498db;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 5px;
  }

  button:hover {
    background-color: #2980b9;
  }
</style>

```

### Erklärung:

1.  **`$state`**:
    
    -   Die Klasse `ResourceManager` verwendet `$state`, um einen reaktiven Zustand für die Ressourcen zu erstellen.
    -   Änderungen in `resourceManager.$state.resources` werden automatisch in der Svelte-Komponente reflektiert.
2.  **Svelte-Komponente**:
    
    -   Durch Schleifen über `Object.keys(resourceManager.$state.resources)` kannst du alle Ressourcen dynamisch rendern.
    -   Änderungen durch `click`-Methoden der Klasse werden direkt in der UI sichtbar.
3.  **Interaktivität**:
    
    -   Beim Klick auf einen Button wird die `click`-Methode aufgerufen, die den `total`-Wert der Ressource erhöht.
    -   Der geänderte Zustand wird automatisch in der UI aktualisiert.

Das Ergebnis ist ein reaktives Clicker-Game mit sauberer Trennung zwischen Logik (Klasse) und UI (Svelte).
