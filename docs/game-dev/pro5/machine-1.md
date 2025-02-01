---
title: Machine 1
tags: 
    - Machines
    - JS
sidebar_position: 21
sidebar_label: Machine 1
---


# Vending Machine



## Utils

```javascript
function roundToNext5Cents(amount) {
  return Math.ceil(amount * 20) / 20;
}
```

### **Funktionsweise:**
1. **`amount * 20`:** Multipliziert den Betrag, sodass 5 Cent einem Ganzzahl-Schritt entspricht (5 Cent = 0.05, also 1/20).  
2. **`Math.ceil(...)`:** Rundet auf die nächste Ganzzahl auf.  
3. **`/ 20`:** Teilt den Betrag wieder zurück, um auf den ursprünglichen Wert zu skalieren.  

### **Beispiele:**
```javascript
console.log(roundToNext5Cents(1.02)); // 1.05
console.log(roundToNext5Cents(1.06)); // 1.10
console.log(roundToNext5Cents(1.00)); // 1.00
console.log(roundToNext5Cents(2.23)); // 2.25
```

**Hinweis:** Die Funktion arbeitet auch für negative Werte entsprechend:  
```javascript
console.log(roundToNext5Cents(-1.07)); // -1.05
```

## Geldeinwurf


### Machine

```javascript title="machine.js"
const coins = [0.05, 0.10, 0.20, 0.5, 1, 2]; // gültige Münzen
let insertCoins = []; // Array für eingeworfene Münzen

// Münze hinzufügen
function addCoin(coin) {
  if (coins.includes(coin)) { // Überprüfen, ob die Münze gültig ist
    insertCoins.push(coin);  // Münze hinzufügen
    console.log(`Münze hinzugefügt: ${coin}`);
  } else {
    console.log(`Ungültige Münze: ${coin}`);
  }
}

// Berechnet den Gesamtwert der eingeworfenen Münzen
function getTotal() {
  return insertCoins.reduce((sum, coin) => sum + coin, 0).toFixed(2);
}

// Preis prüfen und Rückgeld berechnen
function checkPrice(price) {
  const total = parseFloat(getTotal());

  if (total >= price) { // Genug Geld eingeworfen?
    let change = (total - price).toFixed(2); // Rückgeld berechnen
    console.log(`Preis bezahlt! Rückgeld: ${change}€`);
    insertCoins = []; // Eingeworfene Münzen zurücksetzen
    return getChangeArray(change); // Rückgeld in Münzen zurückgeben
  } else {
    console.log(`Nicht genug Geld. Fehlend: ${(price - total).toFixed(2)}€`);
    return [];
  }
}

// Rückgeld in Münzen umwandeln
function getChangeArray(change) {
  let changeCoins = [];
  let remaining = parseFloat(change);

  coins.sort((a, b) => b - a); // Absteigend sortieren

  for (let coin of coins) {
    while (remaining >= coin) {
      changeCoins.push(coin); // Münze hinzufügen
      remaining = (remaining - coin).toFixed(2); // Rest aktualisieren
    }
  }
  return changeCoins;
}


```

### Usage


```javascript title="Test"
addCoin(1);
addCoin(0.5);
console.log('Gesamtwert:', getTotal()); // 1.50€
console.log('Rückgeld:', checkPrice(1.20)); // 0.30€ Rückgeld -> [0.2, 0.1]
console.log('Rückgeld:', checkPrice(2)); // Fehlend: 0.50€
```

**Ausgabe:**

```
Münze hinzugefügt: 1
Münze hinzugefügt: 0.5
Gesamtwert: 1.50
Preis bezahlt! Rückgeld: 0.30€
Rückgeld: [0.2, 0.1]
Nicht genug Geld. Fehlend: 0.50€
``` 


**Info:**


1. **`getTotal()`** – Summiert die eingeworfenen Münzen und gibt den Betrag zurück.  
2. **`checkPrice(price)`** – Vergleicht den Gesamtwert der eingeworfenen Münzen mit dem Preis:  
   - Wenn genügend Geld eingeworfen wurde, wird Rückgeld berechnet und als Münzarray zurückgegeben.  
   - Falls zu wenig eingeworfen wurde, wird eine Fehlermeldung ausgegeben.  
3. **`getChangeArray(change)`** – Zerlegt das Rückgeld in die verfügbaren Münzen aus dem `coins`-Array.  

---