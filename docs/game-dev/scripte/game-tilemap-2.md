---
sidebar_label: Tilemap 2
sidebar_position: 102
---




# How to Create A 2D Tile-Based Game with JavaScript - Orangeable

> ## Excerpt
> Learn to build a 2D tile-based game with JavaScript using no third-party libraries. Great for beginner to intermediate developers!

---
# How to Create A 2D Tile-Based Game with JavaScript

[![JavaScript icon](https://orangeable.com/assets/img/topic/javascript.webp)JavaScript](https://orangeable.com/javascript)

In this tutorial, you'll learn how to create a 2D tile-based game with JavaScript! We'll walk through creating the player, a map, some keyboard controls, and some mechanics to control the viewport for a smooth gaming experience. The great thing about this is there are no third-party plugins or libraries. Just pure, vanilla JavaScript and a sprinkling of HTML and CSS.

Before we dive in, you can check out the [game demo](https://orangeable.com/demo/javascript-2d-tile-based-game/) here, or just dive straight in and gain access to the full code base in the [GitHub repository](https://github.com/orangeable/javascript-2d-tile-based-game).

The game we'll be making looks like this:


Advertising Disclosure: I am compensated for purchases made through affiliate links. [Click here for details.](https://orangeable.com/affiliate)

## The HTML

We have just a bit of HTML code to display the necessary elements. The `canvas` element contains our map, and the character display and animation, a `div` container showing our FPS (frames per second) and player position on the map. We top it off with our JavaScript dependency files that we'll walk through shortly:

```
<span class="hljs-meta" role="none" tabindex="0">&lt;!DOCTYPE html&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">html</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">head</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">title</span>&gt;</span>2D Game<span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">title</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">link</span> <span class="hljs-attr" role="none" tabindex="0">type</span>=<span class="hljs-string" role="none" tabindex="0">"text/css"</span> <span class="hljs-attr" role="none" tabindex="0">href</span>=<span class="hljs-string" role="none" tabindex="0">"assets/css/main.css"</span> <span class="hljs-attr" role="none" tabindex="0">rel</span>=<span class="hljs-string" role="none" tabindex="0">"stylesheet"</span> /&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">head</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">body</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">canvas</span> <span class="hljs-attr" role="none" tabindex="0">id</span>=<span class="hljs-string" role="none" tabindex="0">"game"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">canvas</span>&gt;</span><br><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">div</span> <span class="hljs-attr" role="none" tabindex="0">id</span>=<span class="hljs-string" role="none" tabindex="0">"log"</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">div</span> <span class="hljs-attr" role="none" tabindex="0">id</span>=<span class="hljs-string" role="none" tabindex="0">"fps"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">div</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">div</span> <span class="hljs-attr" role="none" tabindex="0">id</span>=<span class="hljs-string" role="none" tabindex="0">"coords"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">div</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">div</span>&gt;</span><br> <br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">script</span> <span class="hljs-attr" role="none" tabindex="0">src</span>=<span class="hljs-string" role="none" tabindex="0">"assets/js/viewport.js"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">script</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">script</span> <span class="hljs-attr" role="none" tabindex="0">src</span>=<span class="hljs-string" role="none" tabindex="0">"assets/js/player.js"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">script</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">script</span> <span class="hljs-attr" role="none" tabindex="0">src</span>=<span class="hljs-string" role="none" tabindex="0">"assets/js/map.js"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">script</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;<span class="hljs-name" role="none" tabindex="0">script</span> <span class="hljs-attr" role="none" tabindex="0">src</span>=<span class="hljs-string" role="none" tabindex="0">"assets/js/main.js"</span>&gt;</span><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">script</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">body</span>&gt;</span><br><span class="hljs-tag" role="none" tabindex="0">&lt;/<span class="hljs-name" role="none" tabindex="0">html</span>&gt;</span>
```

## The CSS

Now, we'll create some styling for the page background and remove any unnecessary padding and margins from the `html` and `body` selectors. This will ensure that the game extends the entire browser viewport:

```
<span class="hljs-selector-tag" role="none" tabindex="0">html</span>, <span class="hljs-selector-tag" role="none" tabindex="0">body</span> {<br><span class="hljs-attribute" role="none" tabindex="0">width</span>: <span class="hljs-number" role="none" tabindex="0">100%</span>;<br><span class="hljs-attribute" role="none" tabindex="0">height</span>: <span class="hljs-number" role="none" tabindex="0">100%</span>;<br><span class="hljs-attribute" role="none" tabindex="0">background-color</span>: black;<br><span class="hljs-attribute" role="none" tabindex="0">padding</span>: <span class="hljs-number" role="none" tabindex="0">0px</span>;<br><span class="hljs-attribute" role="none" tabindex="0">margin</span>: <span class="hljs-number" role="none" tabindex="0">0px</span>;<br>}
```

Next, we'll style our `canvas` element to be fixed-positioned and stretch the entirety of the viewport horizontally and vertically:

```
<span class="hljs-selector-tag" role="none" tabindex="0">canvas</span><span class="hljs-selector-id" role="none" tabindex="0">#game</span> {<br><span class="hljs-attribute" role="none" tabindex="0">width</span>: <span class="hljs-number" role="none" tabindex="0">100vw</span>;<br><span class="hljs-attribute" role="none" tabindex="0">height</span>: <span class="hljs-number" role="none" tabindex="0">100vh</span>;<br><span class="hljs-attribute" role="none" tabindex="0">position</span>: fixed;<br>}
```

Finally, we'll create a `div` container to show the FPS (frames per second) count and horizontal and vertical tile position of the player on the map:

```
<span class="hljs-selector-tag" role="none" tabindex="0">div</span><span class="hljs-selector-id" role="none" tabindex="0">#log</span> {<br><span class="hljs-attribute" role="none" tabindex="0">background</span>: <span class="hljs-built_in" role="none" tabindex="0">rgba</span>(0, 0, 0, 0.90);<br><span class="hljs-attribute" role="none" tabindex="0">font-family</span>: Arial, sans-serif;<br><span class="hljs-attribute" role="none" tabindex="0">font-size</span>: <span class="hljs-number" role="none" tabindex="0">12pt</span>;<br><span class="hljs-attribute" role="none" tabindex="0">color</span>: white;<br><span class="hljs-attribute" role="none" tabindex="0">text-align</span>: left;<br><span class="hljs-attribute" role="none" tabindex="0">padding</span>: <span class="hljs-number" role="none" tabindex="0">5px</span> <span class="hljs-number" role="none" tabindex="0">9px</span>;<br><span class="hljs-attribute" role="none" tabindex="0">position</span>: fixed;<br><span class="hljs-attribute" role="none" tabindex="0">top</span>: <span class="hljs-number" role="none" tabindex="0">20px</span>;<br><span class="hljs-attribute" role="none" tabindex="0">left</span>: <span class="hljs-number" role="none" tabindex="0">20px</span>;<br><span class="hljs-attribute" role="none" tabindex="0">z-index</span>: <span class="hljs-number" role="none" tabindex="0">2</span>;<br>}
```

## The JavaScript

Now it's time for the fun part! Let's dive into each code block and dissect it carefully to understand how everything works.

## Main.js

This script contains the necessary functions to run the game, including global variables and the game loop.

### Game Configuration

Let's set up our game configuration variables in an object called `config`:

```
<span class="hljs-keyword" role="none" tabindex="0">var</span> config = {<br><span class="hljs-attr" role="none" tabindex="0">win</span>: {<br><span class="hljs-attr" role="none" tabindex="0">width</span>: <span class="hljs-built_in" role="none" tabindex="0">window</span>.innerWidth,<br><span class="hljs-attr" role="none" tabindex="0">height</span>: <span class="hljs-built_in" role="none" tabindex="0">window</span>.innerHeight<br>},<br><span class="hljs-attr" role="none" tabindex="0">tiles</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(<span class="hljs-built_in" role="none" tabindex="0">window</span>.innerWidth / <span class="hljs-number" role="none" tabindex="0">64</span>) + <span class="hljs-number" role="none" tabindex="0">2</span>,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(<span class="hljs-built_in" role="none" tabindex="0">window</span>.innerHeight / <span class="hljs-number" role="none" tabindex="0">64</span>) + <span class="hljs-number" role="none" tabindex="0">2</span><br>},<br><span class="hljs-attr" role="none" tabindex="0">center</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(<span class="hljs-built_in" role="none" tabindex="0">window</span>.innerWidth / <span class="hljs-number" role="none" tabindex="0">64</span>) / <span class="hljs-number" role="none" tabindex="0">2</span>,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(<span class="hljs-built_in" role="none" tabindex="0">window</span>.innerHeight / <span class="hljs-number" role="none" tabindex="0">64</span>) / <span class="hljs-number" role="none" tabindex="0">2</span><br>},<br><span class="hljs-attr" role="none" tabindex="0">size</span>: {<br><span class="hljs-attr" role="none" tabindex="0">tile</span>: <span class="hljs-number" role="none" tabindex="0">64</span>,<br><span class="hljs-attr" role="none" tabindex="0">char</span>: <span class="hljs-number" role="none" tabindex="0">96</span><br>},<br><span class="hljs-attr" role="none" tabindex="0">speed</span>: <span class="hljs-number" role="none" tabindex="0">5</span><br>};
```

There are several objects and variables within the `config` object:

-   `win`: the width and height of the browser's viewport.
-   `tiles`: the number of 64x64 pixel tiles that can fit within the viewport, with an offset of _2_ to ensure there is overflow and no blank space.
-   `center`: the horizontal and vertical center of the viewport in pixels.
-   `size`: the horizontal and vertical size of the map and character tiles in pixels.
-   `speed`: the walking speed of the player.

### Keyboard Input

Now, we'll define the arrow keys used to control the player on the map:

```
<span class="hljs-keyword" role="none" tabindex="0">var</span> keys = {<br><span class="hljs-number" role="none" tabindex="0">37</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: -config.speed,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">a</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">f</span>: [<span class="hljs-number" role="none" tabindex="0">6</span>, <span class="hljs-number" role="none" tabindex="0">7</span>, <span class="hljs-number" role="none" tabindex="0">8</span>, <span class="hljs-number" role="none" tabindex="0">7</span>]<br>},<br><span class="hljs-number" role="none" tabindex="0">38</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: -config.speed,<br><span class="hljs-attr" role="none" tabindex="0">a</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">f</span>: [<span class="hljs-number" role="none" tabindex="0">3</span>, <span class="hljs-number" role="none" tabindex="0">4</span>, <span class="hljs-number" role="none" tabindex="0">5</span>, <span class="hljs-number" role="none" tabindex="0">4</span>]<br>},<br><span class="hljs-number" role="none" tabindex="0">39</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: config.speed,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">a</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">f</span>: [<span class="hljs-number" role="none" tabindex="0">9</span>, <span class="hljs-number" role="none" tabindex="0">10</span>, <span class="hljs-number" role="none" tabindex="0">11</span>, <span class="hljs-number" role="none" tabindex="0">10</span>]<br>},<br><span class="hljs-number" role="none" tabindex="0">40</span>: {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: config.speed,<br><span class="hljs-attr" role="none" tabindex="0">a</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">f</span>: [<span class="hljs-number" role="none" tabindex="0">0</span>, <span class="hljs-number" role="none" tabindex="0">1</span>, <span class="hljs-number" role="none" tabindex="0">2</span>, <span class="hljs-number" role="none" tabindex="0">1</span>]<br>}<br>};
```

The key codes are:

-   `37`: left arrow key
-   `38`: up arrow key
-   `39`: right arrow key
-   `40`: down arrow key

And each of the variables within each arrow key object:

-   `x`: the distance, in pixels, the player will move on the x-axis.
-   `y`: the distance, in pixels, the player will move on the y-axis.
-   `a`: determines if the current key is pressed.
-   `f`: the player animation frames to use from the sprite image. As the player moves, the frame used from the sprite image changes regularly until the arrow keys are released.

Here is the sprite image for the player depicting each animation frame available:

![2D game player sprite](https://orangeable.com/media/post/2d-game-player-sprite.webp)

### Remaining Global Variables

To tie up our global variable assignments, we'll create variables for the viewport, player, map, and canvas, and an object for the FPS display:

```
<span class="hljs-keyword" role="none" tabindex="0">var</span> viewport;<br><span class="hljs-keyword" role="none" tabindex="0">var</span> player;<br><span class="hljs-keyword" role="none" tabindex="0">var</span> map;<br><span class="hljs-keyword" role="none" tabindex="0">var</span> context;<br><br><span class="hljs-keyword" role="none" tabindex="0">var</span> fps = {<br><span class="hljs-attr" role="none" tabindex="0">count</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">shown</span>: <span class="hljs-number" role="none" tabindex="0">0</span>,<br><span class="hljs-attr" role="none" tabindex="0">last</span>: <span class="hljs-number" role="none" tabindex="0">0</span><br>};
```

### The Game Setup

Now, let's create a custom function called `Setup()` that will create a pointer to the canvas, setup the viewport, and draw in the player and map:

```
<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span> <span class="hljs-title" role="none" tabindex="0">Setup</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>context = <span class="hljs-built_in" role="none" tabindex="0">document</span>.getElementById(<span class="hljs-string" role="none" tabindex="0">"game"</span>).getContext(<span class="hljs-string" role="none" tabindex="0">"2d"</span>);<br>viewport = <span class="hljs-keyword" role="none" tabindex="0">new</span> Viewport(<span class="hljs-number" role="none" tabindex="0">0</span>, <span class="hljs-number" role="none" tabindex="0">0</span>, config.win.width, config.win.height);<br>player = <span class="hljs-keyword" role="none" tabindex="0">new</span> Player(<span class="hljs-number" role="none" tabindex="0">4</span>, <span class="hljs-number" role="none" tabindex="0">3</span>);<br>map = <span class="hljs-keyword" role="none" tabindex="0">new</span> <span class="hljs-built_in" role="none" tabindex="0">Map</span>(<span class="hljs-string" role="none" tabindex="0">"Map"</span>);<br><br>Sizing();<br><br>setInterval(<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>fps.shown = fps.count;<br>}, <span class="hljs-number" role="none" tabindex="0">1000</span>);<br>}
```

The `viewport` object points to a custom class, `Viewport`, which we'll cover in a bit, and defines a display of the size of the browser's viewport using our pre-defined global `config` values.

The `Sizing()` function, another method we'll dive into shortly, is invoked to set the width and height of the `window` and `canvas` elements.

We also set up a recurring loop that updates the FPS count every second.

### Window & Canvas Sizing

Here is our `Sizing()` function in action that resets the width and height of the window and `canvas` element, the number of tiles horizontally and vertically on the screen, and the viewport's center. This function is called when the page loads or when the window is resized manually by the player:

```
<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span> <span class="hljs-title" role="none" tabindex="0">Sizing</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>config.win = {<br><span class="hljs-attr" role="none" tabindex="0">width</span>: <span class="hljs-built_in" role="none" tabindex="0">window</span>.innerWidth,<br><span class="hljs-attr" role="none" tabindex="0">height</span>: <span class="hljs-built_in" role="none" tabindex="0">window</span>.innerHeight<br>};<br><br>config.tiles = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(config.win.width / config.size.tile),<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(config.win.height / config.size.tile)<br>}<br><br>config.center = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(config.tiles.x / <span class="hljs-number" role="none" tabindex="0">2</span>),<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(config.tiles.y / <span class="hljs-number" role="none" tabindex="0">2</span>)<br>}<br><br>viewport.x = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>viewport.y = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>viewport.w = config.win.width;<br>viewport.h = config.win.height;<br><br>context.canvas.width = config.win.width;<br>context.canvas.height = config.win.height;<br>}
```

> Resetting the canvas size and other pre-defined object variables is crucial to prevent skewing of the game display.

### Log FPS and Player Position Data

Let's create a function to log the FPS count and player coordinates to the `div` container:

```
<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span> <span class="hljs-title" role="none" tabindex="0">Log</span>(<span class="hljs-params" role="none" tabindex="0">type, text</span>) </span>{<br><span class="hljs-built_in" role="none" tabindex="0">document</span>.getElementById(type).innerHTML = text;<br>}
```

This method accepts two arguments, `type`, which is the `div` element with ID `fps`, which is added in the event you want to add multiple `div` containers to log data, and the `text` argument, which is the text data that will output on the screen.

### Load the JSON Data

Next, we have a custom function, `LoadURL()`, that accepts a single argument, `url`, which is the relative URL to a JSON file within our code, and loads JSON data for our map:

```
<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span> <span class="hljs-title" role="none" tabindex="0">LoadURL</span>(<span class="hljs-params" role="none" tabindex="0">url, callback</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> http = <span class="hljs-keyword" role="none" tabindex="0">new</span> XMLHttpRequest();<br><br>http.overrideMimeType(<span class="hljs-string" role="none" tabindex="0">"application/json"</span>);<br>http.open(<span class="hljs-string" role="none" tabindex="0">"GET"</span>, url + <span class="hljs-string" role="none" tabindex="0">"?v="</span> + <span class="hljs-keyword" role="none" tabindex="0">new</span> <span class="hljs-built_in" role="none" tabindex="0">Date</span>().getTime(), <span class="hljs-literal" role="none" tabindex="0">true</span>);<br>http.onreadystatechange = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (http.readyState === <span class="hljs-number" role="none" tabindex="0">4</span> &amp;&amp; http.status == <span class="hljs-string" role="none" tabindex="0">"200"</span>) {<br>callback(http.responseText);<br>}<br>}<br>http.send(<span class="hljs-literal" role="none" tabindex="0">null</span>);<br>}
```

If the JSON data is found and parsed correctly, it's returned to the originating process using the `callback` method.

### The Game Loop

And now we get to the game loop in the `Loop()` function, a custom method that loops within itself using the `window.requestAnimationFrame()` [method to handle animations](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame) of the player and map within the `canvas` element:

```
<span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span> <span class="hljs-title" role="none" tabindex="0">Loop</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-built_in" role="none" tabindex="0">window</span>.requestAnimationFrame(Loop);<br><br>Sizing();<br><br>viewport.center();<br>map.draw();<br>player.draw();<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (!fps.last) {<br>fps.last = <span class="hljs-built_in" role="none" tabindex="0">Date</span>.now();<br>fps.count = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>}<br><br><span class="hljs-keyword" role="none" tabindex="0">let</span> delta = (<span class="hljs-built_in" role="none" tabindex="0">Date</span>.now() - fps.last) / <span class="hljs-number" role="none" tabindex="0">1000</span>;<br>fps.last = <span class="hljs-built_in" role="none" tabindex="0">Date</span>.now();<br>fps.count = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(<span class="hljs-number" role="none" tabindex="0">1</span> / delta);<br><br>Log(<span class="hljs-string" role="none" tabindex="0">"fps"</span>, <span class="hljs-string" role="none" tabindex="0">"FPS: "</span> + fps.shown);<br>}
```

As the loop continues, the `Sizing()` method is called to handle any manual resizing of the browser window, so the `config` values and `canvas` size are updated accordingly. Not doing this will cause unwanted stretching of the game display.

We continue by re-centering the viewport with `viewport.center()`, updating the map's on screen position with `map.draw()`, and updating the player's position and sprite frame with `player.draw()`.

Finally, we run a few calculations to determine the frames per second to display them to the user.

### On Window Load

Once the page is loaded and all elements are available in the DOM, we'll call our custom `Setup()` function that we defined a bit ago to process all the page elements:

```
<span class="hljs-built_in" role="none" tabindex="0">window</span>.onload = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>Setup();<br>};
```

### On Window Resize

If the user resizes the window, our custom `Sizing()` function is called to recalculate and reposition the window and `canvas` element size along with the number of tiles on the screen and the viewport's center:

```
<span class="hljs-built_in" role="none" tabindex="0">window</span>.onresize = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>Sizing();<br>};
```

## Map.js

This script handles the loading and drawing of the map using the JSON data pulled from a local file.

### The `Map` Class

Here, we define our `Map` class and its associated variables:

```
<span class="hljs-keyword" role="none" tabindex="0">const</span> <span class="hljs-built_in" role="none" tabindex="0">Map</span> = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">title</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.data = {};<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.tiles = [];<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.timer = setInterval(<span class="hljs-string" role="none" tabindex="0">"map.frame()"</span>, <span class="hljs-number" role="none" tabindex="0">750</span>);<br><br><span class="hljs-keyword" role="none" tabindex="0">this</span>.load(title);<br>};
```

-   `this.data`: contains the map data loaded from our JSON file.
-   `this.tiles`: contains pointers to each of the tile images defined in the JSON file.
-   `this.timer`: loops through the `map.frame()` function every 3/4 seconds and animates the map sprites, giving the tiles an animated effect and making the map more lively.

### Loading the Map

`load()` is a function within our `Map` class, accepting a single argument, `title`, which is the name of the JSON file in title-case format:

```
load: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">title</span>) </span>{<br>LoadURL(<span class="hljs-string" role="none" tabindex="0">"assets/json/"</span> + title.toString().toLowerCase() + <span class="hljs-string" role="none" tabindex="0">".json"</span>, <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">result</span>) </span>{<br>map.data = <span class="hljs-built_in" role="none" tabindex="0">JSON</span>.parse(result);<br>map.data.frame = <span class="hljs-number" role="none" tabindex="0">0</span>;<br><br><span class="hljs-keyword" role="none" tabindex="0">let</span> init = <span class="hljs-literal" role="none" tabindex="0">false</span>;<br><span class="hljs-keyword" role="none" tabindex="0">let</span> loaded = <span class="hljs-number" role="none" tabindex="0">0</span>;<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> i = <span class="hljs-number" role="none" tabindex="0">0</span>; i &lt; map.data.assets.length; i++) {<br>map.tiles.push(<span class="hljs-keyword" role="none" tabindex="0">new</span> Image());<br>map.tiles[i].src = <span class="hljs-string" role="none" tabindex="0">"assets/img/tile/"</span> + map.data.assets[i].file_name + <span class="hljs-string" role="none" tabindex="0">".png?v="</span> + <span class="hljs-keyword" role="none" tabindex="0">new</span> <span class="hljs-built_in" role="none" tabindex="0">Date</span>().getTime();<br><br>map.tiles[i].onload = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>loaded++;<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (!init &amp;&amp; loaded == map.data.assets.length) {<br>init = <span class="hljs-literal" role="none" tabindex="0">true</span>;<br><br>Loop();<br>}<br>};<br>}<br>});<br>}
```

This method loads our map data using the `LoadURL()` function we defined earlier, loops through each of the unique tile assets, and loads the image files into memory so we can display them in our map.

### Drawing the Map to the Screen

Next, our `draw()` method within the `Map` class handles painting the map tiles to the `canvas` element. This is a bit more complicated, so we'll split it up into multiple sections:

```
draw: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br>...<br>}
```

There are four variables, `x_min`, `y_min`, `x_max`, and `y_max`, which determine the starting and ending horizontal and vertical positions of the map relative to the viewport:

```
<span class="hljs-keyword" role="none" tabindex="0">let</span> x_min = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.floor(viewport.x / config.size.tile);<br><span class="hljs-keyword" role="none" tabindex="0">let</span> y_min = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.floor(viewport.y / config.size.tile);<br><span class="hljs-keyword" role="none" tabindex="0">let</span> x_max = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil((viewport.x + viewport.w) / config.size.tile);<br><span class="hljs-keyword" role="none" tabindex="0">let</span> y_max = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil((viewport.y + viewport.h) / config.size.tile);
```

These calculations are made to improve the processing of the player and map animations by cropping out the parts of the map that aren't visible in the viewport. Only the visible tiles are animated. This process is called [occlusion culling](https://docs.unity3d.com/560/Documentation/Manual/OcclusionCulling.html) and can be accomplished in both 2D and 3D environments.

Here's an example screenshot where the area within the red box contains the visible tiles and is output to the `canvas` element. The rest of the map tiles are omitted from the output, improving the performance of the game and the animations overall:

![Occlusion culling 2D](https://orangeable.com/media/post/2d-game-occlusion-culling.webp)

We then run a few checks to see if the player has hit any of the four edges of the map and adjust our values accordingly:

```
<span class="hljs-attribute" role="none" tabindex="0">if</span> (x_min &lt; <span class="hljs-number" role="none" tabindex="0">0</span>) { <span class="hljs-attribute" role="none" tabindex="0">x_min</span> = <span class="hljs-number" role="none" tabindex="0">0</span>; }<br><span class="hljs-attribute" role="none" tabindex="0">if</span> (y_min &lt; <span class="hljs-number" role="none" tabindex="0">0</span>) { <span class="hljs-attribute" role="none" tabindex="0">y_min</span> = <span class="hljs-number" role="none" tabindex="0">0</span>; }<br><span class="hljs-attribute" role="none" tabindex="0">if</span> (x_max &gt; map.width) { <span class="hljs-attribute" role="none" tabindex="0">x_max</span> = map.width; }<br><span class="hljs-attribute" role="none" tabindex="0">if</span> (y_max &gt; map.height) { <span class="hljs-attribute" role="none" tabindex="0">y_max</span> = map.height; }
```

Next, we loop through our viewable tile positions from start to finish and paint them to the `canvas` element:

```
<span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> y = y_min; y &lt; y_max; y++) {<br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> x = x_min; x &lt; x_max; x++) {<br><span class="hljs-keyword" role="none" tabindex="0">let</span> value = <span class="hljs-keyword" role="none" tabindex="0">this</span>.data.layout[y][x] - <span class="hljs-number" role="none" tabindex="0">1</span>;<br><span class="hljs-keyword" role="none" tabindex="0">let</span> tile_x = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.floor((x * config.size.tile) - viewport.x + (config.win.width / <span class="hljs-number" role="none" tabindex="0">2</span>) - (viewport.w / <span class="hljs-number" role="none" tabindex="0">2</span>));<br><span class="hljs-keyword" role="none" tabindex="0">let</span> tile_y = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.floor((y * config.size.tile) - viewport.y + (config.win.height / <span class="hljs-number" role="none" tabindex="0">2</span>) - (viewport.h / <span class="hljs-number" role="none" tabindex="0">2</span>));<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (value &gt; <span class="hljs-number" role="none" tabindex="0">-1</span>) {<br><span class="hljs-keyword" role="none" tabindex="0">let</span> frame = <span class="hljs-keyword" role="none" tabindex="0">this</span>.data.frame;<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (frame &gt; <span class="hljs-keyword" role="none" tabindex="0">this</span>.data.assets[value].frames) {<br>frame = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>}<br><br>context.drawImage(<br>map.tiles[value],<br>frame * config.size.tile,<br><span class="hljs-number" role="none" tabindex="0">0</span>,<br>config.size.tile,<br>config.size.tile,<br>tile_x,<br>tile_y,<br>config.size.tile,<br>config.size.tile<br>);<br>}<br>}<br>}
```

As we paint each tile, we loop through the two animated map sprite frames and output to the screen accordingly, giving the tall grass and water sprites that "blowing in the wind" effect with our sprite images.

### Sprite Animations

Finally, we have the `frame()` method that determines which frame of each of the map tile sprites should be displayed, either the first frame or the second frame:

```
frame: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.data.frame = (<span class="hljs-keyword" role="none" tabindex="0">this</span>.data.frame == <span class="hljs-number" role="none" tabindex="0">0</span>) ? <span class="hljs-number" role="none" tabindex="0">1</span> : <span class="hljs-number" role="none" tabindex="0">0</span>;<br>}
```

## Player.js

This script contains everything necessary to load, display, and control the player within the map.

### The `Player` Class

Let's look at the `Player` class and its associated functions and variables. Here is the class definition, accepting two arguments, `tile_x` and `tile_y`, which are the X and Y position of the player relative to the tiles on the map:

```
<span class="hljs-keyword" role="none" tabindex="0">const</span> Player = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">tile_x, tile_y</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.timer = setInterval(<span class="hljs-string" role="none" tabindex="0">"player.frame()"</span>, <span class="hljs-number" role="none" tabindex="0">125</span>);<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.frames = [<span class="hljs-number" role="none" tabindex="0">0.40</span>, <span class="hljs-number" role="none" tabindex="0">0.42</span>, <span class="hljs-number" role="none" tabindex="0">0.44</span>, <span class="hljs-number" role="none" tabindex="0">0.46</span>, <span class="hljs-number" role="none" tabindex="0">0.48</span>, <span class="hljs-number" role="none" tabindex="0">0.50</span>, <span class="hljs-number" role="none" tabindex="0">0.48</span>, <span class="hljs-number" role="none" tabindex="0">0.46</span>, <span class="hljs-number" role="none" tabindex="0">0.44</span>, <span class="hljs-number" role="none" tabindex="0">0.42</span>, <span class="hljs-number" role="none" tabindex="0">0.40</span>];<br><br><span class="hljs-keyword" role="none" tabindex="0">this</span>.sprite = <span class="hljs-keyword" role="none" tabindex="0">new</span> Image();<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.sprite.src = <span class="hljs-string" role="none" tabindex="0">"assets/img/char/hero.png"</span>;<br><br><span class="hljs-keyword" role="none" tabindex="0">this</span>.movement = {<br><span class="hljs-attr" role="none" tabindex="0">moving</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">key</span>: <span class="hljs-number" role="none" tabindex="0">40</span>,<br><span class="hljs-attr" role="none" tabindex="0">frame</span>: <span class="hljs-number" role="none" tabindex="0">1</span><br>};<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.pos = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: config.size.tile * tile_x,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: config.size.tile * tile_y<br>};<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.tile = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: tile_x,<br><span class="hljs-attr" role="none" tabindex="0">y</span>: tile_y<br>};<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.torch = {<br><span class="hljs-attr" role="none" tabindex="0">lit</span>: <span class="hljs-literal" role="none" tabindex="0">false</span>,<br><span class="hljs-attr" role="none" tabindex="0">frame</span>: <span class="hljs-number" role="none" tabindex="0">0</span><br>};<br>};
```

-   `this.timer`: loops every 1/8 seconds to change the animation sprite frame of the player. These sprite frames are pre-determined in the global `keys` object's `f` array.
-   `this.frames`: the player has the option of toggle a torch by pressing the `T` key on the keyboard. This variable controls the opacity of the surrounding torch frames. We'll get into this more in a bit.
-   `this.sprite`: the sprite image frames for the player.
-   `this.movement`: determines if the player is moving, what key is being pressed, and which animation frame the player is currently on as the sprite is animating.
-   `this.pos`: the X and Y position of the player on the map in pixels.
-   `this.tile`: the X and Y tile position of the player on the map relative to the tiles of the map.
-   `this.torch`: determines if the torch is being used and the opacity of the torch light's edge.

### Draw & Update the Player

First, let's explore the `draw()` function:

```
draw: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> frame = (player.movement.moving) ? keys[player.movement.key].f[player.movement.frame] : keys[player.movement.key].f[<span class="hljs-number" role="none" tabindex="0">1</span>];<br><span class="hljs-keyword" role="none" tabindex="0">let</span> pos_x = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(player.pos.x - viewport.x + (config.win.width / <span class="hljs-number" role="none" tabindex="0">2</span>) - (viewport.w / <span class="hljs-number" role="none" tabindex="0">2</span>));<br><span class="hljs-keyword" role="none" tabindex="0">let</span> pos_y = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.round(player.pos.y - viewport.y + (config.win.height / <span class="hljs-number" role="none" tabindex="0">2</span>) - (viewport.h / <span class="hljs-number" role="none" tabindex="0">2</span>));<br><br><span class="hljs-keyword" role="none" tabindex="0">this</span>.light(pos_x, pos_y);<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.torch_func(pos_x, pos_y);<br><br>context.drawImage(<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.sprite,<br>frame * config.size.char,<br><span class="hljs-number" role="none" tabindex="0">0</span>,<br>config.size.char,<br>config.size.char,<br>pos_x,<br>pos_y,<br>config.size.char,<br>config.size.char<br>);<br>}
```

Here, the player's frame is determined by whether or not the player is moving, then the position is calculated based on their location on the map relative to the viewport.

Next, we move the player highlight behind the player's new position. The position of the torch is also updated in the event the player toggles the torch on.

Then, we draw the player to the screen and move the map's position, keeping the player centered in the viewport.

### Player Highlight

Here, we create a highlight that's visible behind the player. The highlight is just a radial gradient effect added to separate the player from the map to make the player stand out more:

```
light: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">pos_x, pos_y</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> light_x = pos_x + (config.size.tile / <span class="hljs-number" role="none" tabindex="0">2</span>);<br><span class="hljs-keyword" role="none" tabindex="0">let</span> light_y = pos_y + (config.size.tile / <span class="hljs-number" role="none" tabindex="0">2</span>);<br><br><span class="hljs-keyword" role="none" tabindex="0">let</span> radius = <span class="hljs-number" role="none" tabindex="0">100</span>;<br><span class="hljs-keyword" role="none" tabindex="0">let</span> radialGradient = context.createRadialGradient(light_x, light_y, <span class="hljs-number" role="none" tabindex="0">0</span>, light_x, light_y, radius);<br><br>radialGradient.addColorStop(<span class="hljs-number" role="none" tabindex="0">0</span>, <span class="hljs-string" role="none" tabindex="0">"rgba(238, 229, 171, 0.325)"</span>);<br>radialGradient.addColorStop(<span class="hljs-number" role="none" tabindex="0">1</span>, <span class="hljs-string" role="none" tabindex="0">"rgba(238, 229, 171, 0)"</span>);<br><br>context.fillStyle = radialGradient;<br>context.arc(light_x, light_y, radius, <span class="hljs-number" role="none" tabindex="0">0</span>, <span class="hljs-built_in" role="none" tabindex="0">Math</span>.PI * <span class="hljs-number" role="none" tabindex="0">2</span>);<br>context.fill();<br>}
```

### Torch Lighting Effect

The player has a hidden torch that can be activated and deactivated by pressing the `T` key on your keyboard. When activated, a large, pixelated gradient effect pulsates and surrounds the player:

  

To accomplish this effect, the `torch_func()` method accepts two arguments, `pos_x` and `pos_y`, which are the player's X and Y positions on the map. If the torch is lit, we loop through the visible tiles and create a pixelated radial gradient effect around the player, hiding everything else that exceeds the surroundings of the torchlight, providing an eerie lighting effect:

```
torch_func: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">pos_x, pos_y</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (<span class="hljs-keyword" role="none" tabindex="0">this</span>.torch.lit) {<br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> y = <span class="hljs-number" role="none" tabindex="0">0</span>; y &lt; config.tiles.y; y++) {<br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> x = <span class="hljs-number" role="none" tabindex="0">0</span>; x &lt; config.tiles.x; x++) {<br><span class="hljs-keyword" role="none" tabindex="0">let</span> distance = <span class="hljs-built_in" role="none" tabindex="0">Math</span>.sqrt((<span class="hljs-built_in" role="none" tabindex="0">Math</span>.pow(x - config.center.x, <span class="hljs-number" role="none" tabindex="0">2</span>)) + (<span class="hljs-built_in" role="none" tabindex="0">Math</span>.pow(y - config.center.y, <span class="hljs-number" role="none" tabindex="0">2</span>)));<br><span class="hljs-keyword" role="none" tabindex="0">let</span> opacity = (distance / <span class="hljs-number" role="none" tabindex="0">4</span>) - <span class="hljs-keyword" role="none" tabindex="0">this</span>.frames[<span class="hljs-keyword" role="none" tabindex="0">this</span>.torch.frame];<br><br>context.fillStyle = <span class="hljs-string" role="none" tabindex="0">"rgba(0, 0, 0, "</span> + opacity + <span class="hljs-string" role="none" tabindex="0">")"</span>;<br>context.fillRect((x * config.size.tile) - (config.size.tile / <span class="hljs-number" role="none" tabindex="0">2</span>), (y * config.size.tile) - (config.size.tile / <span class="hljs-number" role="none" tabindex="0">2</span>), config.size.tile, config.size.tile);<br>}<br>}<br>}<br>}
```

### Player Sprite Frames

In the `frame()` method, we loop through the player's movement and torch frames and update accordingly. As the player moves, it steps through each of the frames in regular intervals, making the player appear like they're walking. Once the animation is completed, it loops back to the beginning so the player appears to move in a linear motion.

The torch frames are also controlled here, giving the torch in the player's hand a glowing effect:

```
frame: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.movement.frame++;<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (<span class="hljs-keyword" role="none" tabindex="0">this</span>.movement.frame == <span class="hljs-number" role="none" tabindex="0">4</span>) {<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.movement.frame = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>}<br><br><span class="hljs-keyword" role="none" tabindex="0">this</span>.torch.frame++;<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (<span class="hljs-keyword" role="none" tabindex="0">this</span>.torch.frame == <span class="hljs-keyword" role="none" tabindex="0">this</span>.frames.length) {<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.torch.frame = <span class="hljs-number" role="none" tabindex="0">0</span>;<br>}<br><br>player.movement.frame = <span class="hljs-keyword" role="none" tabindex="0">this</span>.movement.frame;<br>player.torch = <span class="hljs-keyword" role="none" tabindex="0">this</span>.torch;<br>}
```

### Player Movement

The `move()` method determines the player's movement across the map and adds collision detection for tiles that the player can't walk over. This method accepts two arguments, `x` and `y`, which are the player's current position relative to the map, in pixels.

Collisions are determined based on the JSON data when loading the map. Each tile has a collision flag represented by a boolean value. _1_ means the player can walk across the tile, and _0_ means they can't. As the player moves, their position is updated and a collision check is run.

The `player` object is updated with the latest coordinates and the player's position is logged to the `div` element:

```
move: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">x, y</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> pos = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(<span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.x / config.size.tile),<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil(<span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.y / config.size.tile)<br>};<br><br><span class="hljs-keyword" role="none" tabindex="0">let</span> new_pos = {<br><span class="hljs-attr" role="none" tabindex="0">x</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil((<span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.x + x) / config.size.tile),<br><span class="hljs-attr" role="none" tabindex="0">y</span>: <span class="hljs-built_in" role="none" tabindex="0">Math</span>.ceil((<span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.y + y) / config.size.tile)<br>};<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> i = <span class="hljs-number" role="none" tabindex="0">0</span>; i &lt;= <span class="hljs-number" role="none" tabindex="0">1</span>; i++) {<br><span class="hljs-keyword" role="none" tabindex="0">let</span> tile = ((i == <span class="hljs-number" role="none" tabindex="0">0</span>) ? map.data.layout[pos.y][new_pos.x] : map.data.layout[new_pos.y][pos.x]) - <span class="hljs-number" role="none" tabindex="0">1</span>;<br><span class="hljs-keyword" role="none" tabindex="0">let</span> collision = map.data.assets[tile].collision;<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (!collision) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (i == <span class="hljs-number" role="none" tabindex="0">0</span>) {<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.x += x;<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.tile.x = new_pos.x;<br>}<br><span class="hljs-keyword" role="none" tabindex="0">else</span> {<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.pos.y += y;<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.tile.y = new_pos.y;<br>}<br>}<br>}<br><br>player = <span class="hljs-keyword" role="none" tabindex="0">this</span>;<br><br>Log(<span class="hljs-string" role="none" tabindex="0">"coords"</span>, <span class="hljs-string" role="none" tabindex="0">"Coords: "</span> + <span class="hljs-keyword" role="none" tabindex="0">this</span>.tile.x + <span class="hljs-string" role="none" tabindex="0">", "</span> + <span class="hljs-keyword" role="none" tabindex="0">this</span>.tile.y);<br>}
```

### Moving the Player with Keyboard Input

The last piece of the `player` script takes care of event handling. The only two events we have are `keydown` and `keyup`, detecting whether a key has been pressed or released so the player can be moved or stopped.

If the player presses any of the four arrow keys, the `player.movement.moving` flag is set to _true_, the player's position will update, the player will animate, and the map will move accordingly. This is all done by referencing the pressed key against the `keys` global object. If the key code matches any of these values, an action is performed by the player.

The active flag, `a`, determines if that key is currently being pressed. If it is, its value is set to `true`. This allows the player to move in multiple directions at once, or diagonally, instead of only four directions one at a time.

And, if the `T` key is pressed, the torch will be lit or extinguished.

```
<span class="hljs-built_in" role="none" tabindex="0">document</span>.addEventListener(<span class="hljs-string" role="none" tabindex="0">"keydown"</span>, <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">event</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (event.keyCode &gt;= <span class="hljs-number" role="none" tabindex="0">37</span> &amp;&amp; event.keyCode &lt;= <span class="hljs-number" role="none" tabindex="0">40</span>) {<br>player.movement.moving = <span class="hljs-literal" role="none" tabindex="0">true</span>;<br>player.movement.key = event.keyCode;<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> key <span class="hljs-keyword" role="none" tabindex="0">in</span> keys) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (key == event.keyCode) {<br>keys[key].a = <span class="hljs-literal" role="none" tabindex="0">true</span>;<br>}<br>}<br>}<br><span class="hljs-keyword" role="none" tabindex="0">else</span> {<br><span class="hljs-keyword" role="none" tabindex="0">switch</span> (event.keyCode) {<br><span class="hljs-keyword" role="none" tabindex="0">case</span> <span class="hljs-number" role="none" tabindex="0">84</span>:<br>player.torch.lit = (player.torch.lit) ? <span class="hljs-literal" role="none" tabindex="0">false</span> : <span class="hljs-literal" role="none" tabindex="0">true</span>;<br><span class="hljs-keyword" role="none" tabindex="0">break</span>;<br>}<br>}<br>});
```

When an arrow key is released, the global `keys` object is looped to see if any remaining keys are pressed. If none remain, the `player.movement.moving` flag is set to _false_, meaning the player is no longer moving.

```
<span class="hljs-built_in" role="none" tabindex="0">document</span>.addEventListener(<span class="hljs-string" role="none" tabindex="0">"keyup"</span>, <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">event</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> found = <span class="hljs-literal" role="none" tabindex="0">false</span>;<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> key <span class="hljs-keyword" role="none" tabindex="0">in</span> keys) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (key == event.keyCode) {<br>keys[key].a = <span class="hljs-literal" role="none" tabindex="0">false</span>;<br>}<br>}<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> key <span class="hljs-keyword" role="none" tabindex="0">in</span> keys) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (keys[key].a) {<br>player.movement.key = key;<br>found = <span class="hljs-literal" role="none" tabindex="0">true</span>;<br>}<br>}<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (!found) {<br>player.movement.moving = <span class="hljs-literal" role="none" tabindex="0">false</span>;<br>}<br>});
```

## Viewport.js

The last class we'll cover is the `Viewport` class, available to keep the viewport positioned and centered with the player at all times.

Here is a quick look at the `Viewport` class:

```
<span class="hljs-keyword" role="none" tabindex="0">const</span> Viewport = <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">x, y, w, h</span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.x = x;<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.y = y;<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.w = w;<br><span class="hljs-keyword" role="none" tabindex="0">this</span>.h = h;<br>};
```

The variable assignments are as follows:

-   `this.x`: top-left position of the viewport on the x-axis.
-   `this.y`: top-left position of the viewport on the y-axis.
-   `this.w`: width of the viewport
-   `this.h`: height of the viewport

### Keeping the Viewport Centered

The following code snippet determines the player's position based on its movement, calculates its width and height in pixels, and divides it by two to capture the center of the viewport's position. The viewport's position is updated as the player moves, keeping the player centered on the screen at all times:

```
Viewport.prototype = {<br><span class="hljs-attr" role="none" tabindex="0">center</span>: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0"></span>) </span>{<br><span class="hljs-keyword" role="none" tabindex="0">let</span> move_x = <span class="hljs-number" role="none" tabindex="0">0</span>;<br><span class="hljs-keyword" role="none" tabindex="0">let</span> move_y = <span class="hljs-number" role="none" tabindex="0">0</span>;<br><br><span class="hljs-keyword" role="none" tabindex="0">let</span> center_x = player.pos.x + (config.size.char / <span class="hljs-number" role="none" tabindex="0">2</span>);<br><span class="hljs-keyword" role="none" tabindex="0">let</span> center_y = player.pos.y + (config.size.char / <span class="hljs-number" role="none" tabindex="0">2</span>);<br><br><span class="hljs-keyword" role="none" tabindex="0">for</span> (<span class="hljs-keyword" role="none" tabindex="0">let</span> key <span class="hljs-keyword" role="none" tabindex="0">in</span> keys) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (keys[key].a) {<br><span class="hljs-keyword" role="none" tabindex="0">if</span> (keys[key].x != <span class="hljs-number" role="none" tabindex="0">0</span>) {<br>move_x = keys[key].x;<br>}<br><br><span class="hljs-keyword" role="none" tabindex="0">if</span> (keys[key].y != <span class="hljs-number" role="none" tabindex="0">0</span>) {<br>move_y = keys[key].y;<br>}<br>}<br>}<br><br>player.move(move_x, move_y);<br>viewport.scroll(center_x, center_y);<br>},<br><span class="hljs-attr" role="none" tabindex="0">scroll</span>: <span class="hljs-function" role="none" tabindex="0"><span class="hljs-keyword" role="none" tabindex="0">function</span>(<span class="hljs-params" role="none" tabindex="0">x, y</span>) </span>{<br> <span class="hljs-keyword" role="none" tabindex="0">this</span>.x = x - (<span class="hljs-keyword" role="none" tabindex="0">this</span>.w / <span class="hljs-number" role="none" tabindex="0">2</span>);<br> <span class="hljs-keyword" role="none" tabindex="0">this</span>.y = y - (<span class="hljs-keyword" role="none" tabindex="0">this</span>.h / <span class="hljs-number" role="none" tabindex="0">2</span>);<br> }<br>};
```

## Conclusion

That was a deep dive into creating a 2D tile-based game with JavaScript. I really hope you had fun and learned something new as you followed along through the examples.

You can download or clone the full codeset from the [GitHub repository](https://github.com/orangeable/javascript-2d-tile-based-game). Feel free to play around with the code and turn it into something unique!

If you have any questions or comments, leave them in the comments section below, and make sure to subscribe to the newsletter for more coding updates!

Posted by: [Josh Rowe](https://orangeable.com/about)  
Last Updated: March 18, 2024  
<small>Created: January 31, 2023</small>
