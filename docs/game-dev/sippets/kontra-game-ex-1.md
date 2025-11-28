---
id: doc-kontra-game-ex1
title: Kontra Exampl 1 
sidebar_label: Kontra Exampl 1
sidebar_position: 999
---


# How to Write a Game in Under 13 Kb While Taking Care of a Baby

Wow! It’s been a while! The past year has been wonderful and tough in equal measure. Having and taking care of a baby as an  **equal partner**  is exhilarating, heart-warming and… extremely exhausting, and that’s why you haven’t heard much of me for the past year. Let this article and the  [js13kgames](http://www.js13kgames.com/)  competition be my comeback.

In the paragraphs below you’ll learn how it feels to develop a game in under 13Kb, how I approached the challenge, from planning, to developing the game mechanics, particle systems, generative algorithms, music, galaxies, fleets of mysterious aliens and how I arrived to something resembling a game:  **[Earth That Was](http://js13kgames.com/entries/earth-that-was)**.  _(enter video)_

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#how-about-writing-a-game-in-under-13-kb)How About Writing a Game in Under 13 Kb?

I don’t know how the heck I found out about  [js13kgames](http://wwww.js13kgames.com/). It somehow appeared on my twitter feed and I thought  _“Hmm… nifty…”_  as I scrolled down and onto the next bit of JavaScript news. It wasn’t until a couple of days later that the idea settled and fermented somewhere in the far reaches of my head and I decided,  _“What the heck? This will be an awesome way to rekindle my affair with coding and doing cool stuff outside of work”_.

So that’s how it started. The final push that won over this hesitating dad was following  [a great tutorial](https://medium.com/web-maker/making-asteroids-with-kontra-js-and-web-maker-95559d39b45f)  about building asteroids with  [kontra.js](https://straker.github.io/kontra/)  and realising how much fun it could be.

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#setting-goals-and-choosing-a-theme)Setting Goals And Choosing a Theme

So I was going to participate in the gamejam, but what would my game be about? What did I want to take out of this gamejam experience? For me the most important things were to  **learn about game development**,  **have fun**  and  **finish the game**. With that in mind I decided to simplify things as much as possible and continue with the space shooter theme from the tutorial. I’ve often found that in order to learn new things the best approach that you can follow is to break things down and reduce the axes of complexity you tackle at any given time.

In order to save the precious 13Kb I would use the rule of thumb of favoring procedural generation of maps, images, music, etc… over manual work and, due to my special time constraints, aim to get everything working in its simplest form from first principles (not much time to invest in research).

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#opportunistic-coding-or-how-to-code-when-theres-no-time-to-code)Opportunistic Coding, or how to code when there’s no time to code

The next hurdle was to find time to develop the game. As a dad with a teeny tiny infant, my time and attention belongs to him and only when he’s sleeping can I find the time and tranquility to do anything other than babying. Here’s a couple of tips applicable to dad’s and non-dad creatures alike:

1.  **Kill the multitasking**. Focus on one task at hand. One project at hand. One thing at a time. Continued iterative effort applied to a single problem bears amazing fruits.
2.  **Action begets motivation**. If you are tired and are not feeling it, open your computer and start coding. You’ll be surprised how often you’ll get in the mood and feel energized after just a couple of minutes of coding.

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#setting-up-a-gamedev-environment)Setting up a Gamedev Environment

For the dev environment I’d use something familiar to a web developer in this day and age,  [Webpack](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby),  [TypeScript](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby)  and  [Visual Studio Code](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby). Running something like this:

```shell
$ npm start
```

would setup my game dev environment with live reloads. And:

```shell
$ npm run build
```

would produce my production “binary” optimized for submission to the competition. This was a super convenient setup and  [TypeScript](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby)  helped me find and fix certain bugs faster.

In terms of optimizing JS to keep it under those 13 Kb, I tinkered for a while with  [tsickle](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby)  and the  [closure compiler](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby)  but I ended up using uglifyJS since it has a better integration with Webpack. (TBH I couldn’t make closure work in the little time that I had and UglifyJS was good enough).

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#writing-a-game)Writing a Game

Writing a game is a ton of fun. One of the things I love the most about programming is that it is an art of creation: There’s nothing; you write some code and  **BOOM!**  Out of nothingness comes stuff. Game development is specially strong in this regard because you have the ability to create worlds. Which is infinitely cool if you ask me. The domain model surely beats any application I’ve worked with before,  `SpaceShip`,  `Planet`,  `Bullet`,  `Elder`  win over  `PurchaseOrder`  any time of the day.

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#wouldnt-it-be-cool-if-game-design)Wouldn’t It Be Cool If? Game Design

Since my main goal with this game was to learn how to develop games I took a very open and exploratory approach: I call it  **wouldn’t-it-be-cool-if game design**. I knew that I wanted to make a space shooter because I perceived it as a simpler task than other types of games but I didn’t spend much more time planning the game. I just jumped on writing different isolated mechanics by asking myself:  **Wouldn’t it be cool if…**

-   these asteroids had nice textures?
-   they had different shapes and sizes?
-   they would drop resources to recharge/repair the ship when destroyed?
-   the ship propulsion emitted particles?
-   there were several factions with different ships and goals?
-   there were mysterious and incredibly dangerous aliens roaming around?
-   the different ship systems within the game would stop working when there wasn’t energy available?
-   you could claim planets?
-   you could own the economies of these planets and build defences, ships, etc?
-   you could have different weapon systems and ways to rain fire and destruction upon your enemies?
-   and on, and on, it goes…

Although a fun way to develop a game, it meant that by the last day of the competition I had a bunch of mostly isolated mechanics but not a game. There were ships, asteroids, planets, suns, sectors, galaxies, aliens but nothing to join them together into something resembling a game.

So during the last day I did a brainstorm session together with my son Teo (while he slept) and came up with an idea that could tie all these elements together within the span of a day:

**A ship hovers in orbit around a dying Earth, the last hope of humanity it contains the seeds for a new human civilization across the stars. The only thing missing a new earth capable of hosting the remainings of mankind. Earth that was. But that it can be again.**

So. Deep.

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#using-kontra)Using Kontra

[Kontra.js](https://straker.github.io/kontra/)  is a minimalistic 2D gaming library perfect for the js13k challenge. It gives you all the basics you need to develop a 2D game: a gaming loop to update the state of your game and render it in a canvas, a way to represent things (sprites) within your game such as ships, asteroids or bullets, a way to load assets and process input, tilemaps, spritesheets with animations, etc, etc. The nice thing is that it is modular and you can choose which parts you want to use saving those precious Kb for your game. The less nice thing (depending on your preferences and your dev environment) is that it doesn’t support ESM which would’ve come in handy for tree-shaking.

Kontra’s API is very fond of factory functions so I modelled all my domain objects using factory functions instead of classes since it felt more natural, symmetric and fit better. For instance, this is a bullet-missile-projectile thingie:

```typescript
export interface Bullet extends Sprite {
  damage: number
  owner: Sprite
  color: RGB
}

const numberOfParticles = 2

export default function createBullet(
  position: Position,
  velocity: Velocity,
  angle: number,
  cameraPosition: Position,
  scene: Scene,
  owner: Sprite,
  damage: number = 10,
  color: RGB = { r: 255, g: 255, b: 255 }
): Bullet {
  const cos = Math.cos(degreesToRadians(angle))
  const sin = Math.sin(degreesToRadians(angle))

  return kontra.sprite({
    type: SpriteType.Bullet,
    // start the bullet at the front of the ship
    x: position.x + cos * 12,
    y: position.y + sin * 12,
    // move the bullet slightly faster than the ship
    dx: velocity.dx + cos * 5,
    dy: velocity.dy + sin * 5,
    // damage can vary based on who shoots the missile
    damage,
    // avoid friendly fire
    owner,
    ttl: 50,
    width: 2,
    height: 2,
    color,
    update() {
      this.advance()
      this.addParticles()
    },
    addParticles() {
      let particles = callTimes(numberOfParticles, () =>
        Particle(
          { x: this.x, y: this.y },
          { dx: this.dx, dy: this.dy },
          cameraPosition,
          angle,
          { color }
        )
      )
      particles.forEach(p => scene.addSprite(p))
    },
    render() {
      let position = getCanvasPosition(this, cameraPosition)
      Draw.fillRect(
        this.context,
        position.x,
        position.y,
        this.width,
        this.height,
        Color.rgb(this.color)
      )

      if (Config.debug && Config.showPath) {
        this.context.save()
        this.context.translate(position.x, position.y)
        Draw.drawLine(this.context, 0, 0, this.dx, this.dy, 'red')
        this.context.restore()
      }

      if (Config.debug && Config.renderCollisionArea) {
        this.context.save()
        this.context.translate(position.x, position.y)
        Draw.drawCircle(this.context, 0, 0, this.width / 2, 'red')
        this.context.restore()
      }
    },
  })
}
```

In addition to these game objects which are just factories extending  `kontra.sprite({...})`  and represent any object visible and capable of interaction within the game, I created a couple abstractions more:  `Scene`  and the  `Game`  itself. The scenes were very helpful to represent different parts of the game and group game objects in a meaningful way (as in open scene, space scene, game over scene, etc…) while the game provided a way to centralize state management, control the game music, preload assets and provided a way to transition between scenes.

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#generative-programming)Generative Programming

I spent most of my time doing two things:

1.  Banging my head against basic newtonian physics and trygonometry,
2.  Devising simple algorithms to generate textures, particles, names and galaxies.

Let’s take a closer look at  **#2**  which will probably be more interesting to you. In general, when developing these algorithms I followed a couple of rules:

1.  Get something working as fast as you can and iterate
2.  Think first principles. How would you do this from scratch?

#### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#pixelated-textures)Pixelated Textures

For the textures of the planets I wanted to achieve a pixel-artsy feel which didn’t look like shit (so very low expectations :D). I started with three types of planets: Red, Green and Blue and the idea of generating full palettes from these individual colors.

Immediately, I thought about the  `HSL`  color model as an awesome canditate to generate these palettes.  `HSL`  stands for  `Hue`,  `Saturation`  and  `Lightness`  which is English for  _if I change the lightness up and down I get myself a palette_. And that’s what I did. My first algorithm used a single color and built a color palette with 2 darker shades and 2 lighter shades. These colors were later applied in different proportions to produce a pattern that was then used to fill the surface of a planet. I later experimented with different proportions in different parts of the pattern, transparency and having more colors within a palette.

The final algorithm used a base color and an accent color and looked like this:

```typescript
// A way to represent HSL colors
export interface HSL {
  h: number
  s: number
  l: number
}

// An offscreen canvas to create textures
// in the background
export class OffscreenCanvas {
  // more codes here...
  // but here's the interesting part

  private savedPatterns: Map<string, CanvasPattern> = new Map<
    string,
    CanvasPattern
  >()

  getPatternBasedOnColors(
    primary: HSL,
    secondary: HSL,
    width: number = 16,
    height: number = 16,
    pixelSize: number = 2
  ) {
    // memoize
    // TODO: extract to higher-order function
    if (
      this.savedPatterns.has(twocolorkey(primary, secondary, width, height))
    ) {
      return this.savedPatterns.get(
        twocolorkey(primary, secondary, width, height)
      )
    }

    this.canvas.width = width
    this.canvas.height = height

    // 1. define color theme
    let p = primary
    let s = secondary

    // Functions that return colors with different
    // alpha values. I ended up only using completely solid colors
    let baseColor = (a: number) => Color.hsla(p.h, p.s, p.l, a)
    let lightShade = (a: number) => Color.hsla(p.h, p.s, p.l + 10, a)
    let darkShade = (a: number) => Color.hsla(p.h, p.s, p.l - 10, a)
    let accent = (a: number) => Color.hsla(s.h, s.s, s.l, a)

    // This defines the color distribution
    // e.g. 40% base color, 20% lighter shade, 20% darker shade
    // and 20% accent color
    let buckets = [
      baseColor,
      baseColor,
      baseColor,
      baseColor,
      lightShade,
      lightShade,
      darkShade,
      darkShade,
      accent,
      accent,
    ]

    // 3. distribute randomly pixel by pixel see how it looks
    for (let x = 0; x < this.canvas.width; x += pixelSize) {
      for (let y = 0; y < this.canvas.height; y += pixelSize) {
        let pickedColor = pickColor(buckets)
        this.context.fillStyle = pickedColor
        this.context.fillRect(x, y, pixelSize, pixelSize)
      }
    }

    let pattern = this.context.createPattern(this.canvas, 'repeat')
    this.savedPatterns.set(
      twocolorkey(primary, secondary, width, height),
      pattern
    )
    return pattern
  }
}

function pickColor(buckets: any) {
  let index = Math.round(getValueInRange(0, 9))
  let alpha = 1
  return buckets[index](alpha)
}

function twocolorkey(
  primary: HSL,
  secondary: HSL,
  width: number,
  height: number
) {
  let key1 = key(primary.h, primary.s, primary.l, width, height)
  let key2 = key(secondary.h, secondary.s, secondary.l, width, height)
  return `${key1}//${key2}`
}
```

Since creating a pattern every time you need it is kind of expensive, I  _memoized_  every pattern created using the same colors and size. In layman terms  _memoizing_  means saving the results of a function call with some arguments so that I don’t need to process the same result again. In this case, it means saving textures once they are created and using them over and over again.

There’s a lot of room for improvement here, I would have enjoyed experimenting more and being able to generate land masses, cloud formations, etc. The result however was pretty good, I enjoyed the looked of my planets. :D

#### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#beautiful-stars)Beautiful Stars

When your game happens in space and everything is black it becomes hard for the player to see the effects of moving their ship around. So I wanted to create a starry background and achieve some sort of parallax effect that would give the player great cues about movement in space.

In order to do that I devised an algorithm that would take into account the following:

-   The background around the ship will always be covered in stars.
-   As the ship moves around we’ll move stars from  _behind the ship_  to  _in front of the ship_  creating the illusion that everything is covered in stars.
-   Stars will be at different distances from the ship. Some will be far, far away and others will be nearer
-   Far stars will look dimmer and smaller than nearer stars
-   As the ship moves, far stars move slower than nearer stars

The  `Star`  itself is a very simple game object:

```typescript
export interface StarBuilder extends SpriteBuilder {}
export interface Star extends Sprite {
  distance: number
  color: string
}

export function Star({ x, y, cameraPosition }: StarBuilder): Star {
  let distance: number = parseFloat(getValueInRange(0, 1).toFixed(2))
  let alpha: number = 1 - 3 * distance / 4
  let color: string = Color.get(alpha)
  let size: number = 2.5 + (1 - distance)

  return kontra.sprite({
    // create some variation in positioning
    x: getNumberWithVariance(x, x / 2),
    y: getNumberWithVariance(y, y / 2),
    type: SpriteType.Star,
    dx: 0,
    dy: 0,
    ttl: Infinity,
    distance,
    color,
    size,
    render() {
      // the more distant stars appear dimmer
      // limit alpha between 1 and 0.75
      // more distant stars are less affected by the camera position
      // that is, they move slower in reaction to the camera changing
      // this should work as a parallax effect of sorts.
      let position = getCanvasPosition(this, cameraPosition, this.distance)
      this.context.fillStyle = this.color
      this.context.fillRect(position.x, position.y, this.size, this.size)
    },
  })
}

export function getNumberWithVariance(n: number, variance: number): number {
  return n + Math.random() * variance
}
```

The meat is in the function that calculates the position of a game object in a canvas  `getCanvasPosition`  and takes into account the camera position and the effect of the distance as the camera changes:

```typescript
// Get position of an object within the canvas by taking into account
// the position of the camera
export function getCanvasPosition(
  objectPosition: Position,
  cameraPosition: Position,
  distance: number = 0
): Position {
  // distance affects how distant objects react to the camera changing
  // distant objects move slower that close ones (something like parallax)
  // that is, moving the ship will have less effect on distant objects
  // than near ones

  // distance is a value between 0 (close) and 1 (far)
  // at most the deviation factor will be 0.8
  let deviationFactor = 1 - distance * 0.2

  // include canvasSize / 2 because the camera is always pointing
  // at the middle of the canvas
  let canvasPosition: Position = {
    x:
      objectPosition.x -
      (cameraPosition.x * deviationFactor - Config.canvasWidth / 2),
    y:
      objectPosition.y -
      (cameraPosition.y * deviationFactor - Config.canvasHeight / 2),
  }

  return canvasPosition
}
```

#### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#names)Names

My initial idea was to have an infinite galaxy to explore and naming each star system, star and planet manually just wouldn’t work that way. I only have imagination for between 5 to 7 names. Tops. So I wrote a name generator based on the following principles:

-   Generate syllables of 1 to 3 letters.
-   1 letter syllables will be vocals
-   2 and 3 letters syllables will start with a consonant
-   Put 2 to 4 syllables together to form a word

My hope was that connecting syllables instead of random characters would result in more discernible and believable names and I think that I did achieve that. The algorithm looked like this:

```typescript
export function generateName() {
  let numberOfSyllabes = getIntegerInRange(2, 4)
  let name = ''
  for (let i = 0; i < numberOfSyllabes; i++) {
    name += `${generateSyllable()}`
  }
  return name
}

let vocals = ['a', 'e', 'i', 'o', 'u', 'ä', 'ö', 'å']
let minCharCode = 97 // a
let maxCharCode = 122 // z

function generateSyllable() {
  let syllableSize = getIntegerInRange(1, 3)
  if (syllableSize === 1) return getVocal()
  else if (syllableSize === 2) return `${getConsonant()}${getVocal()}`
  else return `${getConsonant()}${getVocal()}${getConsonant()}`
}

function getVocal() {
  return getRandomValueOf(vocals)
}
function getConsonant() {
  let consonant = ''
  while (!consonant) {
    let code = getIntegerInRange(minCharCode, maxCharCode)
    let letter = String.fromCharCode(code)
    if (!vocals.includes(letter)) consonant = letter
  }
  return consonant
}
```

#### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#particles)Particles

I love particles! I think that they add a  _je ne sais quoi_  that makes a game look and feel much better. When I went about writing the particle engine (although  _engine_  is way too ambitious a word for a couple of functions) I asked myself  **What are particles?**  Which resulted in a very interesting conversation with myself about the answer to the Ultimate Question of Life, the Universe, and Everything. I won’t bother you with the details though… In the end it boiled down to: Particles are small sprites sprouting from a source at different directions, speed and acceleration which fade over time and disappear. So my particle engine would need to:

-   Create particles that would sprout from an origin point
-   With a given direction and speed (I didn’t consider acceleration, I bet that would’ve been something awesome to tinker with)
-   The particles would have different time to live
-   The particles would fade and become smaller over time and disappear
-   The particles would have different colors which you’d be able to configure

And that was pretty much it. This is an example of the particles used for the bullets which ended up looking like the tail of a comet:

```typescript
export interface Particle extends Sprite {}
export interface ParticleOptions {
  ttl?: number
  color?: RGB
  magnitude?: number
}

// particle that takes into account camera position
export function Particle(
  position: Position,
  velocity: Velocity,
  cameraPosition: Position,
  // angle for the particles
  particleAxis: number,
  { ttl = 30, color = { r: 255, g: 255, b: 255 } }: ParticleOptions = {}
): Particle {
  let ParticleAxisVariance = getValueInRange(-5, 5)

  let cos = Math.cos(degreesToRadians(particleAxis + ParticleAxisVariance))
  let sin = Math.sin(degreesToRadians(particleAxis + ParticleAxisVariance))

  return kontra.sprite({
    type: SpriteType.Particle,

    // particles originate from a single point
    x: position.x,
    y: position.y,

    // variance so that different particles will have
    // slightly different trajectories
    dx: velocity.dx - cos * 4,
    dy: velocity.dy - sin * 4,

    // each particle with have a slightly
    // different lifespan
    ttl: getValueInRange(20, ttl),
    dt: 0,

    width: 2,
    update() {
      this.dt += 1 / 60
      this.advance()
    },
    render() {
      let position = getCanvasPosition(this, cameraPosition)
      // as time passes the alpha increases until particles disappear
      let frames = this.dt * 60
      let alpha = 1 - frames / ttl
      let size = (1 + 0.5 * frames / ttl) * this.width
      this.context.fillStyle = Color.rgba(color.r, color.g, color.b, alpha)
      this.context.fillRect(position.x, position.y, size, size)
    },
  })
}
```

#### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#galaxies)Galaxies

As I said a couple of sections ago my initial idea was to generate an seemingly infinite galaxy for the player to explore. I though that, if I made the game difficult and challenging enough, the player would die before getting bored of exploring space. I would’ve loved to explore the idea of generating the galaxy as the player explored it but in the end and as the deadline approached I went for a v0 version in which I just created a 10x10 sector galaxy. So:

-   The galaxy is 10x10 sectors
-   A sector is basically a star system with a central star and from 1 to 5 planets orbiting it (aside from our star system which has all the planets you would expect. Sorry Pluto, no dwarf planets).
-   The sectors would occupy a 10000x10000 pixel surface making the explorable galaxy a 100Kx100K space.
-   The player would start the game orbiting around the earth, in the solar system conveniently placed in the middle of the galaxy.

This is some sample code for the oh so mighty sectors:

```typescript
export interface Sector extends Position {
  name: string
  planets: Planet[]
  sun: Sun
  bodies: Sprite[]

  asteroids?: Asteroid[]
}

export function Sector(
  scene: Scene,
  position: Position,
  cameraPosition: Position,
  name = generateName()
): Sector {
  // HAXOR
  let isSunSystem = name === 'sun'
  let isOrion = name === 'orion'

  let sun = createSectorSun(position, cameraPosition, name)
  let planets = createPlanets(sun, scene, cameraPosition, {
    isSunSystem,
    isOrion,
  })
  return {
    // this position represents the
    // top-left corner of the sector
    x: position.x,
    y: position.y,
    name,

    sun,
    planets,

    bodies: [sun, ...planets],
  }
}

function createSectorSun(
  sectorPosition: Position,
  cameraPosition: Position,
  name: string
) {
  let centerOfTheSector = {
    x: sectorPosition.x + SectorSize / 2,
    y: sectorPosition.y + SectorSize / 2,
  }
  let sunSize = getValueInRange(125, 175)
  let sun = createSun({ ...centerOfTheSector }, sunSize, cameraPosition, name)
  return sun
}

function createPlanets(
  sun: Sun,
  scene: Scene,
  cameraPosition: Position,
  { isSunSystem = false, isOrion = false }
) {
  if (isSunSystem) return createSunSystemPlanets(sun, scene, cameraPosition)
  if (isOrion) return createOrionSystemPlanets(sun, scene, cameraPosition)

  let numberOfPlanets = getIntegerInRange(1, 5)
  let planets = []
  let planetPosition: Position = { x: sun.x, y: sun.y }
  for (let i = 0; i < numberOfPlanets; i++) {
    let additiveOrbit = getValueInRange(500, 1000)
    planetPosition.x = planetPosition.x + additiveOrbit
    let radius = getValueInRange(50, 100)
    let planet = createPlanet(
      sun,
      /* orbit */ planetPosition.x - sun.x,
      radius,
      cameraPosition,
      scene
    )
    planets.push(planet)
  }
  return planets
}

interface PlanetData {
  orbit: number
  radius: number
  name: string
  type: PlanetType
  angle?: number
  claimedBy?: Faction
}
function createSunSystemPlanets(
  sun: Sun,
  scene: Scene,
  cameraPosition: Position
) {
  let planets: PlanetData[] = [
    { orbit: 300, radius: 30, name: 'mercury', type: PlanetType.Barren },
    { orbit: 500, radius: 70, name: 'venus', type: PlanetType.Desert },
    {
      orbit: 700,
      radius: 50,
      name: '*earth*',
      type: PlanetType.Paradise,
      angle: 40,
      claimedBy: Faction.Blue,
    },
    { orbit: 900, radius: 40, name: 'mars', type: PlanetType.Red },
    { orbit: 1500, radius: 150, name: 'jupiter', type: PlanetType.GasGiant },
    { orbit: 2100, radius: 130, name: 'saturn', type: PlanetType.GasGiant },
    { orbit: 2700, radius: 110, name: 'uranus', type: PlanetType.Blue },
    { orbit: 3500, radius: 110, name: 'neptune', type: PlanetType.Blue },
  ]
  return planets.map(p =>
    createPlanet(sun, p.orbit, p.radius, cameraPosition, scene, {
      name: p.name,
      type: p.type,
      startingAngle: p.angle,
      claimedBy: p.claimedBy,
    })
  )
}

function createOrionSystemPlanets(
  sun: Sun,
  scene: Scene,
  cameraPosition: Position
) {
  return [
    createPlanet(sun, 700, 100, cameraPosition, scene, {
      name: 'orion',
      type: PlanetType.Paradise,
    }),
  ]
}
```

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#the-ancient-elder-race)The Ancient Elder Race

I wanted to add a little spice to the game, something like a chilli or a spicey pepper, to make it more challenging and fun. Since I didn’t have a ton of time to think and develop a deep lore for the game, I went for a scifi and fantasy trope,  **The Elder Race**.

I wanted to have at least three different types of enemies the player would have to contend with:

-   A super quick, short-range, weak yet aggresive flying vessel: The  **drone**
-   A mid-size unit, quite sturdy that would patrol around planets and stars: The  **sentry**
-   A huge, strong and powerful battleship that would be rarely seen and which would be able to transport and spout drones at will: The  **mothership**.

The idea would be to have these populating different star systems in diverse measures and have a central system where they would reside and have the mother of all fleets. At the beginning of the game I wasn’t quite sure as to what the role or end goal of this elder race would be but later I settled into them being the guardians of the last planet amenable to human life and therefore final boss of the game.

When I was implementing these elder ships I wanted to develop a system where I could define… let’s call them… AI behaviors (again  **AI**  is too ambitous a word for very basic algorithms) and then compose them together at will. So we could have something like  _Follow this target_, or  _Shoot at it_, or  _Patrol this area_, or  _Follow this course when you don’t have anything else to do_.

The system consisted on a series of  [Mixins](https://www.barbarianmeetscoding.com/blog/2015/12/28/black-tower-summoning-object-composition-with-mixins/)  which exposed the following interface:

```typescript
export interface Behavior {
  type: BehaviorType
  properties: BehaviorProperties
  update(dt?: number): void
  render?(): void
}

export interface BehaviorProperties {
  // any property
  [key: string]: any
}
```

This interface consists in a bunch of arbitrary properties  `BehaviorProperties`  which the behavior itself needed in order to function, and an  `update`  and  `render`  methods to hook into the natural  `Sprite`  lifecycle.

An example of behavior is this  `Shoot`  which implements that interface by making the game object shoot at a target when the target is near (`< 300`):

```typescript
export function Shoot(scene: Scene, target: Position): Behavior {
  return {
    type: BehaviorType.Shoot,
    properties: {
      dts: 0,
      damage: 1,
      color: { r: 255, g: 255, b: 255 },
    },
    update(dt?: number) {
      this.dts += 1 / 60
      let distanceToShip = Vector.getDistanceMagnitude(this, target)
      if (this.dts > 0.5 && distanceToShip < 300) {
        this.dts = 0
        let angle = radiansToDegrees(Math.atan2(this.dy, this.dx))
        let bullet = createBullet(
          this,
          this,
          angle,
          target,
          scene,
          /*owner*/ this,
          this.damage,
          this.color
        )
        scene.addSprite(bullet)
      }
    },
  }
}
```

The way that I would compose this with a normal  `Sprite`  would be using this  `composeBehavior`  function:

```typescript
export function composeBehavior(sprite: Sprite, behavior: Behavior) {
  // only add properties if they're not already there
  Object.keys(behavior.properties).forEach(k => {
    if (sprite[k] === undefined) {
      sprite[k] = behavior.properties[k]
    }
  })

  sprite.update = before(sprite.update, behavior.update).bind(sprite)
  if (behavior.render) {
    sprite.render = after(sprite.render, behavior.render).bind(sprite)
  }
}
```

where  `before`  and  `after`  are utility functions:

```typescript
/* Call a function before another function */
export function before(func: any, beforeFunc: any) {
  return function(...args: any[]) {
    beforeFunc.apply(this, args)
    func.apply(this, args)
  }
}

/* Call a function after another function */
export function after(func: any, ...afterFuncs: any[]) {
  return function(...args: any[]) {
    func.apply(this, args)
    afterFuncs.forEach((f: any) => f.apply(this, args))
  }
}
```

So, taking advantage of this behavior composition I could define a collection of behaviors and  _attach_  them to different elder ships like this:

```typescript
// some code...
if (this.elderType === ElderType.Sentry) {
  // patrol around target following an orbit of 200
  // (it'll be a planet setup later on)
  composeBehavior(elder, PatrolAroundTarget(PatrolType.Orbit, /* orbit */ 200))

  // if the player's ship comes near (<300) follow it steady
  composeBehavior(elder, FollowSteadyBehavior(this.ship, 300))

  // if the player's ship is near (<300) shoot at it
  composeBehavior(elder, Shoot(scene, this.ship))
}
// more code...
```

This is nice because it saves Kb’s and it allows me to configure and attach behaviors at will, to elders and, in the future, perhaps other AI-controlled factions.

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#pixel-art)Pixel Art

I love pixel art but I’m just a complete amateur pixel artists. For this game I wanted to at least have a hand-crafted cool looking spaceship. In order to get a nice pixely look I went for 32x32 sprites with 2x2 pixels and a limited color palette. I used  [Piskel](https://www.piskelapp.com/)  which is a  **very**  nice web based app for creating pixel art. Below you can see some examples of the different ships I made and the Piskel editor itself:

![Different versions of pixel art for the red ship](https://www.barbarianmeetscoding.com/images/js13k-games-pixelart-redship-versions.jpg "Different Versions of the Red Ship")![Red ship in Piskel Editor](https://www.barbarianmeetscoding.com/images/js13k-games-pixelart-redship-piskel-editor.jpg "Red ship in Piskel Editor")![Green ship in Piskel Editor](https://www.barbarianmeetscoding.com/images/js13k-games-pixelart-greenship-piskel-editor.jpg "Green ship in Piskel Editor")![Blue ship in Piskel Editor](https://www.barbarianmeetscoding.com/images/js13k-games-pixelart-blueship-piskel-editor.jpg "Blue ship in Piskel Editor")

### [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#music)Music

Music is a super important ingredient in a game. It helps you make your game more immersive, provides feedback to the player, sets the right atmosphere and triggers emotions (excitement, fear, tension, calmness, etc…). With the 13Kb limitation I immediately thought about generative music (which I’ve been hearing a ton about in my twitter feed) and using the  [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API). I hit two roadblocks:

-   I have not the slightest idea about music, in any way, shape or form
-   I had no idea about the workings of the Web Audio API

Where in other parts of the game I had been content with just thinking and solving problems in isolation from first principles. However, when it came to music I  **really**  needed to research, read and learn from others. This is a small list of articles that I found invaluable when adding music to the game:

-   [These series about web audio](https://teropa.info/blog/2016/08/19/what-is-the-web-audio-api.html)  from  [@teropa](https://twitter.com/teropa)  are INSANEly awesome. They were a great help to understand the workings of the Web Audio API and how to take advantage of it to make music.
-   Also awesome are  [his experiments](https://teropa.info/blog/2017/01/23/terry-rileys-in-c.html)  (and  [more experiments](https://teropa.info/blog/2016/07/28/javascript-systems-music.html)) with generative music. Although they were way too advanced for me while developing this game, they may come in handy in the months to come and perhaps I’ll be able to absorb that knowledge for future game jams.
-   This tutorial on  [Procedural Music Generation](http://www.procjam.com/tutorials/en/music/)  by  [@mcfunkypants](https://twitter.com/McFunkypants)  for  [procjam](http://www.procjam.com/)  was also super good and gave me lots of ideas.
-   Finally reading about  [@kevincennis](https://twitter.com/kevincennis)’s  [journey to implement TinyMusic](https://medium.com/@kevincennis/how-i-built-tinymusic-js-part-1-5a298d5d79ef)  and looking at the  [source code](https://github.com/kevincennis/TinyMusic)  was a great learning experience that taught me about how to create sequences of notes with the Web Audio API.

In the end, I wrote a small music engine drawing a lot of inspiration from  [TinyMusic](https://medium.com/@kevincennis/how-i-built-tinymusic-js-part-1-5a298d5d79ef)  and  [@teropa’s articles on web audio](http://teropa.info/blog/2016/09/20/additive-synthesis.html). Unfortunately, I had to strip it out of the game during the final 13k-witch-hunting hours just before I submitted it to the contest. The only thing that I kept was a  [beating effect](http://teropa.info/blog/2016/09/20/additive-synthesis.html#beating)  I felt matched the feeling of the game. If you’re unfamiliar with the term  _beating_  as was I just a week ago, it consists in mixing waves of very close frequencies which reinforce each other when in-phase and cancel each other when they’re out of phase producing ever-changing quasi-musical notes.

```typescript
function Oscillator(ac: AudioContext, freq = 0) {
  let osc = ac.createOscillator()
  osc.frequency.value = freq
  return osc
}

function Gain(ac: AudioContext, gainValue: number) {
  let gain = ac.createGain()
  gain.gain.value = gainValue
  return gain
}

interface Connectable {
  connect(n: AudioNode): void
}
function Beating(
  ac: AudioContext,
  freq1: number,
  freq2: number,
  gainValue: number
) {
  let osc1 = Oscillator(ac, freq1)
  let osc2 = Oscillator(ac, freq2)
  let gain = Gain(ac, gainValue)
  osc1.connect(gain)
  osc2.connect(gain)
  return {
    connect(n: AudioNode) {
      gain.connect(n)
    },
    start(when = 0) {
      osc1.start(when)
      osc2.start(when)
    },
    stop(when = 0) {
      osc1.stop(when)
      osc2.stop(when)
    },
  }
}

function Connect({ to }: { to: AudioNode }, ...nodes: Connectable[]) {
  nodes.forEach(n => n.connect(to))
}

interface MusicTrack {
  start(): void
  stop(): void
}

function GameOpeningMusic(ac: AudioContext): MusicTrack {
  let b1 = Beating(ac, 330, 330.2, 0.5)
  let b2 = Beating(ac, 440, 440.33, 0.5)
  let b3 = Beating(ac, 587, 587.25, 0.5)
  let masterGain = Gain(ac, 0.1)

  Connect({ to: masterGain }, b1, b2, b3)
  masterGain.connect(ac.destination)

  return {
    start() {
      b1.start()
      b2.start()
      b3.start()
    },
    stop() {
      b1.stop()
      b2.stop()
      b3.stop()
    },
  }
}

export interface GameMusic {
  play(track: Track): void
  stop(): void
  currentTrack: MusicTrack
}

export function GameMusic(): GameMusic {
  let ac = new AudioContext()

  return {
    currentTrack: undefined,
    play(track: Track) {
      if (this.currentTrack) {
        this.currentTrack.stop()
      }
      let musicTrack = Tracks[track]
      this.currentTrack = musicTrack(ac)
      this.currentTrack.start()
    },
    stop() {
      this.currentTrack.stop()
    },
  }
}
```

## [](https://www.barbarianmeetscoding.com/blog/how-to-write-a-game-under-13k-while-taking-care-of-a-baby#conclusion)Conclusion

**THIS WAS SO MUCH FUN!!!**  If you haven’t joined a game jam before I thoroughly recommend it. I don’t know if all game jams are like  [js13k](http://wwww.js13kgames.com/). But the fact that this one was over the space of a whole month and I could just find time here and there without feeling super rushed was great. Also using JavaScript and open web technologies makes it that much easier to get started. You just need an editor and a browser and you’re good to go (or you can even use a browser based editor :D).

I also learnt a ton about game development and the web audio API. I have a ton of different little threads that I’d love to follow and experience many other aspects of game development, generative programming, music and pixel art.

All in all I feel like I fulfilled my goals for this competition. If I could change one thing I would like to have spent a little bit more time planning and having a more clear goal as to were I wanted to go. That would have helped me focus my efforts and have a more polished game to submit in the end.

Over the next weeks  [I’ll keep updating the game](https://www.barbarianmeetscoding.com/js13k-spaceshooter/)  and polishing it to a level that I’m happy with. I think it’ll be the perfect playground to test new game mechanics and polish those generative algorithms.

And you! Take care and consider joining a game jam! :D

P.S. You can play the original game  [here](http://js13kgames.com/entries/earth-that-was)! Give it a try and let me know what you think! :D

[Spread The Word. Share this article!](https://twitter.com/intent/tweet?hashtags=gamedev,javascript&original_referer=undefined&text=I%20really%20enjoyed%20this%20article%20on%20Barbarian%20Meets%20Coding:%20How%20to%20Write%20a%20Game%20in%20Under%2013%20Kb%20While%20Taking%20Care%20of%20a%20Baby&url=undefined&via=vintharas)

