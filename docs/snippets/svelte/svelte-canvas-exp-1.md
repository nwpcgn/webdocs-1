---
sidebar_position: 4
sidebar_label: Canvas 1
---



# Canvas Example

## Simple

```svelte title="App.svelte"
<script>
	import { onMount } from 'svelte';
	let canvas;
	onMount(() => {
		let ctx = canvas.getContext('2d');
		ctx.beginPath();
		ctx.fillRect(20, 20, 150, 100);
		ctx.stroke();
	});
	
</script>

<canvas width="100%" height="100%" bind:this={canvas}></canvas>
```

## App.svelte

```svelte title="App.svelte"
<script\>

import Canvas from './Canvas.svelte';

import Square from './Square.svelte';

  

// we use a seeded random number generator to get consistent jitter

let seed \= 1;

  

function random() {

seed \*= 16807;

seed %= 2147483647;

return (seed \- 1) / 2147483646;

}

  

function jitter(amount) {

return amount \* (random() \- 0.5);

}

</script\>

  

<div class\="container"\>

<Canvas width\={800} height\={1200}>

{#each Array(12) as \_, c}

{#each Array(22) as \_, r}

<Square

x\={180 + c \* 40}

y\={180 + r \* 40}

size\={40}

/>

{/each}

{/each}

</Canvas\>

</div\>

  

<style\>

.container {

height: 100%;

aspect-ratio: 2 / 3;

margin: 0 auto;

background: rgb(224, 219, 213);

filter: drop-shadow(0.5em 0.5em 1em rgba(0, 0, 0, 0.1));

}

</style\>
```

## Canvas.svelte

```svelte title="Canvas.svelte"
<script>
	import { afterUpdate, onMount, tick } from 'svelte';

	export let width = 100;
	export let height = 100;

	let canvas, ctx;
	let items = new Set();
	let scheduled = false;

	onMount(() => {
		ctx = canvas.getContext('2d');
	});

	function addItem(fn) {
		onMount(() => {
			items.add(fn);
			return () => items.delete(fn);
		});
		
		afterUpdate(async () => {
			if (scheduled) return;
			
			scheduled = true;
			await tick();
			scheduled = false;

			draw();
		});
	}

	function draw() {
		ctx.clearRect(0, 0, width, height);
		items.forEach(fn => fn(ctx));
	}
</script>

<canvas bind:this={canvas} {width} {height}>
	<slot />
</canvas>

<style>
	canvas {
		width: 100%;
		height: 100%;
	}
</style>
```


## Square.svelte


```
<script\>

export let x;

export let y;

export let size;

export let rotate;

  

function draw(ctx) {

ctx.save();

  

ctx.translate(x, y);

ctx.rotate(rotate);

  

ctx.strokeStyle \= 'black';

ctx.strokeRect(\-size / 2, \-size / 2, size, size);

  

ctx.restore();

}

</script\>
```
