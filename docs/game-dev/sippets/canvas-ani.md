---
title: Animation Part Canvas
sidebar_position: 11
sidebar_label: Canvas 1
---


## Canvas Animation

Dies ist eine Funktion, die ein `canvas`-Element erstellt und die Animation des Spritesheets abspielt:

```javascript title="canvas.js"
function createAnimatedCanvas(imageSrc, frameWidth, frameHeight, frameCount, fps, scale = 1) {
    const canvas = document.createElement("canvas");
    const ctx = canvas.getContext("2d");
    document.body.appendChild(canvas);
    
    const img = new Image();
    img.src = imageSrc;
    
    let currentFrame = 0;
    canvas.width = frameWidth * scale;
    canvas.height = frameHeight * scale;
    
    function animate() {
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.drawImage(
            img, 
            currentFrame * frameWidth, 0, frameWidth, frameHeight, 
            0, 0, frameWidth * scale, frameHeight * scale
        );
        
        currentFrame = (currentFrame + 1) % frameCount;
        setTimeout(() => requestAnimationFrame(animate), 1000 / fps);
    }
    
    img.onload = () => animate();
}

// Beispielaufruf
document.addEventListener("DOMContentLoaded", () => {
    createAnimatedCanvas("frame-hummer-a.png", 64, 128, 8, 12, 2); // Skalierung hinzugefügt
});


```