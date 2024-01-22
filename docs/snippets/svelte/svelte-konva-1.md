---
sidebar_position: 5
sidebar_label: Canvas 3
---


## How to use canvas with Svelte?

`svelte-konva` is a JavaScript library for drawing complex canvas graphics using Svelte.

[svelte-konva](https://konvajs.org/docs/svelte/index.html)


```
<script>
  import { Stage, Layer, Rect } from 'svelte-konva';
</script>

<Stage config={{ width: window.innerWidth, height: window.innerHeight }}>
  <Layer>
    <Rect config={{ x: 100, y: 100, width: 400, height: 200, fill: 'blue' }} />
  </Layer>
</Stage>
```


[Demo](https://codesandbox.io/p/sandbox/brave-bell-w3ghnn?file=%2Fsrc%2FApp.svelte%3A1%2C1)




