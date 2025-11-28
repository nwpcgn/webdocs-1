---
title: Box Shadow Generator
tags: 
    - Machines
    - JS
sidebar_position: 31
sidebar_label: Pixel Generator
---


# Pixel Art Generator





![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white) ![JavaScript](https://img.shields.io/badge/javascript-%23323330.svg?style=for-the-badge&logo=javascript&logoColor=%23F7DF1E) ![CSS3](https://img.shields.io/badge/css3-%231572B6.svg?style=for-the-badge&logo=css3&logoColor=white)



## Einführung

Dieses Projekt ist eine Webanwendung, die moderne UI-Frameworks wie DaisyUI und TailwindCSS verwendet, um eine benutzerfreundliche Oberfläche bereitzustellen. Es handelt sich um eine modular aufgebaute Anwendung mit HTML, CSS und JavaScript, die sich auf responsives Design und dynamische Inhalte konzentriert.

## Inhaltsverzeichnis

- [Einführung](#einführung)
- [Features](#features)
- [Nutzung](#nutzung)
- [Abhängigkeiten](#abhängigkeiten)
- [Code](#code)


## Features

- **Responsive Design**: Optimiert für verschiedene Bildschirmgrößen.
- **Integration von DaisyUI**: Nutzung von vorgefertigten UI-Komponenten.
- **Dynamische Animationen**: Eingebettet in CSS und JavaScript.
- **Grid-System**: Flexibles Rasterlayout mit anpassbaren Spalten und Reihen.
- **Zentrale Farb- und Größenvariablen**: Einfach anpassbares Design.




## Nutzung

1. Öffnen Sie die Datei `index.html` in Ihrem bevorzugten Browser.
2. Die Anwendung zeigt eine Navbar, dynamische Animationen und Rasterlayouts an.
3. Anpassungen können in den Dateien `styles.css` und `app1.js` vorgenommen werden.

## Abhängigkeiten

Die Anwendung nutzt folgende Bibliotheken und Frameworks:
- [DaisyUI](https://daisyui.com/)
- [TailwindCSS](https://tailwindcss.com/)
- [Reef.js](https://reefjs.com/)




## Code

```javascript title="pa-generator.js"
let {
    signal,
    component
} = reef;

let daten = signal({
    heading: "My Todos",
    items: [],
    amount: 0,
    rows: 10,
    cols: 10,
    size: 40,
    output: "",
});

function createArray(length = 1, value = 0) {
    const arr = new Array(length).fill(value);
    return arr;
}

function gridClick(e) {
    const el = e.currentTarget;
    const id = parseInt(el.dataset.id);
    if (daten.items[id] == 1) {
        daten.items[id] = 0;
    } else {
        daten.items[id] = 1;
    }
    daten.amount = daten.items.reduce((acc, val) => acc + val, 0);
    renderTilemap();
}

const t1 = (data) => `
<section class="layer center nwp"> 
    <div>
    <div>Total: ${data.amount}</div>
    <div class="my-grid" style="--size: ${data.size}px; --row: ${
  data.rows
}; --col: ${data.cols};">
    ${data.items
      .map(
        (v, i) =>
          `<button style="--bgc: ${
            v == 1 ? "var(--sky-500)" : "#fff"
          };" data-id="${i}" data-val="${v}" onclick="gridClick()"><span class="sr-only">${v}</span></button>`
      )
      .join("")}
    </div>
    <pre>${data.output}</pre>
    </div>     
</section>
`;

function renderTilemap() {
    const tileset = daten.items;
    const cols = daten.cols;
    const rows = daten.rows;
    const unit = daten.size;
    let shadow = [];
    let min_width = cols * unit;
    let min_height = rows * unit;
    let max_width = 0;
    let max_height = 0;
    let baxShadow = "";
    for (let y = 0; y < rows; y++) {
        for (let x = 0; x < cols; x++) {
            const tileIndex = tileset[y * cols + x];
            if (tileIndex !== 0) {
                if (x + unit < min_width) {
                    min_width = x + unit;
                }
                if (y + unit < min_height) {
                    min_height = y + unit;
                }
                if (x + unit * 2 > max_width) {
                    max_width = x + unit * 2;
                }
                if (y + unit * 2 > max_height) {
                    max_height = y + unit * 2;
                }
                shadow.push(x + unit + "px " + (y + unit) + "px #333333");
            }
        }
    }

    if (shadow.length) {
        baxShadow = shadow.join(",\n\t\t");
    }

    const styleTemp = `
    .pixels {
        display: block;
        width: ${unit}px;
        height: ${unit}px;
        margin: -${min_height}px ${max_width}px ${max_height}px -${min_width}px;
        box-shadow: ${baxShadow};
    }
`;
    daten.output = styleTemp;
}

function start() {
    component("#app", () => t1(daten), {
        events: {
            gridClick
        }
    });
}

const run = () => {
    console.log("app run");
    const length = daten.rows * daten.cols;
    const a = createArray(length, 0);
    console.log({
        length,
        a
    });
    daten.items = a;
    start();
};

window.addEventListener("DOMContentLoaded", () => {
    console.log("DOMContentLoaded");
    run();
});
``` 


