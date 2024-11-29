---
title: GameDev 6
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 6
---


# 1

Hier ist die aktualisierte Version des Skripts. Der **"Attack"**-Button wird während des Zuges deaktiviert und die Textbox wird automatisch zum unteren Ende gescrollt, wenn Nachrichten hinzugefügt werden.

----------

### HTML

Unverändert, da keine neuen Änderungen im Aufbau erforderlich sind.

----------

### JavaScript

Die Logik für das Aktivieren/Deaktivieren des Buttons und das automatische Scrollen der Textbox wurde hinzugefügt.

```javascript
let messages;

const messageBuffer = [];
let isProcessingMessages = false;
let player, opponent;

// Spieler- und Gegnerdaten
function initializeBattleEntities() {
  player = {
    name: "Bulbasaur",
    hp: 20,
    maxHp: 20,
    attack: 6,
    moves: [
      { name: "Tackle", power: 4 },
      { name: "Vine Whip", power: 6 },
    ],
  };

  opponent = {
    name: "Charmander",
    hp: 18,
    maxHp: 18,
    attack: 7,
    moves: [
      { name: "Scratch", power: 5 },
      { name: "Ember", power: 7 },
    ],
  };
}

function typewriterEffect(element, text, delay = 50) {
  let i = 0;
  function type() {
    if (i < text.length) {
      element.textContent += text.charAt(i);
      i++;
      setTimeout(type, delay);
    } else {
      processNextMessage();
    }
  }
  type();
}

function updateMessages(text, style = '') {
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
  const { text, style } = messageBuffer.shift();
  const message = document.createElement('p');
  if (style) message.className = style;
  messages.appendChild(message);
  typewriterEffect(message, text);

  // Automatisch zum unteren Ende scrollen
  messages.scrollTop = messages.scrollHeight;
  const consoleLogs = document.getElementById("consoleLogs");
  consoleLogs.scrollTop = consoleLogs.scrollHeight;
}

function initMsg() {
  messages = document.getElementById('messages');
}

// Schadensberechnung
function calculateDamage(attacker, defender, move) {
  const baseDamage = attacker.attack + move.power - defender.attack / 2;
  return Math.max(1, Math.floor(baseDamage));
}

function opponentTurn() {
  const attackButton = document.getElementById("attack");
  attackButton.disabled = true; // Gegner ist am Zug

  setTimeout(() => {
    const randomMove = opponent.moves[Math.floor(Math.random() * opponent.moves.length)];
    const damage = calculateDamage(opponent, player, randomMove);

    player.hp = Math.max(0, player.hp - damage);
    updateMessages(
      `${opponent.name} used ${randomMove.name}! ${player.name} took ${damage} damage.`,
      "error"
    );

    if (player.hp <= 0) {
      updateMessages(`${player.name} fainted! ${opponent.name} wins!`, "error");
      disableMoveSelection();
    } else {
      attackButton.disabled = false; // Spieler ist wieder am Zug
    }
  }, 1000); // Gegnerzug verzögern, um Animation zu simulieren
}

function playerTurn(move) {
  const attackButton = document.getElementById("attack");
  attackButton.disabled = true; // Button deaktivieren während des Angriffs

  setTimeout(() => {
    const damage = calculateDamage(player, opponent, move);

    opponent.hp = Math.max(0, opponent.hp - damage);
    updateMessages(
      `${player.name} used ${move.name}! ${opponent.name} took ${damage} damage.`,
      "info"
    );

    if (opponent.hp <= 0) {
      updateMessages(`${opponent.name} fainted! ${player.name} wins!`, "info");
      disableMoveSelection();
    } else {
      opponentTurn(); // Gegnerzug ausführen
    }
  }, 1000); // Spielerzug verzögern, um Animation zu simulieren
}

// Move-Auswahl anzeigen
function enableMoveSelection() {
  const moveSelection = document.getElementById("moveSelection");
  moveSelection.style.display = "block";

  const movesDropdown = document.getElementById("moves");
  movesDropdown.innerHTML = "";

  player.moves.forEach((move, index) => {
    const option = document.createElement("option");
    option.value = index;
    option.textContent = move.name;
    movesDropdown.appendChild(option);
  });

  document.getElementById("attack").disabled = false; // Button aktivieren
}

function disableMoveSelection() {
  document.getElementById("moveSelection").style.display = "none";
}

// Kampf starten
function startBattle() {
  console.log("Battle started!");
  initializeBattleEntities();
  updateMessages("The battle has begun!", "info");
  updateMessages(`${player.name} vs. ${opponent.name}!`, "info");
  enableMoveSelection();
}

// Override console.log to redirect to textarea
(function () {
  const consoleLogs = document.getElementById("consoleLogs");
  const originalLog = console.log;

  console.log = function (...args) {
    originalLog(...args);
    consoleLogs.value += args.join(" ") + "\n";
    consoleLogs.scrollTop = consoleLogs.scrollHeight; // Automatisch scrollen
  };
})();

// Initialize messages and button event
window.onload = function () {
  initMsg();
  console.log("Console initialized.");
  updateMessages("Press the button to start the battle!", "info");

  document.getElementById("startBattle").addEventListener("click", startBattle);

  document.getElementById("attack").addEventListener("click", () => {
    const selectedMoveIndex = document.getElementById("moves").value;
    const selectedMove = player.moves[selectedMoveIndex];
    playerTurn(selectedMove);
  });
};

```

----------

### Änderungen:

1.  **Button-Status**:
    -   Der **"Attack"-Button** wird deaktiviert, wenn ein Zug verarbeitet wird (Spieler- oder Gegnerzug).
    -   Er wird wieder aktiviert, sobald der nächste Spielerzug beginnt.
2.  **Automatisches Scrollen**:
    -   Die Nachrichtenanzeige und die Textbox scrollen automatisch nach unten, wenn neue Inhalte hinzugefügt werden.
3.  **Zugverzögerungen**:
    -   Es gibt eine Verzögerung von 1 Sekunde zwischen den Zügen für ein besseres visuelles Erlebnis.

----------

### Nutzung:

1.  Öffne die HTML-Datei im Browser.
2.  Klicke auf **"Start Battle"**, um den Kampf zu starten.
3.  Wähle eine Attacke aus der Dropdown-Liste und drücke **"Attack"**.
4.  Beobachte, wie der Button während der Zugverarbeitung deaktiviert wird und die Anzeige automatisch nach unten scrollt.


# 2


Hier ist die Svelte-Komponente, die die beschriebene Kampfmechanik implementiert. Die HTML-Struktur und die Logik wurden in Svelte-Syntax übertragen, mit einem Fokus auf reaktives Verhalten.

----------

### `Battle.svelte`

```svelte
<script>
  import { onMount } from "svelte";

  let player = {
    name: "Bulbasaur",
    hp: 20,
    maxHp: 20,
    attack: 6,
    moves: [
      { name: "Tackle", power: 4 },
      { name: "Vine Whip", power: 6 },
    ],
  };

  let opponent = {
    name: "Charmander",
    hp: 18,
    maxHp: 18,
    attack: 7,
    moves: [
      { name: "Scratch", power: 5 },
      { name: "Ember", power: 7 },
    ],
  };

  let battleStarted = false;
  let messages = [];
  let messageBuffer = [];
  let isProcessingMessages = false;
  let selectedMoveIndex = 0;
  let attackButtonDisabled = true;

  function addMessage(text, style = "") {
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
    const { text, style } = messageBuffer.shift();
    messages = [...messages, { text, style }];
    scrollToBottom();
    setTimeout(() => processNextMessage(), 50);
  }

  function scrollToBottom() {
    const messagesContainer = document.getElementById("messages");
    if (messagesContainer) {
      messagesContainer.scrollTop = messagesContainer.scrollHeight;
    }
  }

  function calculateDamage(attacker, defender, move) {
    const baseDamage = attacker.attack + move.power - defender.attack / 2;
    return Math.max(1, Math.floor(baseDamage));
  }

  function opponentTurn() {
    attackButtonDisabled = true;
    setTimeout(() => {
      const randomMove = opponent.moves[Math.floor(Math.random() * opponent.moves.length)];
      const damage = calculateDamage(opponent, player, randomMove);

      player.hp = Math.max(0, player.hp - damage);
      addMessage(`${opponent.name} used ${randomMove.name}! ${player.name} took ${damage} damage.`, "error");

      if (player.hp <= 0) {
        addMessage(`${player.name} fainted! ${opponent.name} wins!`, "error");
        attackButtonDisabled = true;
      } else {
        attackButtonDisabled = false;
      }
    }, 1000);
  }

  function playerTurn() {
    const move = player.moves[selectedMoveIndex];
    attackButtonDisabled = true;

    setTimeout(() => {
      const damage = calculateDamage(player, opponent, move);

      opponent.hp = Math.max(0, opponent.hp - damage);
      addMessage(`${player.name} used ${move.name}! ${opponent.name} took ${damage} damage.`, "info");

      if (opponent.hp <= 0) {
        addMessage(`${opponent.name} fainted! ${player.name} wins!`, "info");
        attackButtonDisabled = true;
      } else {
        opponentTurn();
      }
    }, 1000);
  }

  function startBattle() {
    battleStarted = true;
    messages = [];
    messageBuffer = [];
    isProcessingMessages = false;
    attackButtonDisabled = false;
    player.hp = player.maxHp;
    opponent.hp = opponent.maxHp;

    addMessage("The battle has begun!", "info");
    addMessage(`${player.name} vs. ${opponent.name}!`, "info");
  }

  onMount(() => {
    addMessage("Press the button to start the battle!", "info");
  });
</script>

<style>
  #messages {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    height: 200px;
    overflow-y: auto;
    background-color: #f9f9f9;
  }

  .info {
    color: blue;
  }

  .error {
    color: red;
  }

  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
</style>

<div>
  <h1>Pokemon Battle</h1>
  {#if !battleStarted}
    <button on:click={startBattle}>Start Battle</button>
  {/if}

  {#if battleStarted}
    <div>
      <label for="moves">Choose an attack:</label>
      <select id="moves" bind:value={selectedMoveIndex}>
        {#each player.moves as move, index}
          <option value={index}>{move.name}</option>
        {/each}
      </select>
      <button id="attack" on:click={playerTurn} disabled={attackButtonDisabled}>
        Attack
      </button>
    </div>
  {/if}

  <div id="messages">
    {#each messages as { text, style }}
      <p class={style}>{text}</p>
    {/each}
  </div>
</div>

```

----------

### Änderungen gegenüber der ursprünglichen Version:

1.  **Reaktives Verhalten**:
    -   Variablen wie `battleStarted`, `messages`, und `attackButtonDisabled` steuern die Anzeige und Interaktivität in Echtzeit.
2.  **Event-Handling**:
    -   Buttons und Dropdowns nutzen `on:click` und `bind:value` für Benutzerinteraktionen.
3.  **Svelte Syntax**:
    -   Bedingte Blöcke (`{#if}`), Schleifen (`{#each}`) und Styles wurden in die Komponente integriert.

----------

### Nutzung:

1.  Erstelle ein neues Svelte-Projekt (`npm create vite@latest your-project-name -- --template svelte`).
2.  Füge die Komponente in eine neue Datei (`Battle.svelte`) ein.
3.  Importiere und binde die Komponente in `App.svelte` ein:
    
    ```svelte
    <script>
      import Battle from './Battle.svelte';
    </script>
    
    <Battle />
    
    ```
    
4.  Starte das Projekt (`npm run dev`) und öffne die App im Browser.




# 3




Hier ist die aktualisierte Version der Svelte-Komponente mit einer Funktion zum erneuten Starten des Kampfes. Nach dem Beenden des Kampfes wird ein **"Restart Battle"**-Button angezeigt, und das Nachrichtenfeld wird geleert.

----------

### Aktualisierte `Battle.svelte`

```svelte
<script>
  import { onMount } from "svelte";

  let player = {
    name: "Bulbasaur",
    hp: 20,
    maxHp: 20,
    attack: 6,
    moves: [
      { name: "Tackle", power: 4 },
      { name: "Vine Whip", power: 6 },
    ],
  };

  let opponent = {
    name: "Charmander",
    hp: 18,
    maxHp: 18,
    attack: 7,
    moves: [
      { name: "Scratch", power: 5 },
      { name: "Ember", power: 7 },
    ],
  };

  let battleStarted = false;
  let battleEnded = false;
  let messages = [];
  let messageBuffer = [];
  let isProcessingMessages = false;
  let selectedMoveIndex = 0;
  let attackButtonDisabled = true;

  function addMessage(text, style = "") {
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
    const { text, style } = messageBuffer.shift();
    messages = [...messages, { text, style }];
    scrollToBottom();
    setTimeout(() => processNextMessage(), 50);
  }

  function scrollToBottom() {
    const messagesContainer = document.getElementById("messages");
    if (messagesContainer) {
      messagesContainer.scrollTop = messagesContainer.scrollHeight;
    }
  }

  function calculateDamage(attacker, defender, move) {
    const baseDamage = attacker.attack + move.power - defender.attack / 2;
    return Math.max(1, Math.floor(baseDamage));
  }

  function opponentTurn() {
    attackButtonDisabled = true;
    setTimeout(() => {
      const randomMove = opponent.moves[Math.floor(Math.random() * opponent.moves.length)];
      const damage = calculateDamage(opponent, player, randomMove);

      player.hp = Math.max(0, player.hp - damage);
      addMessage(`${opponent.name} used ${randomMove.name}! ${player.name} took ${damage} damage.`, "error");

      if (player.hp <= 0) {
        addMessage(`${player.name} fainted! ${opponent.name} wins!`, "error");
        endBattle();
      } else {
        attackButtonDisabled = false;
      }
    }, 1000);
  }

  function playerTurn() {
    const move = player.moves[selectedMoveIndex];
    attackButtonDisabled = true;

    setTimeout(() => {
      const damage = calculateDamage(player, opponent, move);

      opponent.hp = Math.max(0, opponent.hp - damage);
      addMessage(`${player.name} used ${move.name}! ${opponent.name} took ${damage} damage.`, "info");

      if (opponent.hp <= 0) {
        addMessage(`${opponent.name} fainted! ${player.name} wins!`, "info");
        endBattle();
      } else {
        opponentTurn();
      }
    }, 1000);
  }

  function startBattle() {
    battleStarted = true;
    battleEnded = false;
    messages = [];
    messageBuffer = [];
    isProcessingMessages = false;
    attackButtonDisabled = false;
    player.hp = player.maxHp;
    opponent.hp = opponent.maxHp;

    addMessage("The battle has begun!", "info");
    addMessage(`${player.name} vs. ${opponent.name}!`, "info");
  }

  function endBattle() {
    battleEnded = true;
    attackButtonDisabled = true;
  }

  function restartBattle() {
    startBattle();
  }

  onMount(() => {
    addMessage("Press the button to start the battle!", "info");
  });
</script>

<style>
  #messages {
    margin-top: 20px;
    padding: 10px;
    border: 1px solid #ccc;
    height: 200px;
    overflow-y: auto;
    background-color: #f9f9f9;
  }

  .info {
    color: blue;
  }

  .error {
    color: red;
  }

  button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
</style>

<div>
  <h1>Pokemon Battle</h1>
  {#if !battleStarted}
    <button on:click={startBattle}>Start Battle</button>
  {/if}

  {#if battleStarted && !battleEnded}
    <div>
      <label for="moves">Choose an attack:</label>
      <select id="moves" bind:value={selectedMoveIndex}>
        {#each player.moves as move, index}
          <option value={index}>{move.name}</option>
        {/each}
      </select>
      <button id="attack" on:click={playerTurn} disabled={attackButtonDisabled}>
        Attack
      </button>
    </div>
  {/if}

  {#if battleEnded}
    <button on:click={restartBattle}>Restart Battle</button>
  {/if}

  <div id="messages">
    {#each messages as { text, style }}
      <p class={style}>{text}</p>
    {/each}
  </div>
</div>

```

----------

### Änderungen:

1.  **Neuer Zustand `battleEnded`**:
    -   Kontrolliert, ob der Kampf beendet ist und der **"Restart Battle"**-Button angezeigt wird.
2.  **Nachrichtenfeld zurücksetzen**:
    -   Beim Neustarten des Kampfes wird das `messages`-Array geleert.
3.  **"Restart Battle"-Button**:
    -   Dieser Button wird nach dem Kampf angezeigt und startet den Kampf neu, indem die `restartBattle`-Funktion aufgerufen wird.

----------

### Nutzung:

1.  Beginne den Kampf mit dem **"Start Battle"**-Button.
2.  Kämpfe durch die Auswahl von Attacken.
3.  Nach dem Kampf (Sieg oder Niederlage) erscheint der **"Restart Battle"**-Button.
4.  Klicke auf **"Restart Battle"**, um einen neuen Kampf zu starten.
