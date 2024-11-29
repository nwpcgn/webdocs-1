---
sidebar_position: 4
sidebar_label: Hash Routing
---



# Hash Routing

#### 1. **Import der Svelte Stores**

```javascript
import { derived, writable } from 'svelte/store';

```

-   `writable`: Ein reaktiver Zustand, der direkt verändert werden kann.
-   `derived`: Ein abgeleiteter Zustand, der basierend auf anderen Stores berechnet wird.

#### 2. **Definition der `createUrlStore` Funktion**

```javascript
export function createUrlStore(ssrUrl) {

```

-   Diese Funktion erstellt einen Store, der die aktuelle URL der Seite speichert.
-   `ssrUrl`: Wird genutzt, um beim Server-Side Rendering (SSR) eine URL vorzugeben, da es im SSR-Modus kein `window`-Objekt gibt.

#### 3. **Fallunterscheidung: SSR oder Client-seitig**

```javascript
if (typeof window === 'undefined') {
	const { subscribe } = writable(ssrUrl);
	return { subscribe };
}

```

-   Falls die Funktion im Server-Side Rendering ausgeführt wird (`typeof window === 'undefined'`):
    -   Es wird ein `writable` Store mit der übergebenen `ssrUrl` erstellt.
    -   Nur die `subscribe`-Methode des Stores wird zurückgegeben, da im SSR-Modus keine URL-Änderungen möglich sind.

#### 4. **Client-seitiger Modus**

Wenn `window` verfügbar ist (also im Browser), wird der Store für die URL erstellt:

##### a) **Erstellung eines `writable` Stores für die aktuelle URL**

```javascript
const href = writable(window.location.href);

```

-   `href`: Speichert die aktuelle URL (`window.location.href`) als Zustand.

##### b) **Überschreiben der History-Methoden**

```javascript
const originalPushState = history.pushState;
const originalReplaceState = history.replaceState;

history.pushState = function () {
	originalPushState.apply(this, arguments);
	updateHref();
};

history.replaceState = function () {
	originalReplaceState.apply(this, arguments);
	updateHref();
};

```

-   `pushState` und `replaceState` sind Methoden der `history`-API, um die URL zu ändern, ohne die Seite neu zu laden.
-   Der Code überschreibt diese Methoden, sodass nach jeder URL-Änderung `updateHref()` aufgerufen wird.

##### c) **Definieren von `updateHref`**

```javascript
const updateHref = () => href.set(window.location.href);

```

-   Diese Funktion aktualisiert den `href`-Store mit der aktuellen URL.

##### d) **Ereignislistener für Navigation**

```javascript
window.addEventListener('popstate', updateHref);
window.addEventListener('hashchange', updateHref);

```

-   `popstate`: Wird ausgelöst, wenn der Benutzer über den Zurück- oder Vorwärts-Button im Browser navigiert.
-   `hashchange`: Wird ausgelöst, wenn sich der Hash in der URL ändert (z. B. von `#section1` zu `#section2`).

#### 5. **Rückgabe eines abgeleiteten Stores**

```javascript
return {
	subscribe: derived(href, ($href) => new URL($href)).subscribe
};

```

-   Es wird ein abgeleiteter Store zurückgegeben, der die `href`-URL (`window.location.href`) in ein `URL`-Objekt umwandelt:
    -   `new URL($href)`: Macht die URL besser nutzbar, z. B. für Abfragen wie `url.pathname`, `url.searchParams`, etc.

#### 6. **Standard-Export**

```javascript
export default createUrlStore();

```

-   Hier wird die Funktion ausgeführt und das Ergebnis (der URL-Store) direkt als Standard-Export bereitgestellt.

----------

### **Wie wird das verwendet?**

1.  **Importieren des Stores:**
    
    ```javascript
    import urlStore from './path/to/store';
    
    ```
    
2.  **Abonnieren des Stores in einer Komponente:**
    
    ```svelte
    <script>
      import urlStore from './path/to/store';
      let currentUrl;
      const unsubscribe = urlStore.subscribe((url) => {
        currentUrl = url;
      });
    </script>
    
    <p>Aktuelle URL: {currentUrl.href}</p>
    
    ```
    
    -   `urlStore.subscribe` liefert das aktuelle `URL`-Objekt.
    -   Du kannst z. B. `currentUrl.pathname` oder `currentUrl.searchParams` verwenden.

----------

### **Vorteile:**

-   Zentralisiert die Verwaltung der aktuellen URL.
-   Reaktiv: Änderungen an der URL werden automatisch in Svelte-Komponenten reflektiert.
-   Nützlich für SPAs, die auf Änderungen in der URL reagieren müssen, z. B. für Routing oder Tracking.



---

### **Svelte 5-Konformität**

1.  **Kompatibilität von Stores:**
    
    -   `writable` und `derived` sind auch in Svelte 5 vorhanden und funktionieren genauso wie in Svelte 3/4.
    -   Die Nutzung von `subscribe` bleibt unverändert.
2.  **Unterschiede in Reactivity:** In Svelte 5 kannst du Stores direkt mit dem `@`-Operator verwenden, wodurch die manuelle Nutzung von `subscribe` in den meisten Fällen entfällt. Der Store muss dafür jedoch die `subscribe`-Methode implementieren (was hier der Fall ist).
    
    Beispiel mit dem Store:
    
    ```svelte
    <script>
      import urlStore from './path/to/store';
    </script>
    
    <p>Aktuelle URL: {@urlStore.href}</p>
    
    ```
    
    Das funktioniert mit dem bestehenden Store, da er korrekt als Svelte-Store implementiert ist (er besitzt eine `subscribe`-Methode).
    

----------

### **Optimierungen für Svelte 5**

Obwohl der Store funktioniert, könnten wir ihn anpassen, um von neuen Features in Svelte 5 zu profitieren:

#### **Optimierung: Automatische Dereferenzierung**

In Svelte 5 kannst du direkt auf die Inhalte eines Stores zugreifen, indem du `@` verwendest. Damit das reibungslos funktioniert, sollte die Rückgabe des Stores sauber und präzise gestaltet sein.

Anpassung:

```javascript
return derived(href, ($href) => new URL($href));

```

-   Statt ein Objekt mit `{ subscribe }` zurückzugeben, wird direkt der abgeleitete Store zurückgegeben.

Das hat zwei Vorteile:

1.  Der Store wird einfacher und klarer.
2.  Er ist direkt kompatibel mit der neuen Svelte-5-Syntax.

----------

### **Vollständiger, angepasster Store**

Hier ist der aktualisierte Code, optimiert für Svelte 5:

```javascript
import { derived, writable } from 'svelte/store';

export function createUrlStore(ssrUrl) {
	if (typeof window === 'undefined') {
		return writable(new URL(ssrUrl));
	}

	const href = writable(window.location.href);

	const originalPushState = history.pushState;
	const originalReplaceState = history.replaceState;

	const updateHref = () => href.set(window.location.href);

	history.pushState = function () {
		originalPushState.apply(this, arguments);
		updateHref();
	};

	history.replaceState = function () {
		originalReplaceState.apply(this, arguments);
		updateHref();
	};

	window.addEventListener('popstate', updateHref);
	window.addEventListener('hashchange', updateHref);

	return derived(href, ($href) => new URL($href));
}

export default createUrlStore();

```

----------

### **Verwendung in Svelte 5**

#### Direkter Zugriff mit `@`:

```svelte
<script>
  import urlStore from './path/to/store';
</script>

<p>Aktuelle URL: {@urlStore.href}</p>
<p>Pfad: {@urlStore.pathname}</p>

```

#### Nutzung in reaktiven Statements:

```svelte
<script>
  import urlStore from './path/to/store';
</script>

{#if @urlStore.pathname === '/home'}
  <p>Willkommen auf der Startseite!</p>
{/if}

```

----------

### **Zusammenfassung:**

-   Der ursprüngliche Store ist **mit Svelte 5 kompatibel**, da er die grundlegenden Store-Schnittstellen (`subscribe`) implementiert.
-   Durch eine kleine Anpassung (direkte Rückgabe eines abgeleiteten Stores) wird er noch einfacher und passt perfekt zu den neuen Features wie dem `@`-Operator.
-   Der optimierte Code verbessert die Lesbarkeit und den Workflow bei der Nutzung in Svelte 5.

----------

### **Optimierungen für Svelte 5**

Obwohl der Store funktioniert, könnten wir ihn anpassen, um von neuen Features in Svelte 5 zu profitieren:

#### **Optimierung: Automatische Dereferenzierung**

In Svelte 5 kannst du direkt auf die Inhalte eines Stores zugreifen, indem du `@` verwendest. Damit das reibungslos funktioniert, sollte die Rückgabe des Stores sauber und präzise gestaltet sein.

Anpassung:

```javascript
return derived(href, ($href) => new URL($href));

```

-   Statt ein Objekt mit `{ subscribe }` zurückzugeben, wird direkt der abgeleitete Store zurückgegeben.

Das hat zwei Vorteile:

1.  Der Store wird einfacher und klarer.
2.  Er ist direkt kompatibel mit der neuen Svelte-5-Syntax.

----------

### **Vollständiger, angepasster Store**

Hier ist der aktualisierte Code, optimiert für Svelte 5:

```javascript
import { derived, writable } from 'svelte/store';

export function createUrlStore(ssrUrl) {
	if (typeof window === 'undefined') {
		return writable(new URL(ssrUrl));
	}

	const href = writable(window.location.href);

	const originalPushState = history.pushState;
	const originalReplaceState = history.replaceState;

	const updateHref = () => href.set(window.location.href);

	history.pushState = function () {
		originalPushState.apply(this, arguments);
		updateHref();
	};

	history.replaceState = function () {
		originalReplaceState.apply(this, arguments);
		updateHref();
	};

	window.addEventListener('popstate', updateHref);
	window.addEventListener('hashchange', updateHref);

	return derived(href, ($href) => new URL($href));
}

export default createUrlStore();

```

----------

### **Verwendung in Svelte 5**

#### Direkter Zugriff mit `@`:

```svelte
<script>
  import urlStore from './path/to/store';
</script>

<p>Aktuelle URL: {@urlStore.href}</p>
<p>Pfad: {@urlStore.pathname}</p>

```

#### Nutzung in reaktiven Statements:

```svelte
<script>
  import urlStore from './path/to/store';
</script>

{#if @urlStore.pathname === '/home'}
  <p>Willkommen auf der Startseite!</p>
{/if}

```

----------

### **Zusammenfassung:**

-   Der ursprüngliche Store ist **mit Svelte 5 kompatibel**, da er die grundlegenden Store-Schnittstellen (`subscribe`) implementiert.
-   Durch eine kleine Anpassung (direkte Rückgabe eines abgeleiteten Stores) wird er noch einfacher und passt perfekt zu den neuen Features wie dem `@`-Operator.
-   Der optimierte Code verbessert die Lesbarkeit und den Workflow bei der Nutzung in Svelte 5.

---



Einen URL-Wechsel in einer Svelte-Anwendung zu triggern, hängt davon ab, ob du eine vollständige Seitenaktualisierung (Page Reload) durchführen möchtest oder nur den URL ändern willst, ohne die Seite neu zu laden. Hier sind die wichtigsten Methoden:

----------

### **1. URL ändern ohne Neuladen (Single-Page Application)**

Wenn du nur die URL ändern möchtest, ohne die Seite neu zu laden, kannst du die `history.pushState`- oder `history.replaceState`-Methoden der Browser-API verwenden. Diese Methoden ändern die URL im Browser, ohne dass die Seite neu geladen wird.

#### **Beispiel: URL ändern**

```javascript
history.pushState({}, '', '/neue-url');

```

-   **`pushState`**: Fügt einen neuen Eintrag in den Verlauf (History) ein. Der Benutzer kann mit den Zurück-/Vorwärts-Buttons navigieren.
-   **`replaceState`**: Ersetzt den aktuellen Verlaufseintrag. Die Zurück-Navigation ist nicht möglich.

#### **Verwendung in Svelte**

Du kannst dies in einem Skript in einer Svelte-Komponente tun:

```svelte
<script>
  function changeUrl() {
    history.pushState({}, '', '/neue-url');
  }
</script>

<button on:click={changeUrl}>URL ändern</button>

```

----------

### **2. URL ändern und Seite neu laden**

Wenn du die URL ändern und die Seite komplett neu laden möchtest, kannst du die `window.location`-Eigenschaft verwenden.

#### **Beispiel: URL setzen und neu laden**

```javascript
window.location.href = '/neue-url';

```

#### **Verwendung in Svelte**

```svelte
<script>
  function reloadAndChangeUrl() {
    window.location.href = '/neue-url';
  }
</script>

<button on:click={reloadAndChangeUrl}>URL ändern und Seite neu laden</button>

```

----------

### **3. Mit dem URL-Store (aus deinem Beispiel)**

Wenn du einen URL-Store wie den in deinem Beispiel verwendest, kannst du die URL reaktiv ändern, indem du `history.pushState` in Kombination mit dem Store nutzt.

#### **Beispiel: URL-Wechsel mit einem Store**

Hier ist eine angepasste Funktion für den Wechsel der URL:

```javascript
function setUrl(newUrl) {
  history.pushState({}, '', newUrl);
}

```

#### **Verwendung in Svelte:**

```svelte
<script>
  import urlStore from './path/to/store';

  function changeToNewUrl() {
    history.pushState({}, '', '/new-page');
    // Der Store wird automatisch aktualisiert, weil er auf pushState lauscht
  }
</script>

<button on:click={changeToNewUrl}>Zur neuen Seite wechseln</button>

```

----------

### **4. URL mit Query-Parametern ändern**

Wenn du nur die Abfrageparameter (`?param=value`) ändern möchtest, kannst du `URLSearchParams` verwenden:

#### **Beispiel: Query-Parameter hinzufügen oder ändern**

```javascript
const params = new URLSearchParams(window.location.search);
params.set('name', 'Max');
history.pushState({}, '', `${window.location.pathname}?${params}`);

```

#### **Verwendung in Svelte:**

```svelte
<script>
  function addQueryParam() {
    const params = new URLSearchParams(window.location.search);
    params.set('lang', 'de');
    history.pushState({}, '', `${window.location.pathname}?${params}`);
  }
</script>

<button on:click={addQueryParam}>Sprache auf Deutsch setzen</button>

```

----------

### **5. Hash-Änderungen**

Für Änderungen des Hash-Anteils (`#`) der URL kannst du einfach `window.location.hash` verwenden:

#### **Beispiel: Hash setzen**

```javascript
window.location.hash = 'section1';

```

#### **Verwendung in Svelte:**

```svelte
<script>
  function setHash() {
    window.location.hash = 'section2';
  }
</script>

<button on:click={setHash}>Zum Abschnitt 2 springen</button>

```

----------

### **Zusammenfassung der Methoden**

Methode

Beschreibung

Seiten-Neuladen

`history.pushState`

Ändert die URL, fügt neuen Verlaufseintrag hinzu

Nein

`history.replaceState`

Ändert die URL, ersetzt aktuellen Verlaufseintrag

Nein

`window.location.href`

Ändert die URL und lädt die Seite neu

Ja

`window.location.hash`

Ändert nur den Hash-Anteil der URL

Nein

`URLSearchParams` + `pushState`

Modifiziert die Query-Parameter der URL

Nein

Für Single-Page Applications (SPA) wird meistens **`history.pushState`** bevorzugt, um eine reaktive Benutzererfahrung ohne Neuladen zu ermöglichen.
