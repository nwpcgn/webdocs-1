--- 
title: Cyber Game 2
sidebar_label: CP II
sidebar_position: 712
---


Um Hindernisse zu deinem Spiel hinzuzufügen, erstellen wir eine `Obstacle`-Klasse, die Hindernisse darstellt und sich auf den Spieler zubewegt. Wir fügen auch eine Kollisionsfunktion hinzu, um Kollisionen zwischen dem Spieler und den Hindernissen zu erkennen. Wenn eine Kollision auftritt, wird das Spiel entweder gestoppt oder eine Funktion ausgeführt, um z.B. den Punktestand zu erhöhen.





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

## Svelte Comp


```html
<script lang="ts">
	import { onMount } from 'svelte'

	let config = {
		baseX: 250,
		baseY: 500,
		punkW: 100,
		punkH: 100,
		hitbox: {
			width: 38,
			height: 75,
			offsetX: 10,
			offsetY: 25,
			strokeColor: 'red'
		},
		obstacles: [
			{
				width: 50,
				height: 50,
				y: 550,
				speed: 1.1,
				imageSrc: '/img/cp/obstacle1.png'
			},
			{
				width: 60,
				height: 60,
				y: 540,
				speed: 1.1,
				imageSrc: '/img/cp/obstacle1.png'
			},
			{
				width: 40,
				height: 40,
				y: 560,
				speed: 1.1,
				imageSrc: '/img/cp/obstacle1.png'
			}
		],
		bgs: [
			{
				key: 'backyard',
				src: '/img/cp/backyard.png',
				type: 'layer',
				width: 1066,
				height: 600,
				frames: 0
			},
			{
				key: 'buildings',
				src: '/img/cp/buildings.png',
				type: 'layer',
				width: 1066,
				height: 600,
				frames: 0
			},
			{
				key: 'clouds',
				src: '/img/cp/clouds.png',
				type: 'layer',
				width: 1066,
				height: 600,
				frames: 0
			},
			{
				key: 'skyline',
				src: '/img/cp/skyline.png',
				type: 'layer',
				width: 1066,
				height: 600,
				frames: 0
			}
		],
		sprites: [
			{
				key: 'idle',
				src: '/img/cp/idle.png',
				type: 'sprite',
				width: 96,
				height: 96,
				frames: 4
			},
			{
				key: 'jump',
				src: '/img/cp/jump 1.png',
				type: 'sprite',
				width: 96,
				height: 96,
				frames: 6
			},
			{
				key: 'run',
				src: '/img/cp/run 1.png',
				type: 'sprite',
				width: 96,
				height: 96,
				frames: 4
			}
		]
	}

	let canvas: HTMLCanvasElement | null = null
	let context: CanvasRenderingContext2D | null = null

	interface Animation {
		image: HTMLImageElement
		frameCount: number
		frameWidth: number
		frameHeight: number
	}

	interface Hitbox {
		width: number
		height: number
		offsetX: number
		offsetY: number
		strokeColor: string
	}

	interface ObstacleConfig {
		width: number
		height: number
		y: number
		speed: number
		imageSrc: string
	}

	class Player {
		x: number
		y: number
		width: number
		height: number
		animations: { [key: string]: Animation }
		baseX: number
		baseY: number
		currentAnimation: string
		currentFrame: number
		frameDelay: number
		frameDelayCounter: number
		velocityY: number
		gravity: number
		jumpStrength: number
		isJumping: boolean
		isRunning: boolean
		hitbox: Hitbox

		constructor(
			x: number,
			y: number,
			width: number,
			height: number,
			animations: { [key: string]: Animation },
			baseX: number,
			baseY: number,
			hitbox: Hitbox
		) {
			this.x = x
			this.y = y
			this.width = width
			this.height = height
			this.animations = animations
			this.baseX = baseX
			this.baseY = baseY
			this.currentAnimation = 'idle'
			this.currentFrame = 0
			this.frameDelay = 10
			this.frameDelayCounter = 0
			this.velocityY = 0
			this.gravity = 0.2 // Reduziere die Gravitationskraft, um den Sprung zu verlangsamen
			this.jumpStrength = -12 // Reduziere die Sprungstärke, um den Sprung zu verlängern
			this.isJumping = false
			this.isRunning = false
			this.hitbox = hitbox
			this.hitboxBorder = 'red'
		}

		setAnimation(animation: string) {
			if (this.currentAnimation !== animation) {
				this.currentAnimation = animation
				this.currentFrame = 0
				this.frameDelayCounter = 0

				if (animation === 'jump') {
					this.velocityY = this.jumpStrength
					this.isJumping = true
				}
			}
		}

		update() {
			this.frameDelayCounter++
			if (this.frameDelayCounter >= this.frameDelay) {
				this.currentFrame =
					(this.currentFrame + 1) %
					this.animations[this.currentAnimation].frameCount
				this.frameDelayCounter = 0
			}

			this.y += this.velocityY
			this.velocityY += this.gravity

			if (this.y >= this.baseY) {
				this.y = this.baseY
				this.velocityY = 0
				this.isJumping = false
				if (this.currentAnimation === 'jump') {
					this.setAnimation('idle')
				}
			}

			this.hitbox.x = this.x + config.hitbox.offsetX
			this.hitbox.y = this.y + config.hitbox.offsetY
		}

		draw(ctx: CanvasRenderingContext2D) {
			const animation = this.animations[this.currentAnimation]
			const frameX = this.currentFrame * animation.frameWidth
			ctx.drawImage(
				animation.image,
				frameX,
				0,
				animation.frameWidth,
				animation.frameHeight,
				this.x,
				this.y,
				this.width,
				this.height
			)

			// Draw hitbox for debugging purposes
			ctx.strokeStyle = config.hitbox.strokeColor
			ctx.strokeRect(
				this.hitbox.x,
				this.hitbox.y,
				this.hitbox.width,
				this.hitbox.height
			)
		}

		checkCollision(obstacle: Obstacle): boolean {
			return (
				this.hitbox.x < obstacle.x + obstacle.width &&
				this.hitbox.x + this.hitbox.width > obstacle.x &&
				this.hitbox.y < obstacle.y + obstacle.height &&
				this.hitbox.y + this.hitbox.height > obstacle.y
			)
		}
	}

	class Obstacle {
		x: number
		y: number
		width: number
		height: number
		speed: number
		image: HTMLImageElement

		constructor(
			x: number,
			y: number,
			width: number,
			height: number,
			speed: number,
			image: HTMLImageElement
		) {
			this.x = x
			this.y = y
			this.width = width
			this.height = height
			this.speed = speed
			this.image = image
		}

		update() {
			this.x -= this.speed
		}

		draw(ctx: CanvasRenderingContext2D) {
			ctx.drawImage(this.image, this.x, this.y, this.width, this.height)
		}
	}

	class BackgroundLayer {
		image: HTMLImageElement
		speed: number
		runspeed: number
		x: number

		constructor(image: HTMLImageElement, speed: number, runspeed: number) {
			this.image = image
			this.speed = speed
			this.runspeed = runspeed
			this.x = 0
		}

		update(isRunning: boolean) {
			if (isRunning) {
				if (this.speed < 1) {
					this.x -= this.runspeed
				} else {
					this.x -= this.speed
				}

				if (this.x <= -this.image.width) {
					this.x = 0
				}
			} else if (this.speed < 1.5) {
				this.x -= this.speed
				if (this.x <= -this.image.width) {
					this.x = 0
				}
			}
		}

		draw(ctx: CanvasRenderingContext2D) {
			ctx.drawImage(this.image, this.x, 0)
			ctx.drawImage(this.image, this.x + this.image.width, 0)
		}
	}

	function initCanvas() {
		// canvas = document.getElementById('myCanvas') as HTMLCanvasElement
		if (!canvas) {
			console.error('Canvas-Element nicht gefunden')
			return
		}

		context = canvas.getContext('2d')
		if (!context) {
			console.error('2D-Kontext konnte nicht abgerufen werden')
			return
		}

		loadImagesAndStartGame()
	}

	function loadImage(src: string): Promise<HTMLImageElement> {
		return new Promise((resolve, reject) => {
			const img = new Image()
			img.onload = () => resolve(img)
			img.onerror = () =>
				reject(new Error(`Fehler beim Laden des Bildes: ${src}`))
			img.src = src
		})
	}

	async function loadImagesAndStartGame() {
		try {
			const [
				idleImage,
				runImage,
				jumpImage,
				cloudsImage,
				backyardImage,
				skylineImage,
				buildingsImage,
				...obstacleImages
			] = await Promise.all([
				loadImage('/img/cp/idle.png'),
				loadImage('/img/cp/run.png'),
				loadImage('/img/cp/jump.png'),
				loadImage('/img/cp/clouds.png'),
				loadImage('/img/cp/backyard.png'),
				loadImage('/img/cp/skyline.png'),
				loadImage('/img/cp/buildings.png'),
				...config.obstacles.map((obstacle) => loadImage(obstacle.imageSrc))
			])

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
			}

			const clouds = new BackgroundLayer(cloudsImage, 0.5, 1.2)
			const backyard = new BackgroundLayer(backyardImage, 0.6, 1.4)
			const skyline = new BackgroundLayer(skylineImage, 0.7, 1.6)
			const buildings = new BackgroundLayer(buildingsImage, 1.6, 1.8)

			const obstacles = config.obstacles.map((obstacleConfig, index) => {
				const { width, height, y, speed } = obstacleConfig
				return new Obstacle(
					800 + index * 300,
					y,
					width,
					height,
					speed,
					obstacleImages[index]
				)
			})

			startGame(animations, [clouds, backyard, skyline, buildings], obstacles)
		} catch (error) {
			console.error('Fehler beim Laden der Bilder:', error)
		}
	}

	function startGame(
		animations: { [key: string]: Animation },
		backgroundLayers: BackgroundLayer[],
		obstacles: Obstacle[]
	) {
		const player = new Player(
			config.baseX,
			config.baseY,
			config.punkW,
			config.punkH,
			animations,
			config.baseX,
			config.baseY,
			config.hitbox
		)
		let score = 0

		function gameLoop() {
			if (context && canvas) {
				context.clearRect(0, 0, canvas.width, canvas.height)

				for (const layer of backgroundLayers) {
					layer.update(player.isRunning)
					layer.draw(context)
				}

				for (const obstacle of obstacles) {
					obstacle.update()
					obstacle.draw(context)
					config.hitbox.strokeColor = 'red'

					if (player.checkCollision(obstacle)) {
						console.log('Kollision erkannt!')
						player.isRunning = false
						// return
					}
				}

				player.update()
				player.draw(context)

				// Score anzeigen
				context.fillStyle = 'black'
				context.font = '20px Arial'
				context.fillText(`Score: ${score}`, 10, 20)

				if (player.isRunning) {
					score++
				}
			}
			requestAnimationFrame(gameLoop)
		}

		document.addEventListener('keydown', (event) => {
			if (event.key === 'ArrowRight') {
				player.setAnimation('run')
				player.isRunning = true
			} else if (event.key === ' ') {
				if (!player.isJumping) {
					player.setAnimation('jump')
				}
			}
		})

		document.addEventListener('keyup', (event) => {
			if (event.key === 'ArrowRight') {
				player.setAnimation('idle')
				player.isRunning = false
			}
		})

		gameLoop()
	}

	onMount(() => {
		console.log('init CP!')
		initCanvas()
	})
</script>

<canvas bind:this={canvas} id="myCanvas" width="800" height="600"></canvas>

```

