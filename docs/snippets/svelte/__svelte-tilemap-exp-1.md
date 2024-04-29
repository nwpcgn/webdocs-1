---
sidebar_position: 5
sidebar_label: Tilemap 1
---



# Tile Map Example





The goal of this article is to explain the process I used for making tilemaps in javascript. I will explain the process of designing the map and then writing a function to render the map in the browser. A live example of the project I will be explaining can be found [here](https://tjconti12.github.io/Cat-Crusader/). The source code is also available [here](https://github.com/tjconti12/Cat-Crusader).

In my game Cat Crusader, I knew that I wanted to build a tilemap. Unfamiliar with what a tilemap is? Ever played Super Mario Bros?

![](https://miro.medium.com/v2/resize:fit:875/1*WQhbpRr6ZSeHok1bkzZ9YA.png)

Example of a tilemap used in-game

This old-school classic is a perfect example of a game that uses title maps. Essentially a tilemap is a collection of tiles that can be put together to make up a game map. Think of it as making a puzzle, except you can reuse the pieces! Tilemaps are stored in your project as something called a tile atlas. This is essentially an image holding all of the possible tiles that you can use. A great resource for free tilemaps is [opengameart](https://opengameart.org/). Here is the tilemap that was used for the first level in Cat Crusader.

![](https://miro.medium.com/v2/resize:fit:640/1*AUJjroWwWJGr3AZCzdHCJA.png)

House Level Tilemap. Map courtesy of Sharm, HughSpectrum, and LPC

Which was turned into this:

![](https://miro.medium.com/v2/resize:fit:875/1*BliZQ0fIvTUlAlABsluqcA.png)

House Level in Cat Crusader

## Making A Custom Map

It really is impressive what you can do with tilemaps! Now how exactly did I go about designing these maps? There is an amazing free and open-sourced application called tiled. You can download it [here](https://www.mapeditor.org/). Now the learning curve for tiled was a little steep at first, but with a little time and practice, you can make some amazing maps! When you open tiled, start by creating a new tileset.

![](https://miro.medium.com/v2/resize:fit:815/1*XamhB-ki2hMKNcbGIbCiTg.png)

Tiled application opening page

Here is where you can upload your chosen tile atlas. It is key to know the size of each tile in the atlas. Most of the time they are either 16 or 32 px. If the source you downloaded from does not specify, try either 16 or 32px to see if it is correct. You also have to option to select a margin and spacing, however, most tilesets do not need this. Give your tileset a name and save it!

![](https://miro.medium.com/v2/resize:fit:819/1*8SloRIDgN5nqDbF9jT1nhQ.png)

Creating new Tileset

As you can now see, tiled has highlighted each tile in the tileset. Now you can click create a new map to start designing!.

![](https://miro.medium.com/v2/resize:fit:829/1*F0Ai-PkPhOz9rlTWHm63ig.png)

Newly Created Tileset

You will want to specify the size of your map in width and height. You also need to give it the tile size in pixels again. (You will need this information when it comes time for the code)

![](https://miro.medium.com/v2/resize:fit:814/1*OhOvW4nPXsYfdViPYtUTBw.png)

Create New Map

Now you can grab the individual tiles and design away!

![](https://miro.medium.com/v2/resize:fit:875/1*qEe2P1BAwM_gOWkyarY7-Q.png)

Designing the Map

Tiled gives us the ability to add multiple layers to our maps. For example, if we wanted to add a chair tile on top of a floor tile without the floor disappearing. Tiled makes this pretty simple with the layers view. The only thing to keep in mind with adding more than one layer is that it will make the code more complicated. I will show you the effect this has when we get to the code. Now tile has the ability to export the map as either a javascript map file or a JSON map file. I chose to use a javascript map file because I was only interested in static maps. I believe you can use JSON map files to create larger maps that load as the player progresses.

The newly created javascript map file will contain a lot of information. We are only interested in the data array. This tells us which tile should go in which position. For example, the first number is 17. This means that the 17th tile from the atlas should go in the first position on the map. Save this array, for now, we will use it later in our draw function.

![](https://miro.medium.com/v2/resize:fit:875/1*-hpGwSydp3fWd3lRrYZkGg.png)

Example of the Data Array

## Implementing the Map in Javascript

Now that we have our beautiful maps, we need to write some code to draw out these maps in javascript. To do this, we will use a javascript element called canvas ([here](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial) is a link to the documentation). The canvas element essentially allows us to draw images to a canvas that will be displayed by the browser. These elements can be styled using normal CSS (or scss which was used in cat crusader).

```
<! -- HTML code --><canvas id="test-canvas"></canvas>/* CSS code */#test-canvas {   width: 500px;   height: 250px;}
```

Now we need to write a function that will allow us to draw the correct tiles from the tileset to the canvas. Canvas uses a function called getContext to allow us to draw images, shapes, etc. For example, drawing a simple rectangle on a canvas element.

```
const canvas = document.getElementById('test-canvas');const ctx = canvas.getContext('2d');// Setting the color of the rectanglectx.fillStyle = 'blue';// First parameter is the starting x position on the canvas  // an x value of 0 would start drawing all the way to the left// Second parameter is the starting y position on the canvas  // a y value of 0 would start drawing all the way to the top// The third and fourth parameters are for the width and height of the rectanglectx.fillRect(50, 50, 100, 100);
```

![](https://miro.medium.com/v2/resize:fit:464/1*tuDtamU_RQhwveiw7XRuXA.png)

Blue Square Rendered on a Canvas

The above code will produce a blue square that is 100x100px. It will also start drawing the square 50 pixels from the left, and 50 pixels from the top. [Here](https://codepen.io/tjconti12/pen/abpgVEd) is the link to a codepen that I made for this example.

There are a couple of different ways to deal with images in javascript for this project. You could use the `document.createElement(‘img’)` function. However, javascript has a class constructor that makes this even easier.

```
const tileAtlas = new Image();tileAtlas.src = 'path to image';tileAtlas.onload = drawFunction;
```

Now let’s talk about what we just did. We use the `new Image()` in javascript to create a new image element. Then we set its source to the path of the tile atlas (not the actual map we created, just the atlas that contains all of the tiles). Then we can use the `.onload` method to call our draw function once the image is loaded into javascript.

Once we have our tile atlas loaded, we need to set up a few variables. This isn’t necessarily required, however, it will make the code much easier to edit and reuse for different size levels and different tile atlas pixel sizes (remember the previous example uses tiles that are 32 pixels in size). First, we will declare a variable for the individual tile size, the desired tile output size (more on this later), and the newly updated tile size.

```
let tileSize = 32;let tileOutputSize = 1.5 // can set to 1 for 32px or higherlet updatedTileSize = tileSize * tileOutputSize;
```

We will use these variables in our draw function, so it’s easy to reuse the function for different levels in the future. I decided to make the `tileOutputSize` variable because this allowed me to easily scale up or down the size of my map to ensure it fits within the canvas.

Now we need to set some more variables for the tile atlas. Each tile atlas will have a different amount of rows and columns of tiles. For example, the house level tile atlas contained 16 columns and 14 rows of tiles.

![](https://miro.medium.com/v2/resize:fit:829/1*G1ltBI0Hd-IwtmM0hgPesw.png)

Tile Atlas Rows and Columns Highlighted

The map itself has 25 columns and 14 rows. Let’s go ahead and make variables for the atlas and the map.

```
let atlasCol = 16;let atlasRow = 14;let mapCols = 25;let mapRows = 14;let mapHeight = mapRows * tileSize;let mapWidth = mapCols * tileSize
```

Let’s also add in the previously saved data array. I renamed it to something that makes sense (ex. level1Map).

```
let level1Map = [17, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132,  132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 19, 17, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 19, 17, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19, 17, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 148, 148, 132, 132, 19, 17, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19, 17, 132, 132, 148, 148, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 132, 19, 17, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 148, 19, 17, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 164, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19, 17, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 188, 19]
```

Now it’s time to write the draw function. Essentially our goal is to render the map starting from an x position of 0 and a y position of 0.

![](https://miro.medium.com/v2/resize:fit:625/1*pKL-BCT8I-4CUj2IuHqs-Q.gif)

How the Tiles are rendered onto the Canvas

To do this, we will write a nested for loop. We want to loop through each row for each column of the map. The function also requires three variables that we will declare before the loop. Here is the full draw function. Detailed explanations of each section will follow.

```
let mapIndex = 0;let sourceX = 0;let sourceY = 0;function draw() {   for (let col = 0; col < mapHeight; col += tileSize) {      for (let row = 0; row < mapWidth; row += tileSize) {         let tileVal = level1Map[mapIndex];         if(tileVal !=0) {            tileVal -= 1;            sourceY = Math.floor(tileVal/atlasCol) * tileSize;            sourceX = (tileVal % atlasCol) * tileSize;            ctx.drawImage(tileAtlas, sourceX, sourceY, tileSize,            tileSize, row * tileOutputSize, col * tileOutputSize,            updatedTileSize, updatedTileSize);         }         mapIndex ++;      }   }}
```

The first variable mapIndex represents the position in the `level1Map` array. This will start at 0, and increment every time the inner loop runs. This will evaluate each value inside of the `level1Map` array. The next two variables are the X and Y sources. The `ctx.drawImage()` needs to know exactly how many pixels along the X-axis and the Y-axis of `tileAtlas` to start cutting from. We just initialize the values here.

Each time the inner for loop runs, it will represent the position on the map to be drawn. For example, the first time the nested `for` loop runs, we are trying to draw the top left square of the map.

![](https://miro.medium.com/v2/resize:fit:875/1*TRrc1hZRmOz7ccJiWXS2BQ.png)

First Square Highlighted

The function will first determine the value inside of the `level1Map` array.

```
let tileVal = level1Map[mapindex];// tileVal would be equal to 17 the first time the loop runs
```

Next, we add some logic in case the tile value is 0. (This happens if you use multiple layers, because not every layer has an associated value for each tile. It does not apply however to this example) If the value is 0, nothing is drawn to the canvas for the particular square, and the loop repeats. If the value is not 0, we need to determine the X and Y source to cut from the `tileAtlas` . For this example, the first value is 17, which refers to the first tile on the second row of the tile atlas.

![](https://miro.medium.com/v2/resize:fit:829/1*RNQZYQj8t3nsUwNY8x533w.png)

Tile Atlas Highlighted Showing the Position of the 17th Square

The tiled software that we used to create the map starts counting from 1. So each of the values in the `level1Map` array is actually off by 1. To correct this error, inside of the if statement, subtract 1 from the `tileVal` .

```
tileVal -= 1;
```

We need to tell the `ctx.drawImage()` function to start cutting at an X position of 0, and a Y position of 32px. (Each tile in the atlas is 32px) For any tile originating in the first row of the atlas, its `sourceY` should be equal to 0. If it's originating in the second row, its `sourceY` should be equal to 32 and so on. In this example, any value between 0–15 is in the first row. To determine if the tile is located in the first row, divide the tile value by the number of columns in the tile atlas. If the number is less than 1, then it originates at a Y position of 0. If the number is greater than 1 but less than 2, then it originates at the Y position of 32.

```
sourceY = Math.floor(tileVal/atlasCol) * tileSize;// For a tile originating in the first row of the atlassourceY = Math.floor(1/16) * 32; which will equal 0// For the first tile of this example// sourceY = Math.floor(16/16) * 32; which will equal 32
```

For the X position, we can use the javascript modulus operator `%`. For any tile originating in the first column of the atlas, its `sourceX` should be equal to 0. For the second column, it should be 32 and so on. In this example, the atlas has 16 columns. We want to count how many columns we are from the left.

![](https://miro.medium.com/v2/resize:fit:829/1*yxBslMb9n-6mRJpPqx5k1g.png)

Tile Atlas Highlighted showing the First Two Rows

```
sourceX = tileVal * tileSize;
```

Any `tileVal` between 0 and 15 would work great. However, anything over 15 would cause a problem. We need to reset the value back to 0 if it is greater than 15. This is where the modulus operator comes in.

```
sourceX = (tileVal % atlasCol) * tileSize;// Our first map tile has the value of 16// sourceX = (16 % 16) * 32; which would equal 0// For a tile in the second row and second column// sourceX = (17 % 16) * 32; which would equal 32
```

Now that we know where to start cutting the tile from the atlas, we need to import this information into the `cts.drawImage()` function. This function takes in 9 parameters.

1.  The source of the atlas. The reference to the image. In the example, we called it `tileAtlas`.
2.  `sx`: How many pixels on the X-axis should we begin cutting from. This will be the variable `sourceX`.
3.  `sy`: How many pixels on the Y-axis should we begin cutting from. This will be the variable `sourceY`.
4.  `sWidth`: The source width. How many pixels across the X-axis to be cut. This should be the `tileSize`.
5.  `sHeight`: The source height. How many pixels across the Y-axis to be cut. This should also be the `tileSize`.
6.  `dx`: The destination canvas X coordinate. This is where the top left corner of the tile should be drawn in the destination canvas.
7.  `dy`: The destination canvas Y coordinate. This is where the top left corner of the tile should be drawn in the destination canvas.
8.  `dWidth`: The width of the tile to be drawn on the destination canvas.
9.  `dHeight`: The height of the tile to be drawn on the destination canvas.

![](https://miro.medium.com/v2/resize:fit:375/1*m86406jhoPYMq2A3cqT2Zg.jpeg)

Canvas draw function example. Source [MDN Docs](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/drawImage)

```
ctx.drawImage(tileAtlas, sourceX, sourceY, tileSize, tileSize, row * tileOutputSize, col * tileOutputSize, updatedTileSize, updatedTileSize);
```

## Layering

Now you have a completed function to draw tilemaps! You can repeat this function for different maps very easily by just changing a few variables. If you want to add more than one layer to the map, tiled also allows you to do this.

![](https://miro.medium.com/v2/resize:fit:399/1*fT79hNN322pzztEzBuKFTw.png)

Example of Layering. The chair and table tiles are on top of the floor tiles.

When you export the map as a javascript map file, you will get a data array for each layer that you make.

![](https://miro.medium.com/v2/resize:fit:875/1*_UyjmvjkOIKCYl77fX2wtQ.png)

The data arrays of three different layers. This one is from the Cat Crusader level 1

All you have to do is create a canvas element for each layer that you want to create. Repeat the function for each layer, and draw to the corresponding `ctx` element for that layer.

## Conclusion

I hope this quick tutorial was helpful. Be sure to check out the source code if you’re curious how this was implemented in the game. Stay tuned for future posts about the development of the Cat Crusader! I plan to write about how I created the game engine logic for collisions, gravity, and friction! Big shout out to [PothOnPrograming](https://www.youtube.com/channel/UCdS3ojA8RL8t1r18Gj1cl6w) for the tutorial videos that helped make this game a reality.