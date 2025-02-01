--- 
title: Cyber Game 1
sidebar_label: CP I
sidebar_position: 711
---



Um Hindernisse zu deinem Spiel hinzuzufügen, erstellen wir eine `Obstacle`-Klasse, die Hindernisse darstellt und sich auf den Spieler zubewegt. Wir fügen auch eine Kollisionsfunktion hinzu, um Kollisionen zwischen dem Spieler und den Hindernissen zu erkennen. Wenn eine Kollision auftritt, wird das Spiel entweder gestoppt oder eine Funktion ausgeführt, um z.B. den Punktestand zu erhöhen.

Hier ist die aktualisierte Version des Codes:

1. **HTML-Datei**:

   ```html
   <!DOCTYPE html>
   <html lang="de">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Canvas Animation in TypeScript</title>
   </head>
   <body>
       <canvas id="myCanvas" width="800" height="600"></canvas>
       <script src="main.js"></script>
   </body>
   </html>
   ```

2. **TypeScript-Datei (main.ts)**:

   ```typescript
   let canvas: HTMLCanvasElement | null = null;
   let context: CanvasRenderingContext2D | null = null;

   interface Animation {
       image: HTMLImageElement;
       frameCount: number;
       frameWidth: number;
       frameHeight: number;
   }

   class Player {
       x: number;
       y: number;
       width: number;
       height: number;
       animations: { [key: string]: Animation };
       currentAnimation: string;
       currentFrame: number;
       frameDelay: number;
       frameDelayCounter: number;
       velocityY: number;
       gravity: number;
       jumpStrength: number;
       isJumping: boolean;
       isRunning: boolean;

       constructor(x: number, y: number, width: number, height: number, animations: { [key: string]: Animation }) {
           this.x = x;
           this.y = y;
           this.width = width;
           this.height = height;
           this.animations = animations;
           this.currentAnimation = 'idle';
           this.currentFrame = 0;
           this.frameDelay = 10;
           this.frameDelayCounter = 0;
           this.velocityY = 0;
           this.gravity = 0.5;
           this.jumpStrength = -10;
           this.isJumping = false;
           this.isRunning = false;
       }

       setAnimation(animation: string) {
           if (this.currentAnimation !== animation) {
               this.currentAnimation = animation;
               this.currentFrame = 0;
               this.frameDelayCounter = 0;

               if (animation === 'jump') {
                   this.velocityY = this.jumpStrength;
                   this.isJumping = true;
               }
           }
       }

       update() {
           this.frameDelayCounter++;
           if (this.frameDelayCounter >= this.frameDelay) {
               this.currentFrame = (this.currentFrame + 1) % this.animations[this.currentAnimation].frameCount;
               this.frameDelayCounter = 0;
           }

           this.y += this.velocityY;
           this.velocityY += this.gravity;

           if (this.y >= 400) {
               this.y = 400;
               this.velocityY = 0;
               this.isJumping = false;
               if (this.currentAnimation === 'jump') {
                   this.setAnimation('idle');
               }
           }
       }

       draw(ctx: CanvasRenderingContext2D) {
           const animation = this.animations[this.currentAnimation];
           const frameX = this.currentFrame * animation.frameWidth;
           ctx.drawImage(
               animation.image,
               frameX, 0,
               animation.frameWidth, animation.frameHeight,
               this.x, this.y,
               this.width, this.height
           );
       }

       checkCollision(obstacle: Obstacle): boolean {
           return (
               this.x < obstacle.x + obstacle.width &&
               this.x + this.width > obstacle.x &&
               this.y < obstacle.y + obstacle.height &&
               this.y + this.height > obstacle.y
           );
       }
   }

   class Obstacle {
       x: number;
       y: number;
       width: number;
       height: number;
       speed: number;
       image: HTMLImageElement;

       constructor(x: number, y: number, width: number, height: number, speed: number, image: HTMLImageElement) {
           this.x = x;
           this.y = y;
           this.width = width;
           this.height = height;
           this.speed = speed;
           this.image = image;
       }

       update() {
           this.x -= this.speed;
       }

       draw(ctx: CanvasRenderingContext2D) {
           ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
       }
   }

   class BackgroundLayer {
       image: HTMLImageElement;
       speed: number;
       x: number;

       constructor(image: HTMLImageElement, speed: number) {
           this.image = image;
           this.speed = speed;
           this.x = 0;
       }

       update(isRunning: boolean) {
           if (isRunning || this.speed < 1) {
               this.x -= this.speed;
               if (this.x <= -this.image.width) {
                   this.x = 0;
               }
           }
       }

       draw(ctx: CanvasRenderingContext2D) {
           ctx.drawImage(this.image, this.x, 0);
           ctx.drawImage(this.image, this.x + this.image.width, 0);
       }
   }

   function initCanvas() {
       canvas = document.getElementById('myCanvas') as HTMLCanvasElement;
       if (!canvas) {
           console.error('Canvas-Element nicht gefunden');
           return;
       }

       context = canvas.getContext('2d');
       if (!context) {
           console.error('2D-Kontext konnte nicht abgerufen werden');
           return;
       }

       loadImagesAndStartGame();
   }

   function loadImage(src: string): Promise<HTMLImageElement> {
       return new Promise((resolve, reject) => {
           const img = new Image();
           img.onload = () => resolve(img);
           img.onerror = () => reject(new Error(`Fehler beim Laden des Bildes: ${src}`));
           img.src = src;
       });
   }

   async function loadImagesAndStartGame() {
       try {
           const [idleImage, runImage, jumpImage, cloudsImage, skylineImage, buildingsImage, obstacleImage] = await Promise.all([
               loadImage('idle.png'),
               loadImage('run.png'),
               loadImage('jump.png'),
               loadImage('clouds.png'),
               loadImage('skyline.png'),
               loadImage('buildings.png'),
               loadImage('obstacle.png')
           ]);

           const animations: { [key: string]: Animation } = {
               idle: {
                   image: idleImage,
                   frameCount: 4,
                   frameWidth: idleImage.width / 4,
                   frameHeight: idleImage.height
               },
               run: {
                   image: runImage,
                   frameCount: 6,
                   frameWidth: runImage.width / 6,
                   frameHeight: runImage.height
               },
               jump: {
                   image: jumpImage,
                   frameCount: 4,
                   frameWidth: jumpImage.width / 4,
                   frameHeight: jumpImage.height
               }
           };

           const clouds = new BackgroundLayer(cloudsImage, 0.5);
           const skyline = new BackgroundLayer(skylineImage, 1.5);
           const buildings = new BackgroundLayer(buildingsImage, 3);

           const obstacle = new Obstacle(800, 400, 50, 50, 3, obstacleImage); // Beispielwert für ein Hindernis

           startGame(animations, [clouds, skyline, buildings], [obstacle]);
       } catch (error) {
           console.error('Fehler beim Laden der Bilder:', error);
       }
   }

   function startGame(animations: { [key: string]: Animation }, backgroundLayers: BackgroundLayer[], obstacles: Obstacle[]) {
       const player = new Player(50, 400, 100, 100, animations);
       let score = 0;

       function gameLoop() {
           if (context && canvas) {
               context.clearRect(0, 0, canvas.width, canvas.height);

               for (const layer of backgroundLayers) {
                   layer.update(player.isRunning);
                   layer.draw(context);
               }

               for (const obstacle of obstacles) {
                   obstacle.update();
                   obstacle.draw(context);

                   if (player.checkCollision(obstacle)) {
                       console.log('Kollision erkannt!');
                       player.isRunning = false;
                       return;
                   }
               }

               player.update();
               player.draw(context);

               // Score anzeigen
               context.fillStyle = 'black';
               context.font = '20px Arial';
               context.fillText(`Score: ${score}`, 10, 20);

               if (player.isRunning) {
                   score++;
               }
           }
           requestAnimationFrame(gameLoop);
       }

       document.addEventListener('keydown', (event) => {
           if (event.key === 'ArrowRight') {
               player.setAnimation('run');
               player.isRunning = true;
           } else if (event.key === ' ') {
               if (!player.isJumping) {
                   player.setAnimation('jump');
               }
           }
       });

       document.addEventListener('keyup', (event) => {
           if (event.key === 'ArrowRight') {
               player.setAnimation('idle');
               player.isRunning = false;
           }
       });

       gameLoop();
   }

   window.onload = initCanvas;
   ```

### Erklärung:

1. **Player-Klasse**:
  

 - Die Methode `checkCollision` wurde hinzugefügt, um Kollisionen zwischen dem Spieler und einem Hindernis zu erkennen.

2. **Obstacle-Klasse**:
   - Diese Klasse repräsentiert ein Hindernis, das sich auf den Spieler zubewegt. Sie hat Methoden `update` und `draw`, um die Position zu aktualisieren und das Hindernis zu zeichnen.

3. **Spiel-Logik**:
   - Ein `obstacle` wird in der `loadImagesAndStartGame`-Funktion erstellt und zum Spiel hinzugefügt.
   - Im `gameLoop` wird überprüft, ob der Spieler mit einem Hindernis kollidiert. Wenn eine Kollision erkannt wird, wird das Spiel gestoppt und eine Meldung ausgegeben.
   - Der Punktestand (`score`) wird erhöht, solange der Spieler läuft, und auf dem Bildschirm angezeigt.

Mit dieser Erweiterung hast du nun Hindernisse, die der Spieler überspringen muss, sowie eine einfache Kollisionslogik, die das Spielverhalten beeinflusst.

---

Wie zufrieden bist du mit dieser Lösung auf einer Skala von 1 bis 7? Deine Rückmeldung hilft mir, meine Antworten weiter zu verbessern. 😊
