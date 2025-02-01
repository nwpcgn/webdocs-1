---
title: Notification Typewriter
sidebar_label: Typewriter
sidebar_position: 93
---



Wenn du die `notification`-Variable definieren möchtest, bevor das DOM vollständig geladen ist, kannst du sie zunächst als `null` deklarieren und sie später im `DOMContentLoaded`-Event-Listener initialisieren. Dadurch wird sichergestellt, dass die Variable existiert, auch wenn sie noch nicht auf ein Element verweist.

Hier ist die angepasste Version des Codes:

### 1. HTML

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Notification Typewriter</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="notification-window"></div>
    <script src="script.js"></script>
</body>
</html>
```

### 2. CSS (styles.css)

```css
#notification-window {
    position: fixed;
    bottom: 20px;
    left: 20px;
    padding: 20px;
    background-color: #333;
    color: #fff;
    border-radius: 5px;
    font-family: 'Courier New', Courier, monospace;
    white-space: pre-wrap;
    overflow: hidden;
    width: 300px;
    height: 50px;
    display: none;
}

.typewriter-text {
    display: inline-block;
    border-right: 2px solid;
    animation: blink-caret 0.75s step-end infinite;
}

@keyframes blink-caret {
    from, to {
        border-color: transparent;
    }
    50% {
        border-color: #fff;
    }
}
```

### 3. TypeScript (script.ts)

```typescript
class Notification {
    private messages: string[] = [];
    private currentIndex: number = 0;
    private notificationWindow: HTMLElement;

    constructor(notificationWindow: HTMLElement) {
        this.notificationWindow = notificationWindow;
    }

    public addMessage(message: string) {
        this.messages.push(message);
        if (this.messages.length === 1) {
            this.showNextMessage();
        }
    }

    private async showNextMessage() {
        if (this.currentIndex >= this.messages.length) {
            this.currentIndex = 0;
            this.messages = [];
            this.notificationWindow.style.display = 'none';
            return;
        }

        const message = this.messages[this.currentIndex];
        await this.displayMessageWithTypewriterEffect(message);
        this.currentIndex++;
        setTimeout(() => this.showNextMessage(), 3000);
    }

    private displayMessageWithTypewriterEffect(message: string): Promise<void> {
        return new Promise((resolve) => {
            this.notificationWindow.innerHTML = '';
            this.notificationWindow.style.display = 'block';

            const typewriterSpan = document.createElement('span');
            typewriterSpan.classList.add('typewriter-text');
            this.notificationWindow.appendChild(typewriterSpan);

            let charIndex = 0;

            const typewriterInterval = setInterval(() => {
                if (charIndex < message.length) {
                    typewriterSpan.textContent += message[charIndex];
                    charIndex++;
                } else {
                    clearInterval(typewriterInterval);
                    resolve();
                }
            }, 100); // Adjust the speed of typewriter effect by changing this value
        });
    }
}

let notification: Notification | null = null;

document.addEventListener('DOMContentLoaded', () => {
    const notificationWindow = document.getElementById('notification-window');
    if (notificationWindow) {
        notification = new Notification(notificationWindow);

        // Example usage:
        notification.addMessage('First message to display.');
        notification.addMessage('Second message to display.');
        notification.addMessage('Third message to display.');
    } else {
        console.error('Notification window element not found');
    }
});

// Example of how to add messages after the DOM has loaded
function addMessageToNotification(message: string) {
    if (notification) {
        notification.addMessage(message);
    } else {
        console.error('Notification not initialized');
    }
}
```

### 4. JavaScript Compilation

Stelle sicher, dass du den TypeScript-Code in JavaScript kompilierst:

```bash
tsc script.ts
```

### Erklärung

- **HTML und CSS**: Unverändert.
- **TypeScript**:
  - Die `notification`-Variable wird als `null` deklariert.
  - Im `DOMContentLoaded`-Event-Listener wird das `notificationWindow`-Element gesucht und eine neue `Notification`-Instanz erstellt, wobei `notification` initialisiert wird.
  - Die Funktion `addMessageToNotification` ermöglicht es, Nachrichten hinzuzufügen, nachdem die Seite geladen wurde. Diese Funktion überprüft, ob die `notification`-Instanz initialisiert ist, bevor sie Nachrichten hinzufügt.