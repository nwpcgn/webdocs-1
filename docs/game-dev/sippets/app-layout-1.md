---
title: AppLayout I
sidebar_position: 8
sidebar_label: AppLayout I
---


## APP-LAYOUT
-------------


[Pen](https://codepen.io/nwpcgn/pen/dPMWZjX) by [nwpcgn](https://codepen.io/nwpcgn) on [CodePen](https://codepen.io).


---


### Style



```css title="layout.css"
/* -------------------------------- Component ------------------------------- */
.stack-box {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-template-areas: "stack-item";

  > * {
    grid-area: stack-item;
    width: 100%;
    height: 100%;
    overflow: hidden;
    position: relative;
  }
}

.main {
  flex: 1;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  display: grid;
}

.main {
  display: grid;
  grid-template-columns: 1fr min-content;
  grid-template-rows: 1fr;
  grid-auto-columns: 1fr;
  grid-auto-flow: row;
  grid-template-areas: "page aside";
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;

  .page {
    grid-area: page;
    width: 100%;
    height: 100%;
    position: relative;
    overflow-x: hidden;
    overflow-y: auto;
  }

  .aside {
    grid-area: aside;
    position: relative;
    overflow-x: hidden;
    overflow-y: auto;
  }
}

.page {
  grid-area: page;
  width: 100%;
  height: 100%;
  position: relative;
  overflow-x: hidden;
  overflow-y: auto;
}

.aside {
  grid-area: aside;
}

/* ------------------------------------------- DEVELOP ------------------------------------------ */

.gb-aside,
.aside,
.gb-footer {
  background-color: oklch(50.156% 0.12916 246.066 / 0.9);
  color: oklch(96.8% 0.007 247.896);
}

.page {
  background-color: oklch(60.307% 0.13444 160.95 / 0.9);
  color: oklch(95% 0.052 163.051);
}

/* For presentation only, no need to copy the code below */
body {
  padding: 1rem;
}
.main {
  gap: 1rem;
  > * {
    border: 1px dashed red;
    position: relative;
  }
}

/* ------------------------------------------- ADVANCED ------------------------------------------ */

:root {
  --spacing-base-define: 1rem;
  --page-width-define: 780px;
  --page-width-sm-define: 360px;
  --spacing-base: var(--spacing-base-define);
  --spacing-xs: calc(var(--spacing-base) * 0.25);
  --spacing-sm: calc(var(--spacing-base) * 0.5);
  --spacing-md: calc(var(--spacing-base) * 0.75);
  --spacing-lg: calc(var(--spacing-base) * 1.5);
  --spacing-xl: calc(var(--spacing-base) * 2);
  --spacing-clamp: clamp(var(--spacing-base), 2vw, var(--spacing-lg));
  --content-space: var(--spacing-base);
  --page-space: var(--content-space);
  --page-width: var(--page-width-define);
}

body {
  width: 100vw;
  height: 100vh;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.img-grid {
  --min: 68px;
  --gap: 0.5rem;
  display: grid;
  grid-gap: var(--gap);
  grid-template-columns: repeat(auto-fit, minmax(min(100%, var(--min)), 1fr));
}

.img-grid figure {
  display: grid;
  place-content: center;
  aspect-ratio: 1/1;
  border: 1px dashed red;
}

svg.icon,
svg.nwp-icon {
  display: inline-block;
  width: 1em;
  height: 1em;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}

svg.icon,
svg.nwp-icon {
  font-size: var(--fs, 24px);
}

svg.icon-lg {
  --fs: 32px;
}

svg.icon-xl {
  --fs: 48px;
}

svg.icon-sm {
  --fs: 20px;
}

svg.icon-xs {
  --fs: 16px;
}

.padded {
  padding: var(--spacing-clamp);
}

.padded-flat {
  padding: 0 clamp(1rem, 2vw, 2rem);
}

.padded-small {
  padding: calc(clamp(1rem, 2vw, 2rem) * 0.5) clamp(1rem, 2vw, 2rem);
}
.page {
  width: 100%;
  height: 100%;
  overflow-x: hidden;
  overflow-y: auto;
  position: relative;
  display: flex;
  flex-direction: column;

  > article {
    width: 100%;
    max-width: var(--page-width, 940px);
    margin-left: auto;
    margin-right: auto;
    padding: var(--spacing-clamp, 1rem);
  }
}

.page.center {
  justify-content: center;
  align-items: center;

  > article {
    text-align: center;
  }
}

.page.animated {
  pointer-events: none;
  z-index: -1;
  opacity: 0;
  transform: translateY(100%);
  transition: opacity 400ms ease-out 200ms, transform 400ms ease-out 0ms;
}

.page.active {
  pointer-events: all;
  opacity: 1;
  transform: translate(0, 0);
  transition: opacity 300ms cubic-bezier(0.82, 0.21, 0.15, 0.88) 100ms,
    transform 400ms ease-in;
  z-index: 1;
}

```


### HTML


```html title="layout.html"
<div class="padded">Appbar</div>
<div class="main">
  <div class="page">
    <header class="padded">
      <h4>Page</h4>
    </header>
  </div>
  <div class="aside">
    <header class="padded">
      <h4>Aside</h4>
    </header>
  </div>
  </div>
```


## GB-LAYOUT
-------------



[Pen](https://codepen.io/nwpcgn/pen/MYyGzgq) by [nwpcgn](https://codepen.io/nwpcgn) on [CodePen](https://codepen.io).


---


### Style



```css title="layout-gb.css"

/* -------------------------------- Component ------------------------------- */

:root {
  --gb-w: 600px;
  --gb-h: 600px;
  --gb-sb-w: 200px;
  --gb-space: 1rem;
  --gb-bg: var(--gb-color-light, #fff);
  --gb-col: var(--gb-color-dark, #333);
}
.gb-4 {
    flex: 1;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  display: grid;
  grid-template-columns: 1fr var(--gb-w, 450px) min-content 1fr;
  grid-template-rows: 1fr var(--gb-h, 450px) min-content 1fr;
  grid-auto-flow: row;
  grid-template-areas:
    ". . . ."
    ". gb-main gb-aside ."
    ". gb-footer . ."
    ". . . .";
}

.gb-footer {  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  gap: 0px 0px;
  grid-auto-flow: row;
  grid-template-areas:
    "bar";
  grid-area: gb-footer;
  width: 100%;
  height: 100%;
}

.bar { grid-area: bar; }

.gb-main {  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-auto-flow: row;
  grid-template-areas:
    "page";
  grid-area: gb-main;
    width: var(--gb-w, 450px);
	height: var(--gb-h, 450px);
}

.page {
  grid-area: page;
  width: 100%;
  height: 100%;
}

.gb-aside {  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-auto-flow: row;
  grid-template-areas:
    "panel";
  grid-area: gb-aside;
   width: var(--gb-sb-w, 200px);
  height: 100%;
}

.panel { grid-area: panel; }


html, body , .gb-4 {
  height: 100%;
  margin: 0;
}



/* ------------------------------------------- DEVELOP ------------------------------------------ */

.gb-aside,
.gb-footer {
  background-color: oklch(50.156% 0.12916 246.066 / 0.9);
  color: oklch(96.8% 0.007 247.896);
}

.page {
  background-color: oklch(60.307% 0.13444 160.95 / 0.9);
  color: oklch(95% 0.052 163.051);
}

/* For presentation only, no need to copy the code below */

.gb-4 {
  gap: 1rem;
  > * {
    border: 1px dashed red;
    position: relative;
  }
}


/* ------------------------------------------- ADVANCED ------------------------------------------ */

:root {
  --spacing-base-define: 1rem;
  --page-width-define: 780px;
  --page-width-sm-define: 360px;
  --spacing-base: var(--spacing-base-define);
  --spacing-xs: calc(var(--spacing-base) * 0.25);
  --spacing-sm: calc(var(--spacing-base) * 0.5);
  --spacing-md: calc(var(--spacing-base) * 0.75);
  --spacing-lg: calc(var(--spacing-base) * 1.5);
  --spacing-xl: calc(var(--spacing-base) * 2);
  --spacing-clamp: clamp(var(--spacing-base), 2vw, var(--spacing-lg));
  --content-space: var(--spacing-base);
  --page-space: var(--content-space);
  --page-width: var(--page-width-define);
}

body {
  width: 100vw;
  height: 100vh;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.img-grid {
  --min: 68px;
  --gap: 0.5rem;
  display: grid;
  grid-gap: var(--gap);
  grid-template-columns: repeat(auto-fit, minmax(min(100%, var(--min)), 1fr));
}

.img-grid figure {
  display: grid;
  place-content: center;
  aspect-ratio: 1/1;
  border: 1px dashed red;
}

svg.icon,
svg.nwp-icon {
  display: inline-block;
  width: 1em;
  height: 1em;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}

svg.icon,
svg.nwp-icon {
  font-size: var(--fs, 24px);
}

svg.icon-lg {
  --fs: 32px;
}

svg.icon-xl {
  --fs: 48px;
}

svg.icon-sm {
  --fs: 20px;
}

svg.icon-xs {
  --fs: 16px;
}

.padded {
  padding: var(--spacing-clamp);
}

.padded-flat {
  padding: 0 clamp(1rem, 2vw, 2rem);
}

.padded-small {
  padding: calc(clamp(1rem, 2vw, 2rem) * 0.5) clamp(1rem, 2vw, 2rem);
}
.page {
  width: 100%;
  height: 100%;
  overflow-x: hidden;
  overflow-y: auto;
  position: relative;
  display: flex;
  flex-direction: column;

  > article {
    width: 100%;
    max-width: var(--page-width, 940px);
    margin-left: auto;
    margin-right: auto;
    padding: var(--spacing-clamp, 1rem);
  }
}

.page.center {
  justify-content: center;
  align-items: center;

  > article {
    text-align: center;
  }
}

.page.animated {
  pointer-events: none;
  z-index: -1;
  opacity: 0;
  transform: translateY(100%);
  transition: opacity 400ms ease-out 200ms, transform 400ms ease-out 0ms;
}

.page.active {
  pointer-events: all;
  opacity: 1;
  transform: translate(0, 0);
  transition: opacity 300ms cubic-bezier(0.82, 0.21, 0.15, 0.88) 100ms,
    transform 400ms ease-in;
  z-index: 1;
}
```


### HTML


```html title="layout-gb.html"
<div class="gb-4">
  <div class="gb-footer">
    <div class="bar">
            <header class="padded-flat">
      <h4>Bar</h4>
    </header>
    </div>
  </div>
  <div class="gb-main">
    <div class="page">
        <header class="padded">
      <h4>Page</h4>
    </header>
    </div>
  </div>
  <div class="gb-aside">
    <div class="panel">
        <header class="padded">
      <h4>Aside</h4>
    </header>
    </div>
  </div>
</div>

```





## GB-LAYOUT Subgrid
---------------------


A [Pen](https://codepen.io/nwpcgn/pen/ByKxGod) by [nwpcgn](https://codepen.io/nwpcgn) on [CodePen](https://codepen.io).

[License](https://codepen.io/license/pen/ByKxGod).





### Style



```css title="layout-gb-s.css"
/* ++++++++++++++++++++++++++ GB-SUBGRID +++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ */
:root {
  --gb-w: 500px;
  --gb-h: 500px;
  --gb-sb-w: 200px;
  --gb-space: 1rem;
  --gb-bg: var(--gb-color-light, #fff);
  --gb-col: var(--gb-color-dark, #333);
}
.gb-4 {
  flex: 1;
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  display: grid;
  grid-template-columns: 1fr var(--gb-w, 450px) min-content 1fr;
  grid-template-rows: 1fr var(--gb-h, 450px) min-content 1fr;
  grid-auto-flow: row;
  grid-template-areas:
    ". . . ."
    ". gb-main gb-aside ."
    ". gb-footer . ."
    ". . . .";
}

.gb-footer {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-template-areas: "bar";
  grid-area: gb-footer;
  width: 100%;
  height: 100%;
}

.bar {
  grid-area: bar;
}

.gb-main {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  gap: 0;
  grid-template-areas: "page";
  grid-area: gb-main;
  width: var(--gb-w, 450px);
  height: var(--gb-h, 450px);
  /* Subgrid aktivieren */
  grid-template-columns: subgrid;
  grid-template-rows: subgrid;
}

.page {
  grid-area: page;
  width: 100%;
  height: 100%;
}

.gb-aside {
  display: grid;
  grid-template-columns: 1fr;
  grid-template-rows: 1fr;
  grid-template-areas: "panel";
  grid-area: gb-aside;

  /* Subgrid aktivieren */
  grid-template-columns: subgrid;
  grid-template-rows: subgrid;
}

.panel {
  width: var(--gb-sb-w, 200px);
  height: 100%;
  grid-area: panel;
}

html,
body,
.gb-4 {
  height: 100%;
  margin: 0;
}

/* ------------------------------------------- DEVELOP ------------------------------------------ */

.gb-aside,
.gb-footer {
  background-color: oklch(50.156% 0.12916 246.066 / 0.9);
  color: oklch(96.8% 0.007 247.896);
}

.page {
  background-color: oklch(60.307% 0.13444 160.95 / 0.9);
  color: oklch(95% 0.052 163.051);
}

/* For presentation only, no need to copy the code below */

.gb-4 {
  gap: 1rem;
  > * {
    border: 1px dashed red;
    position: relative;
  }
}

/* ------------------------------------------- ADVANCED ------------------------------------------ */

:root {
  --spacing-base-define: 1rem;
  --page-width-define: 780px;
  --page-width-sm-define: 360px;
  --spacing-base: var(--spacing-base-define);
  --spacing-xs: calc(var(--spacing-base) * 0.25);
  --spacing-sm: calc(var(--spacing-base) * 0.5);
  --spacing-md: calc(var(--spacing-base) * 0.75);
  --spacing-lg: calc(var(--spacing-base) * 1.5);
  --spacing-xl: calc(var(--spacing-base) * 2);
  --spacing-clamp: clamp(var(--spacing-base), 2vw, var(--spacing-lg));
  --content-space: var(--spacing-base);
  --page-space: var(--content-space);
  --page-width: var(--page-width-define);
}

body {
  width: 100vw;
  height: 100vh;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.img-grid {
  --min: 68px;
  --gap: 0.5rem;
  display: grid;
  grid-gap: var(--gap);
  grid-template-columns: repeat(auto-fit, minmax(min(100%, var(--min)), 1fr));
}

.img-grid figure {
  display: grid;
  place-content: center;
  aspect-ratio: 1/1;
  border: 1px dashed red;
}

svg.icon,
svg.nwp-icon {
  display: inline-block;
  width: 1em;
  height: 1em;
  stroke-width: 0;
  stroke: currentColor;
  fill: currentColor;
}

svg.icon,
svg.nwp-icon {
  font-size: var(--fs, 24px);
}

svg.icon-lg {
  --fs: 32px;
}

svg.icon-xl {
  --fs: 48px;
}

svg.icon-sm {
  --fs: 20px;
}

svg.icon-xs {
  --fs: 16px;
}

.padded {
  padding: var(--spacing-clamp);
}

.padded-flat {
  padding: 0 clamp(1rem, 2vw, 2rem);
}

.padded-small {
  padding: calc(clamp(1rem, 2vw, 2rem) * 0.5) clamp(1rem, 2vw, 2rem);
}
.page {
  width: 100%;
  height: 100%;
  overflow-x: hidden;
  overflow-y: auto;
  position: relative;
  display: flex;
  flex-direction: column;

  > article {
    width: 100%;
    max-width: var(--page-width, 940px);
    margin-left: auto;
    margin-right: auto;
    padding: var(--spacing-clamp, 1rem);
  }
}

.page.center {
  justify-content: center;
  align-items: center;

  > article {
    text-align: center;
  }
}

.page.animated {
  pointer-events: none;
  z-index: -1;
  opacity: 0;
  transform: translateY(100%);
  transition: opacity 400ms ease-out 200ms, transform 400ms ease-out 0ms;
}

.page.active {
  pointer-events: all;
  opacity: 1;
  transform: translate(0, 0);
  transition: opacity 300ms cubic-bezier(0.82, 0.21, 0.15, 0.88) 100ms,
    transform 400ms ease-in;
  z-index: 1;
}
```


### HTML


```html title="layout-gb-s.html"
<div class="gb-4">
  <div class="gb-footer">
    <div class="bar">
            <header class="padded-flat">
      <h4>Bar</h4>
    </header>
    </div>
  </div>
  <div class="gb-main">
    <div class="page">
        <header class="padded">
      <h4>Page</h4>
    </header>
    </div>
  </div>
  <div class="gb-aside">
    <div class="panel">
        <header class="padded">
      <h4>Aside</h4>
    </header>
    </div>
  </div>
</div>
```