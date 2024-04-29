---
title: StarField
sidebar_position: 1
sidebar_label: Star-Field
---


##  Render Starfield Background on Canvas

:::tip[My tip]

Use this awesome feature option

:::

:::danger[Take care]

This action is dangerous

:::



```javascript
/**
  Render Starfield Background on Canvas  
  <canvas class="canvas"></canvas>
*/

let starfields = [];
let canvas = document.querySelector(".canvas");
let ctx = canvas.getContext("2d");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

class star {
    constructor(size, x, y) {
        this.size = size;
        this.x = x;
        this.y = y;
    }
}

class Starfield {
    constructor(scale, velocityX, velocityY) {
        this.stars = [];
        this.scale = scale;
        this.velocityX = velocityX;
        this.velocityY = velocityY;
        this.totalElapsedMilliseconds = 0;
        let starCount = Math.random() * 100 + 3;
        for (let i = 0; i < starCount; ++i) {
            let radius = scale * (Math.random() * 3) + 1;
            let x = Math.random() * canvas.width + 20;
            let y = Math.random() * canvas.height + 20;
            this.stars.push(new star(radius, x, y));
        }
    }

    update(elapsed) {
        let elapsedMsSinceLastFrame = elapsed - this.totalElapsedMilliseconds;
        let xMove = elapsedMsSinceLastFrame * this.velocityX;
        let yMove = elapsedMsSinceLastFrame * this.velocityY;
        if (isNaN(xMove) || isNaN(yMove)) return;
        for (let i = 0; i < this.stars.length; ++i) {
            // 1. move stars
            // 2. if the star is out of bounds, move it to ensure it gets displayed again

            this.stars[i].x += xMove;
            this.stars[i].y += yMove;
            if (this.stars[i].x < 0) {
                this.stars[i].x = canvas.width;
            }
            if (this.stars[i].x > canvas.width) {
                this.stars[i].x = 0;
            }
            if (this.stars[i].y < 0) {
                this.stars[i].y = canvas.height;
            }
            if (this.stars[i].y > canvas.height) {
                this.stars[i].y = 0;
            }
        }

        this.totalElapsedMilliseconds = elapsed;
    }

    render() {
        for (let i = 0; i < this.stars.length; ++i) {
            ctx.beginPath();
            ctx.fillStyle = "white";
            ctx.arc(
                this.stars[i].x,
                this.stars[i].y,
                this.stars[i].size,
                0,
                2 * Math.PI,
                false
            );
            ctx.fill();
        }
    }
}

function renderBackground() {
    for (let i = 0; i < starfields.length; ++i) {
        starfields[i].render();
    }
}

function setup() {
    starfields.push(new Starfield(0.1, 0.01, 0.005));
    starfields.push(new Starfield(0.6, 0.02, 0.022));
    starfields.push(new Starfield(1.1, 0.04, 0.028));
}

function update(elapsed) {
    for (let i = 0; i < starfields.length; ++i) {
        starfields[i].update(elapsed);
    }
}

function render() {
    ctx.fillStyle = "#151d26";
    ctx.fillRect(0, 0, canvas.width, canvas.height);
    renderBackground();
}

function setFrame() {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
}

const run = (e) => {
    update(e);
    render();
    window.requestAnimationFrame(run);
};

window.addEventListener("resize", setFrame, false);
window.addEventListener("load", setFrame);

setup();
run();
```
