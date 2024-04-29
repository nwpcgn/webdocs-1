---
sidebar_position: 5
sidebar_label: Canvas 2
---

# Canvas Example



# Responsible Canvas

> I'm quite new to svelte and I'm trying to get a canvas to render on the full screen using svelte. Sounds quite easy to do, but I can't get it to work properly. I'm binding a `width` and `height` variable to the `clientWidth`/`clientHeight` of the parent and using these variables to set the dimensions of the canvas. The issue now is that when `onMount` is called, the `width` and `height` variables are set but they are not applied to the canvas element yet. This means when the canvas renders for the first time, it still has the initial dimensions and not the ones of the parent. Only when I render it a second time it has the proper dimensions. How can get the canvas to have the right dimensions on the first render or render the canvas again when it has the proper dimensions

[Here](https://svelte.dev/repl/fd3a6f54b1f9436791444b7765be0cd7?version=3.24.0) you can find a "working" version.



```
<script>
  import { onMount } from "svelte";

  let canvas;
  let ctx;
  let width = 1007;
  let height = 1140;

    const draw = () => {
    ctx.clearRect(0, 0, width, height);
    ctx.beginPath();
    ctx.moveTo(width/2 - 50, height/2);
    ctx.arc(width/2, height/2, 50, 0, 2 * Math.PI);
    ctx.fill();
    }
    
  onMount(() => {
    ctx = canvas.getContext("2d");
        draw();
        setTimeout(draw, 5000);
  });
</script>

<style>
  .container {
    width: 100%;
    height: 100%;
  }
</style>

<div
  class="container"
  bind:clientWidth={width}
  bind:clientHeight={height}>
  <canvas bind:this={canvas} {width} {height} />
</div>
```


**You can solve this by waiting for the next 'tick'**

```
import { onMount, tick } from 'svelte';

onMount(async () => {
  ctx = canvas.getContext("2d");
  canvas.width = width;
  canvas.height = height;
  await tick()
  draw();
});
```


