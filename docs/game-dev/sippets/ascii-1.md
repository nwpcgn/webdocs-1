---
title: Ascii I
sidebar_position: 9
sidebar_label: Ascii I
---


# Ascii Render Canvas


-   ASCII-Code aus **einem Array** (deiner JSON) rendert
    
-   **Zeile für Zeile** zeichnet (erst wenn eine fertig ist, beginnt die nächste)
    
-   **Zeichen für Zeichen** als Typewriter-Effekt schreibt
    
-   Die **Geschwindigkeit anpassbar** macht
    
-   Automatisch ein Canvas oder ein `<pre>` beschreiben kann (hier: Canvas)
    

**KEINE Abhängigkeiten – pure Vanilla JS.**

----------

# ✅ **Funktion: `renderAsciiTypewriter()`**

✔ nimmt ein Canvas, ein Array von ASCII-Strings, eine Geschwindigkeit und eine Farbe  
✔ zeichnet Pixel-genau wie ein Terminal

----------

## **Kompletter Code**

```html
<canvas id="ascii" width="400" height="400"></canvas>

<script>
// ========================================================
// Typewriter ASCII Renderer (Canvas)
// ========================================================
async function renderAsciiTypewriter({
  canvas,
  ascii,
  speed = 20,     // ms pro Zeichen
  font = "16px monospace",
  color = "#0f0",
  lineHeight = 18
}) {
  const ctx = canvas.getContext("2d");
  ctx.font = font;
  ctx.fillStyle = color;

  let y = lineHeight;

  for (let row of ascii) {
    let current = "";

    for (let char of row) {
      current += char;

      ctx.clearRect(0, y - lineHeight, canvas.width, lineHeight);
      ctx.fillText(current, 10, y);

      await new Promise(r => setTimeout(r, speed));
    }

    y += lineHeight; // nächste Zeile
  }
}


// ========================================================
// Beispiel-Aufruf
// ========================================================
const asciiData = [
  "....................",
  ".+----------------+.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".+----------------+.",
  "...................."
];

const canvas = document.getElementById("ascii");

renderAsciiTypewriter({
  canvas,
  ascii: asciiData,
  speed: 15,       // schneller / langsamer
  font: "16px monospace",
  color: "#4F4",
  lineHeight: 18
});
</script>

```

----------

# ✨ **Wie es aussieht**

Die Ausgabe zeichnet Zeile für Zeile, jede Zeile entsteht wie auf einer Schreibmaschine:

```
....................
.+----------------+.
.|................|.
.|................|.
...

```

Jede Zeile wird erst nach vollständigem Zeichnen der vorherigen geschrieben.

----------

# ⚙️ Anpassbare Parameter

Parameter

Beschreibung

`canvas`

HTMLCanvasElement

`ascii`

Array von Strings (deine JSON)

`speed`

Geschwindigkeit in Millisekunden pro Zeichen

`font`

Canvas-Schriftart

`color`

Textfarbe

`lineHeight`

Abstand zwischen Zeilen

----------

Alles klar — hier ist die sauberste, einfachste und zuverlässig funktionierende Variante:

# ✅ ASCII-Typewriter in ein `<div>` rendern

Mit korrektem Styling für Monospace, fixe Breite & Zeilenumbrüche.

----------

# 1️⃣ **DIV richtig stylen**

Damit ASCII _exakt so aussieht wie in deiner JSON_ (keine Schriftmodifikationen, keine Verdichtung, feste Breite, Zeilenumbrüche sichtbar):

```html
<div id="ascii-box"></div>

<style>
  #ascii-box {
    font-family: monospace;    /* WICHTIG: echte monospaced Ausgabe */
    white-space: pre;          /* Zeilenumbrüche UND Leerzeichen übernehmen */
    background: #111;
    color: #4F4;
    padding: 10px;
    border: 1px solid #333;
    display: inline-block;     /* passt sich dem Inhalt an */
  }
</style>

```

**white-space: pre** ist der wichtigste Teil.  
Damit ignoriert das DIV _NICHT_ mehrere Punkte, Leerzeichen etc.

----------

# 2️⃣ **Typewriter-Renderfunktion für ein DIV**

Hier kommt eine reine **Vanilla-JS** Funktion, die:

-   Zeile für Zeile schreibt
    
-   Zeichen für Zeichen animiert
    
-   Geschwindigkeit einstellbar macht
    
-   keinerlei Canvas benötigt
    

```html
<script>
async function renderAsciiToDiv({
  element,
  ascii,
  speed = 20
}) {
  element.textContent = ""; // initial leeren

  for (let row of ascii) {
    let current = "";

    for (let char of row) {
      current += char;

      // Letzte Zeile + aktuelles Teilstück rendern
      element.textContent += "\r" + current;

      await new Promise(r => setTimeout(r, speed));
    }

    // Zeile abschließen und Zeilenumbruch hinzufügen
    element.textContent += "\n";
  }
}
</script>

```

----------

# 3️⃣ **Beispiel-Aufruf**

```html
<script>
const asciiData = [
  "....................",
  ".+----------------+.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".+----------------+.",
  "...................."
];

renderAsciiToDiv({
  element: document.getElementById("ascii-box"),
  ascii: asciiData,
  speed: 15
});
</script>

```

----------

# 🎉 Ergebnis

✔ ASCII erscheint **genau wie in deiner Datei**  
✔ Leerzeichen, Punkte, Linien → unverändert  
✔ Typewriter schreibt **Zeichen für Zeichen**  
✔ DIV erzeugt ein echtes Text-Terminal-Gefühl

----------


Alles klar — hier bekommst du eine **voll funktionsfähige Terminal-Konsole** mit:

✔ **Typewriter-Write-API (`terminal.write("...")`)**  
✔ **Blinkendem Cursor (`_`)**  
✔ **Automatischem Scrollen**  
✔ **ASCII-Perfekter Darstellung**  
✔ **Gut dokumentiertem Vanilla-JS Code**

Das Ganze ist minimal, flexibel und leicht erweiterbar.

----------

# ✅ **KOMPLETTE TERMINAL-KOMPONENTE (Copy & Paste)**

```html
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8" />
<title>ASCII Terminal</title>

<style>
  /* ===========================
     TERMINAL STYLE
     =========================== */
  #terminal {
    font-family: monospace;   /* ASCII benötigt echte Fixed-Width Schrift */
    white-space: pre;         /* Zeilenumbrüche, Punkte, Leerzeichen beibehalten */
    background: #111;
    color: #4F4;
    padding: 12px;
    border: 1px solid #333;
    width: 500px;
    height: 300px;
    overflow-y: auto;         /* Konsole scrollbar machen */
    display: block;
  }

  /* Cursor-Blitzern animieren */
  .cursor {
    display: inline-block;
    width: 10px;
    animation: blink 0.6s steps(1) infinite;
  }
  @keyframes blink {
    0% { opacity: 1; }
    50%{ opacity: 0; }
    100%{ opacity: 1; }
  }
</style>

</head>
<body>

<div id="terminal"></div>

<script>
// ========================================================================
// TERMINAL CLASS MIT WRITE-API
// ========================================================================
class Terminal {
  constructor(element, speed = 20) {
    this.el = element;
    this.speed = speed;

    this.cursor = document.createElement("span");
    this.cursor.className = "cursor";
    this.cursor.textContent = "_";

    // Terminal-Inhalt
    this.buffer = "";  
    this.el.textContent = "";
    this.el.appendChild(this.cursor);
  }

  // --------------------------------------------------------
  // PRIVATE HILFSFUNKTION: Terminal neuschreiben + Cursor
  // --------------------------------------------------------
  _render() {
    this.el.textContent = this.buffer;
    this.el.appendChild(this.cursor);

    // automatisch nach unten scrollen
    this.el.scrollTop = this.el.scrollHeight;
  }

  // --------------------------------------------------------
  // WRITE: Gibt einen Text mit Typewriter-Effekt aus
  // --------------------------------------------------------
  async write(text) {
    for (let char of text) {
      this.buffer += char;
      this._render();
      await new Promise(res => setTimeout(res, this.speed));
    }
  }

  // --------------------------------------------------------
  // WRITELN: wie write, aber beendet mit einer neuen Zeile
  // --------------------------------------------------------
  async writeln(text = "") {
    await this.write(text);
    this.buffer += "\n";
    this._render();
  }

  // --------------------------------------------------------
  // WRITE ASCII ARRAY (z.B. dein JSON ASCII-Bild)
  // zeichnet Zeile für Zeile, Zeichen für Zeichen
  // --------------------------------------------------------
  async writeAscii(asciiArray) {
    for (let row of asciiArray) {
      await this.writeln(row);
    }
  }
}



// ========================================================================
// TERMINAL INITIALISIEREN
// ========================================================================
const term = new Terminal(document.getElementById("terminal"), 15);


// ========================================================================
// DEMO: ASCII-Daten schreiben (dein Beispiel)
// ========================================================================
const asciiData = [
  "....................",
  ".+----------------+.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".|................|.",
  ".+----------------+.",
  "...................."
];

(async () => {
  await term.writeln("STARTE ASCII-RENDER...");
  await term.writeAscii(asciiData);
  await term.writeln();
  await term.writeln("FERTIG!");
})();
</script>

</body>
</html>

```

----------

# 💡 **Was du jetzt hast**

## ✔ Terminal-Komponente

-   `write(text)` → Tippt Text wie eine Maschine
    
-   `writeln(text)` → Wie write, nur mit `\n` am Ende
    
-   `writeAscii(array)` → ASCII-Grafiken Zeile für Zeile animieren
    

## ✔ Blinkender Cursor

Ein animiertes `_` das immer am Ende der Zeile blinkt.

## ✔ Scrollbar

Immer wenn der Inhalt wächst → Konsole scrollt automatisch nach unten.

## ✔ ASCII-Safe Darstellung

`white-space: pre; font-family: monospace;` sorgt dafür, dass Punkte, Linien usw. **perfekt ausgerichtet** erscheinen.

----------


