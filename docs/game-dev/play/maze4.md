---
title: GameDev 4
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 4
---

Das klingt nach einem spannenden Projekt! Ich werde dir eine grundlegende Struktur in TypeScript bereitstellen, die dir den Einstieg erleichtert. In diesem Clicker-Game wirst du verschiedene Aspekte deines Nachtclubs managen, die dann die drei Hauptwerte beeinflussen: Ambiente, Unterhaltung und Personal.

### Grundlegende Struktur

1.  **HTML**: Basis-HTML-Struktur für das Spiel.
2.  **CSS**: Einfache Styles, um das Spiel optisch ansprechend zu gestalten.
3.  **TypeScript**: Die Spiel-Logik, die die Werte und Interaktionen verwaltet.

### 1. HTML

Erstelle eine Datei `index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nachtclub Management Clicker Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game">
        <h1>Nachtclub Management</h1>
        <div id="stats">
            <p>Ambiente: <span id="ambiente">0</span></p>
            <p>Unterhaltung: <span id="unterhaltung">0</span></p>
            <p>Personal: <span id="personal">0</span></p>
        </div>
        <div id="controls">
            <button id="upgrade-ambiente">Ambiente verbessern</button>
            <button id="upgrade-unterhaltung">Unterhaltung verbessern</button>
            <button id="upgrade-personal">Personal verbessern</button>
        </div>
    </div>
    <script src="app.js"></script>
</body>
</html>

```

### 2. CSS

Erstelle eine Datei `style.css`:

```css
body {
    font-family: Arial, sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #222;
    color: #fff;
}

#game {
    text-align: center;
}

button {
    margin: 5px;
    padding: 10px 20px;
    border: none;
    background-color: #444;
    color: #fff;
    cursor: pointer;
}

button:hover {
    background-color: #555;
}

```

### 3. TypeScript

Erstelle eine Datei `app.ts`:

```typescript
class Nachtclub {
    ambiente: number;
    unterhaltung: number;
    personal: number;

    constructor() {
        this.ambiente = 0;
        this.unterhaltung = 0;
        this.personal = 0;
    }

    upgradeAmbiente() {
        this.ambiente += 1;
        this.updateUI();
    }

    upgradeUnterhaltung() {
        this.unterhaltung += 1;
        this.updateUI();
    }

    upgradePersonal() {
        this.personal += 1;
        this.updateUI();
    }

    updateUI() {
        (document.getElementById('ambiente') as HTMLSpanElement).innerText = this.ambiente.toString();
        (document.getElementById('unterhaltung') as HTMLSpanElement).innerText = this.unterhaltung.toString();
        (document.getElementById('personal') as HTMLSpanElement).innerText = this.personal.toString();
    }
}

const club = new Nachtclub();

document.getElementById('upgrade-ambiente')!.addEventListener('click', () => {
    club.upgradeAmbiente();
});

document.getElementById('upgrade-unterhaltung')!.addEventListener('click', () => {
    club.upgradeUnterhaltung();
});

document.getElementById('upgrade-personal')!.addEventListener('click', () => {
    club.upgradePersonal();
});

club.updateUI();

```

### TypeScript Kompilieren

Du musst die TypeScript-Datei in JavaScript kompilieren. Dazu kannst du den TypeScript-Compiler (`tsc`) verwenden. Stelle sicher, dass du TypeScript installiert hast und führe dann im Terminal folgenden Befehl aus:

```bash
tsc app.ts

```

Dies wird eine `app.js`-Datei erstellen, die du in deiner HTML-Datei eingebunden hast.

### Fazit

Mit diesen Dateien hast du eine einfache Grundlage für dein Nachtclub-Management-Clicker-Game. Du kannst diese weiter ausbauen, indem du z.B. Upgrades, Ereignisse oder weitere Spielmechaniken hinzufügst. Viel Erfolg und Spaß beim Programmieren!



Natürlich! Ich werde die Struktur erweitern, um die Tage zu zählen und ein Auszahlungssystem hinzuzufügen. Wir fügen eine Geldverwaltung und ein Zeitintervall hinzu, das jeden Tag simuliert.

### 1. HTML

Ergänze die HTML-Datei `index.html` um eine Anzeige für Geld und Tage:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nachtclub Management Clicker Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game">
        <h1>Nachtclub Management</h1>
        <div id="stats">
            <p>Ambiente: <span id="ambiente">0</span></p>
            <p>Unterhaltung: <span id="unterhaltung">0</span></p>
            <p>Personal: <span id="personal">0</span></p>
            <p>Geld: <span id="geld">0</span></p>
            <p>Tag: <span id="tage">1</span></p>
        </div>
        <div id="controls">
            <button id="upgrade-ambiente">Ambiente verbessern</button>
            <button id="upgrade-unterhaltung">Unterhaltung verbessern</button>
            <button id="upgrade-personal">Personal verbessern</button>
        </div>
    </div>
    <script src="app.js"></script>
</body>
</html>

```

### 2. TypeScript

Ergänze die Datei `app.ts` um die Tagesverwaltung und das Geldsystem:

```typescript
class Nachtclub {
    ambiente: number;
    unterhaltung: number;
    personal: number;
    geld: number;
    tage: number;
    tagDauer: number;

    constructor() {
        this.ambiente = 0;
        this.unterhaltung = 0;
        this.personal = 0;
        this.geld = 0;
        this.tage = 1;
        this.tagDauer = 3 * 60 * 1000; // 3 Minuten in Millisekunden
        this.startTagesIntervall();
    }

    upgradeAmbiente() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.ambiente += 1;
            this.updateUI();
        }
    }

    upgradeUnterhaltung() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.unterhaltung += 1;
            this.updateUI();
        }
    }

    upgradePersonal() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.personal += 1;
            this.updateUI();
        }
    }

    berechneEinnahmen() {
        return this.ambiente * 5 + this.unterhaltung * 10 + this.personal * 8; // Beispielwerte
    }

    naechsterTag() {
        this.tage += 1;
        this.geld += this.berechneEinnahmen();
        this.updateUI();
    }

    startTagesIntervall() {
        setInterval(() => {
            this.naechsterTag();
        }, this.tagDauer);
    }

    updateUI() {
        (document.getElementById('ambiente') as HTMLSpanElement).innerText = this.ambiente.toString();
        (document.getElementById('unterhaltung') as HTMLSpanElement).innerText = this.unterhaltung.toString();
        (document.getElementById('personal') as HTMLSpanElement).innerText = this.personal.toString();
        (document.getElementById('geld') as HTMLSpanElement).innerText = this.geld.toString();
        (document.getElementById('tage') as HTMLSpanElement).innerText = this.tage.toString();
    }
}

const club = new Nachtclub();

document.getElementById('upgrade-ambiente')!.addEventListener('click', () => {
    club.upgradeAmbiente();
});

document.getElementById('upgrade-unterhaltung')!.addEventListener('click', () => {
    club.upgradeUnterhaltung();
});

document.getElementById('upgrade-personal')!.addEventListener('click', () => {
    club.upgradePersonal();
});

club.updateUI();

```

### Erklärung

-   **Geldverwaltung**: Die Upgrades kosten jetzt Geld (z.B. 10 Einheiten).
-   **Tagesintervall**: Alle 3 Minuten (die in Millisekunden umgerechnet sind) wird ein neuer Tag simuliert, und der Club erhält Einnahmen basierend auf den drei Werten.
-   **Einnahmeberechnung**: Die Einnahmen werden auf Grundlage der drei Werte berechnet. Diese Formel kann an deine Bedürfnisse angepasst werden.

Dieses Grundgerüst bietet dir eine solide Basis, auf der du weiter aufbauen kannst. Du kannst weitere Funktionen hinzufügen, wie z.B. zufällige Ereignisse, zusätzliche Upgrades oder erweiterte Einnahmeberechnungen. Viel Spaß beim Programmieren!

Verstanden, du möchtest also einen Countdown-Timer anzeigen, der den verbleibenden Tageszeitraum visuell darstellt. Der Timer soll von 3 Minuten herunterzählen und der Range-Slider soll den aktuellen Fortschritt anzeigen. Hier ist, wie du das umsetzen kannst:

### 1. HTML

Ergänze die HTML-Datei `index.html` um das Range-Element, das als Countdown-Timer dient:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Nachtclub Management Clicker Game</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="game">
        <h1>Nachtclub Management</h1>
        <div id="stats">
            <p>Ambiente: <span id="ambiente">0</span></p>
            <p>Unterhaltung: <span id="unterhaltung">0</span></p>
            <p>Personal: <span id="personal">0</span></p>
            <p>Geld: <span id="geld">0</span></p>
            <p>Tag: <span id="tage">1</span></p>
        </div>
        <div id="controls">
            <button id="upgrade-ambiente">Ambiente verbessern</button>
            <button id="upgrade-unterhaltung">Unterhaltung verbessern</button>
            <button id="upgrade-personal">Personal verbessern</button>
        </div>
        <div id="timer">
            <p>Verbleibende Zeit: <span id="timerAnzeigen">180</span> Sekunden</p>
            <input type="range" id="timerSlider" min="0" max="180" value="180" disabled>
        </div>
    </div>
    <script src="app.js"></script>
</body>
</html>

```

### 2. TypeScript

Ergänze die Datei `app.ts`, um den Countdown-Timer zu implementieren und den Slider entsprechend zu aktualisieren:

```typescript
class Nachtclub {
    ambiente: number;
    unterhaltung: number;
    personal: number;
    geld: number;
    tage: number;
    tagDauer: number;
    verbleibendeZeit: number;
    tagIntervall: NodeJS.Timeout | null;
    timerIntervall: NodeJS.Timeout | null;

    constructor() {
        this.ambiente = 0;
        this.unterhaltung = 0;
        this.personal = 0;
        this.geld = 0;
        this.tage = 1;
        this.tagDauer = 180; // 3 Minuten in Sekunden
        this.verbleibendeZeit = this.tagDauer;
        this.tagIntervall = null;
        this.timerIntervall = null;

        this.startTagesIntervall();
        this.startTimerIntervall();
    }

    upgradeAmbiente() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.ambiente += 1;
            this.updateUI();
        }
    }

    upgradeUnterhaltung() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.unterhaltung += 1;
            this.updateUI();
        }
    }

    upgradePersonal() {
        if (this.geld >= 10) { // Beispielkosten für Upgrade
            this.geld -= 10;
            this.personal += 1;
            this.updateUI();
        }
    }

    berechneEinnahmen() {
        return this.ambiente * 5 + this.unterhaltung * 10 + this.personal * 8; // Beispielwerte
    }

    naechsterTag() {
        this.tage += 1;
        this.geld += this.berechneEinnahmen();
        this.verbleibendeZeit = this.tagDauer;
        this.updateUI();
    }

    startTagesIntervall() {
        this.tagIntervall = setInterval(() => {
            this.naechsterTag();
        }, this.tagDauer * 1000); // Konvertiere Sekunden zu Millisekunden
    }

    startTimerIntervall() {
        this.timerIntervall = setInterval(() => {
            this.verbleibendeZeit -= 1;
            if (this.verbleibendeZeit < 0) {
                this.verbleibendeZeit = this.tagDauer;
            }
            this.updateTimer();
        }, 1000); // Jede Sekunde aktualisieren
    }

    updateTimer() {
        const timerElement = document.getElementById('timerAnzeigen') as HTMLSpanElement;
        const sliderElement = document.getElementById('timerSlider') as HTMLInputElement;
        timerElement.innerText = this.verbleibendeZeit.toString();
        sliderElement.value = this.verbleibendeZeit.toString();
    }

    updateUI() {
        (document.getElementById('ambiente') as HTMLSpanElement).innerText = this.ambiente.toString();
        (document.getElementById('unterhaltung') as HTMLSpanElement).innerText = this.unterhaltung.toString();
        (document.getElementById('personal') as HTMLSpanElement).innerText = this.personal.toString();
        (document.getElementById('geld') as HTMLSpanElement).innerText = this.geld.toString();
        (document.getElementById('tage') as HTMLSpanElement).innerText = this.tage.toString();
        this.updateTimer();
    }
}

const club = new Nachtclub();

document.getElementById('upgrade-ambiente')!.addEventListener('click', () => {
    club.upgradeAmbiente();
});

document.getElementById('upgrade-unterhaltung')!.addEventListener('click', () => {
    club.upgradeUnterhaltung();
});

document.getElementById('upgrade-personal')!.addEventListener('click', () => {
    club.upgradePersonal();
});

club.updateUI();

```

### Erklärung

-   **Timer-Elemente hinzufügen**: Ein Timer-Anzeige-Element (`<span>`) und ein Range-Element (`<input type="range">`) werden hinzugefügt, um die verbleibende Tageszeit anzuzeigen.
-   **Countdown-Timer**: Der `startTimerIntervall`-Funktion reduziert jede Sekunde die verbleibende Zeit und aktualisiert die Anzeige und den Slider.
-   **Tag-Intervall**: Der `startTagesIntervall`-Funktion setzt den Timer nach Ablauf der Zeit zurück und führt die Tageswechsel-Logik aus.
-   **UI-Aktualisierung**: `updateTimer` aktualisiert den Timer und den Slider. `updateUI` ruft `updateTimer` auf, um sicherzustellen, dass der Timer immer synchronisiert ist.

Mit dieser Struktur wird der verbleibende Tageszeitraum über einen Countdown-Timer visuell dargestellt, und der Slider zeigt die verbleibenden Sekunden an.

