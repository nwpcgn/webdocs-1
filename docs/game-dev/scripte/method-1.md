---
title: Methods 1
sidebar_label: Methods 1
sidebar_position: 91
---



Um zu überprüfen, ob ein Array auch Arrays als Elemente enthält, kannst du `Array.isArray()` verwenden. Hier ist ein erweiterter Ansatz, der die verschiedenen Typen im Array berücksichtigt:

### Erweiterte Methode
```javascript
const arr = [[1, 2], "hello", { key: 3 }];

const containsArrays = arr.some(item => Array.isArray(item));
const containsOnlyArrays = arr.every(item => Array.isArray(item));

console.log(containsArrays);    // true (mindestens ein Array enthalten)
console.log(containsOnlyArrays); // false (nicht nur Arrays enthalten)
```

### Erklärung
1. **`Array.isArray(item)`**: Prüft, ob ein Element des Arrays selbst ein Array ist.
2. **`some()`**: Gibt `true` zurück, wenn **mindestens ein Element** des Arrays ein Array ist.
3. **`every()`**: Gibt `true` zurück, wenn **alle Elemente** des Arrays Arrays sind.

### Zusammenfassung aller Checks
Hier ist eine Funktion, die überprüft, ob ein Array Objekte, Strings oder Arrays enthält:

```javascript
function analyzeArray(arr) {
  return {
    hasObjects: arr.some(item => typeof item === 'object' && item !== null && !Array.isArray(item)),
    hasStrings: arr.some(item => typeof item === 'string'),
    hasArrays: arr.some(item => Array.isArray(item)),
    containsOnlyArrays: arr.every(item => Array.isArray(item)),
  };
}

// Beispiel
const testArray = [["nested"], "hello", { key: 3 }, [4, 5]];
console.log(analyzeArray(testArray));
```

### Ausgabe für das Beispiel
```javascript
{
  hasObjects: true,
  hasStrings: true,
  hasArrays: true,
  containsOnlyArrays: false
}
```

Diese Funktion hilft dir, schnell zu erkennen, welche Typen im Array vorhanden sind.

---


Wenn du nach einer Prüfung entscheiden möchtest, ob eine Variable ein Array zugewiesen bekommt oder nicht, kannst du das so gestalten:

### Beispiel
```javascript
const input = ["hello", { key: 2 }, [1, 2]]; // Beispiel-Array

// Überprüfungen
const hasOnlyObjects = input.every(item => typeof item === 'object' && item !== null && !Array.isArray(item));
const hasOnlyStrings = input.every(item => typeof item === 'string');
const hasArrays = input.some(item => Array.isArray(item));

// Bedingte Zuweisung
let result;
if (hasOnlyObjects) {
  result = input; // Zuweisung, wenn alle Elemente Objekte sind
} else if (hasOnlyStrings) {
  result = input; // Zuweisung, wenn alle Elemente Strings sind
} else if (hasArrays) {
  result = input; // Zuweisung, wenn mindestens ein Element ein Array ist
} else {
  result = null; // Kein passendes Kriterium
}

console.log(result);
```

### Erklärung
1. **`input`**: Das ursprüngliche Array, das überprüft wird.
2. **Prüfungen**:
   - `hasOnlyObjects`: Prüft, ob alle Elemente Objekte sind.
   - `hasOnlyStrings`: Prüft, ob alle Elemente Strings sind.
   - `hasArrays`: Prüft, ob mindestens ein Element ein Array ist.
3. **Zuweisung**:
   - `result` wird basierend auf der Prüfung mit `input` oder einem anderen Wert zugewiesen.

### Flexibler Ansatz mit einer Funktion
Wenn du dieselbe Logik wiederverwenden möchtest, kannst du eine Funktion schreiben:

```javascript
function assignArray(input) {
  if (input.every(item => typeof item === 'object' && item !== null && !Array.isArray(item))) {
    return input; // Alle Objekte
  } else if (input.every(item => typeof item === 'string')) {
    return input; // Alle Strings
  } else if (input.some(item => Array.isArray(item))) {
    return input; // Mindestens ein Array
  }
  return null; // Keine Übereinstimmung
}

// Beispiel
const result = assignArray(input);
console.log(result);
```

### Ergebnis
- Gibt `input` zurück, wenn die Bedingungen erfüllt sind.
- Gibt `null` zurück, wenn keine der Bedingungen erfüllt wird.

---

Wenn du überprüfen möchtest, ob ein Array von Objekten alle Elemente mit den Schlüsseln `width` und `height` enthält, kannst du die `every()`-Methode nutzen. Hier ist eine Beispielimplementierung:

### Codebeispiel
```javascript
const array = [
  { width: 100, height: 200 },
  { width: 50, height: 75 },
  { width: 300 } // Dieses Element fehlt `height`
];

// Überprüfen, ob alle Objekte die Keys `width` und `height` haben
const allHaveWidthAndHeight = array.every(
  obj => obj.hasOwnProperty('width') && obj.hasOwnProperty('height')
);

console.log(allHaveWidthAndHeight); // false
```

### Erklärung
1. **`every()`**:
   - Prüft, ob **alle** Elemente im Array die Bedingung erfüllen.
2. **`hasOwnProperty(key)`**:
   - Überprüft, ob das Objekt den Schlüssel `key` besitzt.
3. **Rückgabe**:
   - Gibt `true` zurück, wenn alle Objekte `width` und `height` enthalten.
   - Gibt `false` zurück, wenn mindestens ein Objekt einen der Schlüssel nicht besitzt.

---

### Alternative: Funktion für Wiederverwendbarkeit
Du kannst eine Funktion erstellen, die flexibel überprüft, ob Objekte bestimmte Schlüssel enthalten:

```javascript
function allObjectsHaveKeys(array, keys) {
  return array.every(obj => keys.every(key => obj.hasOwnProperty(key)));
}

// Beispiel
const arrayToCheck = [
  { width: 100, height: 200 },
  { width: 50, height: 75 },
  { width: 300 }
];

const result = allObjectsHaveKeys(arrayToCheck, ['width', 'height']);
console.log(result); // false
```

### Erklärung
1. **Verschachtelte `every()`-Methode**:
   - `array.every()` prüft jedes Objekt.
   - `keys.every()` prüft, ob jedes Objekt alle erforderlichen Schlüssel enthält.
2. **Flexible Schlüssel-Liste**:
   - Übergib beliebige Schlüssel als Array an die Funktion (z. B. `['width', 'height']`).

### Bonus: Ungültige Elemente herausfiltern
Wenn du die Objekte, die die Schlüssel nicht enthalten, herausfiltern möchtest:
```javascript
const invalidObjects = array.filter(
  obj => !('width' in obj && 'height' in obj)
);

console.log(invalidObjects);
// [{ width: 300 }]
```

So kannst du nach Bedarf die fehlenden Objekte weiterverarbeiten.
