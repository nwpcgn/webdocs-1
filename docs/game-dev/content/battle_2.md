---
title: BattleGame 2
description: Battle HTML5 Game 2.1
sidebar_position: 22
sidebar_label: Js-Battle
---

# Battle Game in HTML5 


## 1

Um ein einfaches Battle Game in HTML5 zu erstellen, benötigst du HTML, CSS und JavaScript. Hier ist ein grundlegendes Beispiel für den Ablauf, den du beschrieben hast:

1.  **index.html (Landing Page):**

```html title="index.html"
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Battle Game</title>
</head>
<body>
    <div id="loading-screen">
        <h1>Loading...</h1>
    </div>

    <div id="logo-animation" class="hidden">
        <!-- Logo Animation goes here -->
    </div>

    <div id="landing-page" class="hidden">
        <h1>Welcome to the Battle Game</h1>
        <button onclick="startBattle()">Start Battle</button>
    </div>

    <script src="script.js"></script>
</body>
</html>

```

2.  **styles.css:**

```css
body {
    margin: 0;
    font-family: 'Arial', sans-serif;
}

.hidden {
    display: none;
}

#loading-screen, #logo-animation, #landing-page {
    text-align: center;
    margin-top: 50vh;
    transform: translateY(-50%);
}

button {
    padding: 10px;
    font-size: 16px;
    cursor: pointer;
}

```

3.  **script.js:**

```javascript
document.addEventListener("DOMContentLoaded", function() {
    // Simulate loading screen
    setTimeout(function() {
        document.getElementById("loading-screen").classList.add("hidden");
        document.getElementById("logo-animation").classList.remove("hidden");
        
        // Simulate logo animation (add your own animation logic)

        // Show landing page after animation
        setTimeout(function() {
            document.getElementById("logo-animation").classList.add("hidden");
            document.getElementById("landing-page").classList.remove("hidden");
        }, 3000); // Adjust the time according to your animation
    }, 2000); // Adjust the loading time

});

function startBattle() {
    // Navigate to the Battle page
    window.location.href = "battle.html";
}

```



```html title="battle.html (Battle Page)"
<!-- Similar structure to landing page, with fighter selection -->

```


```html title="arena.html (Arena Page)"
<!-- Arena page for battle mechanics -->

```



```html title="result.html (Result Page)"
<!-- Result page with end battle options -->
```

Dieses Beispiel ist sehr einfach gehalten und zeigt nur den Grundgerüst deines Spiels. Du kannst die Seiten mit mehr Inhalten, Styling und Funktionalitäten füllen, insbesondere die "battle.html" und "arena.html", um die Kämpfe und die Battle-Mechanik zu implementieren. Beachte, dass das Beispiel auch simulierten Code für das Laden und die Animation enthält, die du durch deine eigenen Implementierungen ersetzen solltest.


## 2


Um die Seiten dynamisch über JavaScript zu laden, kannst du die Funktionen `loadPage` und `startBattle` im JavaScript-Code anpassen. Hier ist eine überarbeitete Version des `script.js`-Codes:

```javascript
document.addEventListener("DOMContentLoaded", function() {
    // Simulate loading screen
    setTimeout(function() {
        document.getElementById("loading-screen").classList.add("hidden");
        document.getElementById("logo-animation").classList.remove("hidden");
        
        // Simulate logo animation (add your own animation logic)

        // Show landing page after animation
        setTimeout(function() {
            document.getElementById("logo-animation").classList.add("hidden");
            document.getElementById("landing-page").classList.remove("hidden");
        }, 3000); // Adjust the time according to your animation
    }, 2000); // Adjust the loading time

});

function loadPage(pageId) {
    // Hide all pages
    var pages = document.querySelectorAll('.page');
    pages.forEach(function(page) {
        page.classList.add('hidden');
    });

    // Show the selected page
    var selectedPage = document.getElementById(pageId);
    selectedPage.classList.remove('hidden');
}

function startBattle() {
    // Dynamically load the Battle page
    var battlePage = document.getElementById("battle-page");
    if (!battlePage) {
        // Create the Battle page if it doesn't exist
        battlePage = document.createElement("div");
        battlePage.id = "battle-page";
        battlePage.classList.add("page");
        battlePage.innerHTML = "<h1>Battle Page</h1><button onclick='selectFighter()'>Select Fighter</button>";
        document.body.appendChild(battlePage);
    }

    // Load the Battle page
    loadPage("battle-page");
}

function selectFighter() {
    // Dynamically load the Fighter selection page
    var fighterPage = document.getElementById("fighter-page");
    if (!fighterPage) {
        // Create the Fighter selection page if it doesn't exist
        fighterPage = document.createElement("div");
        fighterPage.id = "fighter-page";
        fighterPage.classList.add("page");
        fighterPage.innerHTML = "<h1>Select Fighter</h1><button onclick='findOpponent()'>Find Opponent</button>";
        document.body.appendChild(fighterPage);
    }

    // Load the Fighter selection page
    loadPage("fighter-page");
}

function findOpponent() {
    // Dynamically load the Opponent selection page
    var opponentPage = document.getElementById("opponent-page");
    if (!opponentPage) {
        // Create the Opponent selection page if it doesn't exist
        opponentPage = document.createElement("div");
        opponentPage.id = "opponent-page";
        opponentPage.classList.add("page");
        opponentPage.innerHTML = "<h1>Select Opponent</h1><button onclick='startArena()'>Start Arena</button>";
        document.body.appendChild(opponentPage);
    }

    // Load the Opponent selection page
    loadPage("opponent-page");
}

function startArena() {
    // Dynamically load the Arena page
    var arenaPage = document.getElementById("arena-page");
    if (!arenaPage) {
        // Create the Arena page if it doesn't exist
        arenaPage = document.createElement("div");
        arenaPage.id = "arena-page";
        arenaPage.classList.add("page");
        arenaPage.innerHTML = "<h1>Arena</h1><button onclick='showResult()'>Show Result</button>";
        document.body.appendChild(arenaPage);
    }

    // Load the Arena page
    loadPage("arena-page");
}

function showResult() {
    // Dynamically load the Result page
    var resultPage = document.getElementById("result-page");
    if (!resultPage) {
        // Create the Result page if it doesn't exist
        resultPage = document.createElement("div");
        resultPage.id = "result-page";
        resultPage.classList.add("page");
        resultPage.innerHTML = "<h1>Result Page</h1><button onclick='backToStart()'>Back to Start</button><button onclick='restartBattle()'>Restart Battle</button>";
        document.body.appendChild(resultPage);
    }

    // Load the Result page
    loadPage("result-page");
}

function backToStart() {
    // Go back to the Landing page
    loadPage("landing-page");
}

function restartBattle() {
    // Go back to the Battle page
    loadPage("battle-page");
}
```

In diesem Code werden die Seiten dynamisch erstellt und geladen, wenn sie noch nicht existieren. Die Funktion `loadPage` zeigt die ausgewählte Seite an, während die anderen Funktionen für die Navigation zwischen den Seiten verwendet werden. Beachte, dass die Seiten durch `div`-Elemente mit IDs repräsentiert werden, und der Klasse "page" für allgemeine Styling-Anpassungen zugeordnet sind.

