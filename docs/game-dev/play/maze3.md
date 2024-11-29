---
title: GameDev 3
tags: 
    - Gamedev
    - JS
sidebar_position: 22
sidebar_label: GameDev 3
---


Um den Inhalt der einzelnen Schritte in der Klasse zu enthalten, können wir die Klasse so erweitern, dass sie die Fragen, den Eingabetyp und die Information, ob ein Feld erforderlich ist, speichert. Hier ist eine überarbeitete Version der `Questionnaire`-Klasse und des HTML-Gerüsts, die dies ermöglicht:

### HTML und JavaScript

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fragebogen - Fantastic-Plastic</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            padding: 20px;
        }

        .questionnaire {
            max-width: 600px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .question {
            margin-bottom: 20px;
        }

        .question label {
            display: block;
            margin-bottom: 10px;
        }

        .question input[type="text"],
        .question input[type="number"],
        .question select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .question button {
            display: inline-block;
            padding: 10px 20px;
            background: #007bff;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .question button[disabled] {
            background: #ccc;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <div class="questionnaire" id="questionnaire"></div>

    <script>
        class Questionnaire {
            constructor(containerId) {
                this.steps = [
                    { question: "Name:", type: "text", required: true },
                    { question: "Alter:", type: "number", required: true },
                    { question: "Interesse an (PVC, Gummi, Latex):", type: "select", options: ["PVC", "Gummi", "Latex"], required: true }
                ];
                this.currentStep = 0;
                this.container = document.getElementById(containerId);
                this.renderStep();
            }

            renderStep() {
                const step = this.steps[this.currentStep];
                this.container.innerHTML = '';

                const questionDiv = document.createElement('div');
                questionDiv.classList.add('question');

                const label = document.createElement('label');
                label.textContent = step.question;
                questionDiv.appendChild(label);

                let input;
                if (step.type === 'select') {
                    input = document.createElement('select');
                    input.name = `step-${this.currentStep}`;
                    input.required = step.required;

                    const defaultOption = document.createElement('option');
                    defaultOption.value = '';
                    defaultOption.textContent = 'Bitte auswählen';
                    input.appendChild(defaultOption);

                    step.options.forEach(optionValue => {
                        const option = document.createElement('option');
                        option.value = optionValue.toLowerCase();
                        option.textContent = optionValue;
                        input.appendChild(option);
                    });
                } else {
                    input = document.createElement('input');
                    input.type = step.type;
                    input.name = `step-${this.currentStep}`;
                    input.required = step.required;
                }

                questionDiv.appendChild(input);

                const button = document.createElement('button');
                button.textContent = 'Weiter';
                button.onclick = () => this.nextStep();
                questionDiv.appendChild(button);

                this.container.appendChild(questionDiv);
            }

            nextStep() {
                const currentInputs = this.container.querySelectorAll('input, select');
                let allValid = true;
                currentInputs.forEach(input => {
                    if (!input.checkValidity()) {
                        allValid = false;
                    }
                });

                if (allValid) {
                    this.currentStep++;
                    if (this.currentStep < this.steps.length) {
                        this.renderStep();
                    } else {
                        this.showCompletionMessage();
                    }
                } else {
                    alert('Bitte füllen Sie alle Felder aus, bevor Sie fortfahren.');
                }
            }

            showCompletionMessage() {
                this.container.innerHTML = '<div class="question"><p>Vielen Dank für Ihre Bewerbung!</p></div>';
            }
        }

        const questionnaire = new Questionnaire('questionnaire');
    </script>
</body>
</html>

```

### Erklärung:

1.  **HTML-Struktur:**
    
    -   Ein einzelnes `div`-Element mit der ID `questionnaire`, in dem der gesamte Fragebogen dynamisch gerendert wird.
2.  **JavaScript-Klasse `Questionnaire`:**
    
    -   **`constructor(containerId)`**: Initialisiert die Schritte und setzt den aktuellen Schritt auf 0. `containerId` wird verwendet, um das `div`-Element zu identifizieren, in dem die Fragen gerendert werden sollen.
    -   **`steps`**: Ein Array, das die Fragen, den Eingabetyp und zusätzliche Optionen für den `select`-Eingabetyp enthält.
    -   **`renderStep()`**: Diese Methode rendert den aktuellen Schritt basierend auf dem `currentStep`. Sie erstellt die entsprechenden Eingabeelemente (`input`, `select`) und fügt sie in das Container-Element ein.
    -   **`nextStep()`**: Überprüft, ob alle Eingabefelder im aktuellen Schritt ausgefüllt sind und gültig sind. Wenn ja, wird der nächste Schritt gerendert; andernfalls wird eine Warnung angezeigt.
    -   **`showCompletionMessage()`**: Zeigt eine Abschlussnachricht an, wenn alle Schritte abgeschlossen sind.

Diese Lösung ermöglicht eine flexible Verwaltung der Schritte und ihrer Inhalte innerhalb der `Questionnaire`-Klasse. Der Fragebogen wird dynamisch gerendert und die Benutzerführung erfolgt in einem klaren, benutzerfreundlichen Workflow.


Um die Möglichkeit hinzuzufügen, dynamisch weitere Schritte zum Fragebogen hinzuzufügen, können wir die Klasse `Questionnaire` um eine Methode `addStep` erweitern. Diese Methode ermöglicht es, neue Schritte zu definieren und dem bestehenden Fragenkatalog hinzuzufügen.

Hier ist der aktualisierte Code:

### HTML und JavaScript

```html
<!DOCTYPE html>
<html lang="de">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Fragebogen - Fantastic-Plastic</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f4f4;
            color: #333;
            padding: 20px;
        }

        .questionnaire {
            max-width: 600px;
            margin: 0 auto;
            background: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        .question {
            margin-bottom: 20px;
        }

        .question label {
            display: block;
            margin-bottom: 10px;
        }

        .question input[type="text"],
        .question input[type="number"],
        .question select {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }

        .question button {
            display: inline-block;
            padding: 10px 20px;
            background: #007bff;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        .question button[disabled] {
            background: #ccc;
            cursor: not-allowed;
        }
    </style>
</head>
<body>
    <div class="questionnaire" id="questionnaire"></div>

    <script>
        class Questionnaire {
            constructor(containerId) {
                this.steps = [
                    { question: "Name:", type: "text", required: true },
                    { question: "Alter:", type: "number", required: true },
                    { question: "Interesse an (PVC, Gummi, Latex):", type: "select", options: ["PVC", "Gummi", "Latex"], required: true }
                ];
                this.currentStep = 0;
                this.container = document.getElementById(containerId);
                this.renderStep();
            }

            addStep(question, type, options = [], required = false) {
                this.steps.push({ question, type, options, required });
            }

            renderStep() {
                const step = this.steps[this.currentStep];
                this.container.innerHTML = '';

                const questionDiv = document.createElement('div');
                questionDiv.classList.add('question');

                const label = document.createElement('label');
                label.textContent = step.question;
                questionDiv.appendChild(label);

                let input;
                if (step.type === 'select') {
                    input = document.createElement('select');
                    input.name = `step-${this.currentStep}`;
                    input.required = step.required;

                    const defaultOption = document.createElement('option');
                    defaultOption.value = '';
                    defaultOption.textContent = 'Bitte auswählen';
                    input.appendChild(defaultOption);

                    step.options.forEach(optionValue => {
                        const option = document.createElement('option');
                        option.value = optionValue.toLowerCase();
                        option.textContent = optionValue;
                        input.appendChild(option);
                    });
                } else {
                    input = document.createElement('input');
                    input.type = step.type;
                    input.name = `step-${this.currentStep}`;
                    input.required = step.required;
                }

                questionDiv.appendChild(input);

                const button = document.createElement('button');
                button.textContent = 'Weiter';
                button.onclick = () => this.nextStep();
                questionDiv.appendChild(button);

                this.container.appendChild(questionDiv);
            }

            nextStep() {
                const currentInputs = this.container.querySelectorAll('input, select');
                let allValid = true;
                currentInputs.forEach(input => {
                    if (!input.checkValidity()) {
                        allValid = false;
                    }
                });

                if (allValid) {
                    this.currentStep++;
                    if (this.currentStep < this.steps.length) {
                        this.renderStep();
                    } else {
                        this.showCompletionMessage();
                    }
                } else {
                    alert('Bitte füllen Sie alle Felder aus, bevor Sie fortfahren.');
                }
            }

            showCompletionMessage() {
                this.container.innerHTML = '<div class="question"><p>Vielen Dank für Ihre Bewerbung!</p></div>';
            }
        }

        const questionnaire = new Questionnaire('questionnaire');

        // Beispiel für das Hinzufügen eines neuen Schritts
        questionnaire.addStep("Was ist dein Lieblingskostüm?", "text", [], true);
    </script>
</body>
</html>

```

### Erklärung der neuen Funktion:

1.  **`addStep(question, type, options = [], required = false)`**:
    
    -   Diese Methode fügt einen neuen Schritt zum Fragebogen hinzu.
    -   **Parameter**:
        -   `question`: Die Frage, die im Schritt angezeigt werden soll.
        -   `type`: Der Typ des Eingabefelds (`text`, `number`, `select`, etc.).
        -   `options`: Eine optionale Liste von Optionen (nur für den `select`-Typ).
        -   `required`: Ein boolean, der angibt, ob das Feld erforderlich ist oder nicht (Standardwert: `false`).
2.  **Beispiel für das Hinzufügen eines neuen Schritts**:
    
    -   `questionnaire.addStep("Was ist dein Lieblingskostüm?", "text", [], true);`
    -   Fügt einen neuen Schritt mit einer Textfrage hinzu, die ausgefüllt werden muss.

Mit dieser Erweiterung kannst du den Fragebogen flexibel erweitern, indem du neue Schritte dynamisch hinzufügst. Jeder Schritt wird in der `steps`-Array gespeichert und entsprechend gerendert, wenn der Benutzer durch den Fragebogen navigiert.
