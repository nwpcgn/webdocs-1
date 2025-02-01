---
title: Dynamisches Bootstrap 5 Modal 
tags: 
    - Bootstrap
    - JS
sidebar_position: 32
sidebar_label: Bs Modal
---




> Du kannst ein Bootstrap-Modal dynamisch erstellen! Hier ist ein Beispiel, wie du ein Modal zur Laufzeit mit JavaScript hinzufügst und anzeigst:

---

### **Dynamisches Bootstrap-Modal erstellen**

```html
<button id="openModalBtn" class="btn btn-primary">Open Modal</button>

<script>
    document.getElementById('openModalBtn').addEventListener('click', function() {
    // Modal-HTML erstellen
    const modalHtml = `
        <div class="modal fade" id="dynamicModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog">
            <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Dynamisches Modal</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>
            <div class="modal-body">
                Dies ist ein dynamisch erstelltes Modal!
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Schließen</button>
                <button type="button" class="btn btn-primary">Speichern</button>
            </div>
            </div>
        </div>
        </div>
    `;

    // Modal zum DOM hinzufügen
    document.body.insertAdjacentHTML('beforeend', modalHtml);

    // Modal-Instanz erstellen und anzeigen
    const modal = new bootstrap.Modal(document.getElementById('dynamicModal'));
    modal.show();

    // Event-Listener für das Entfernen des Modals nach dem Schließen
    const modalElement = document.getElementById('dynamicModal');
    modalElement.addEventListener('hidden.bs.modal', () => {
        modalElement.remove(); // Modal aus dem DOM entfernen
    });
});

```

---

### Usage
1. **Modal-HTML dynamisch erzeugen** – Das HTML für das Modal wird als Template-String erstellt.  
2. **In den DOM einfügen** – Das Modal wird mit `insertAdjacentHTML` zur Laufzeit zum DOM hinzugefügt.  
3. **Bootstrap-Modal initialisieren und anzeigen** – Mit `new bootstrap.Modal()` wird das Modal gesteuert.  
4. **Entfernen nach dem Schließen** – Mit einem Event-Listener auf `hidden.bs.modal` wird das Modal nach dem Schließen aus dem DOM entfernt, um Speicherplatz freizugeben.

---

**Hinweis**: Dies funktioniert für **Bootstrap 5**. Für frühere Versionen könnten kleine Anpassungen erforderlich sein.