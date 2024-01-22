---
title: CP Backgrounds 3.1
description: Cyberpunk Backgrounds 3.1
sidebar_position: 10
sidebar_label: CP Backgrounds 3.1
---


 ![https://nwp-cgn.de/img/poser/cp_logo_1.gif](https://nwp-cgn.de/img/poser/cp_logo_1.gif)


## Vanilla JS

```javascript title="app.js"
'use strict';

const elArray = [{
        position: "absolute",
        left: 0,
        top: 0,
        width: 6912,
        height: 641,
        slug: "game-frame1",
    },
    {
        position: "absolute",
        left: 919,
        top: 0,
        width: 832,
        height: 448,
        slug: "sub1",
    },
    {
        position: "absolute",
        left: 3392,
        top: 64,
        width: 2368,
        height: 384,
        slug: "main1",
    },
    {
        position: "absolute",
        left: 0,
        top: 448,
        width: 6912,
        height: 193,
        slug: "foo1",
    },
    {
        position: "absolute",
        left: 679,
        top: 353,
        width: 142,
        height: 96,
        slug: "barrels",
    },
];

let layer = {
    x: 0,
    y: 0,
    width: 100,
    height: 100,
    top: 0,
    left: 0,
};

const frame_elem = document.querySelector("main.main");
const range = document.querySelector("#range1");
const frame_layer = document.querySelector(".game-frame1");

const calcFameElem = (array = []) => {
    function calculateAspectRatio(imgW, imgH, mW, mH, left, top) {
        let ratioWidth = 1,
            ratioHeight = mW / mH,
            ratioAspect =
            ratioWidth > 1 ? ratioWidth : ratioHeight > 1 ? ratioHeight : 1,
            scaleW = imgW / ratioAspect,
            scaleH = imgH / ratioAspect,
            scaleL = left / ratioAspect,
            scaleT = top / ratioAspect,
            h = Math.floor(scaleH),
            w = Math.floor(scaleW),
            x = Math.floor(scaleL),
            y = Math.floor(scaleT),
            end = w - mW;
        return {
            h,
            w,
            x,
            y,
            end
        };
    }
    let temp = "";
    array.map((data, id) => {
        const {
            left,
            top,
            width,
            height,
            slug
        } = data;
        const {
            x,
            y,
            w,
            h,
            end
        } = calculateAspectRatio(
            width,
            height,
            layer.width,
            layer.height,
            left,
            top
        );
        temp += `--layer-${slug}-x: ${x}px; --layer-${slug}-y: ${y}px; --layer-${slug}-w: ${w}px; --layer-${slug}-h: ${h}px; `;
        if (id == 0) {
            range.setAttribute("max", end);
            range.value = 0;
        }
    });
    console.log({
        range
    });
    frame_elem.setAttribute("style", temp);
};

const calcFame = ({
    x,
    y,
    width,
    height,
    top,
    left
}) => {
    let option = {};
    return new Promise((resolve, reject) => {
        option.x = Math.floor(x);
        option.y = Math.floor(y);
        option.width = Math.floor(width);
        option.height = Math.floor(height);
        option.top = Math.floor(top);
        option.left = Math.floor(left);
        layer = {
            ...layer,
            ...option
        };
        if (option) resolve(option);
        reject();
    });
};

const handleXchange = (event) => {
  const val = parseInt(event.target.value);
  let style = `--layer-game-frame1-pos-x: -${val}px; --bg-pos-x: -${val}px;`;
  frame_layer.setAttribute("style", style);
  console.log("onChange", { style });
};

const initFrame = () => {
    const rect = frame_elem.getBoundingClientRect();
    calcFame(rect).then((data) => {
        console.log({
            calculate: data
        });
        calcFameElem(elArray);
    });
};

const clickHandler = (event) => {
    let el1 = event.target.closest("[data-toggle]");
    if (el1) {
        // event.preventDefault();
        console.log(el1.getAttribute("[data-toggle]"));
    }
};

range.addEventListener("change", handleXchange);
document.addEventListener("click", clickHandler, false);
window.addEventListener("load", () => {
    console.log("Document Ready");

    initFrame();
});
```



## SCSS

```scss title="cyberpunk-bg3.scss"
:root {
  /* game-frame1  */
  --layer-game-frame1-x: 0px;
  --layer-game-frame1-y: 0px;
  --layer-game-frame1-w: 6912px;
  --layer-game-frame1-h: 641px;
  --layer-game-frame1-pos-x: 0px;
  --layer-game-frame1-dura: 1000ms;
  /* sub1  */
  --layer-sub1-x: 919px;
  --layer-sub1-y: 0px;
  --layer-sub1-w: 832px;
  --layer-sub1-h: 448px;
  --layer-sub1-pos-x: 0px;
  --layer-sub1-dura: 1000ms;
  /* main1  */
  --layer-main1-x: 3392px;
  --layer-main1-y: 64px;
  --layer-main1-w: 2368px;
  --layer-main1-h: 384px;
  --layer-main1-pos-x: 0px;
  --layer-main1-dura: 1000ms;
  /* foo1  */
  --layer-foo1-x: 0px;
  --layer-foo1-y: 448px;
  --layer-foo1-w: 6912px;
  --layer-foo1-h: 193px;
  --layer-foo1-pos-x: 0px;
  --layer-foo1-dura: 1000ms;
  /* barrels  */
  --layer-barrels-x: 679px;
  --layer-barrels-y: 353px;
  --layer-barrels-w: 142px;
  --layer-barrels-h: 96px;
  --layer-barrels-pos-x: 0px;
  --layer-barrels-dura: 1000ms;
  --bg-layer-all-url: url("https://nwp-cgn.de/147/upload/images/game-frame1.png");
  --bg-layer-foo1-url: url("https://nwp-cgn.de/147/upload/images/foo1.png");
  --bg-layer-main1-url: url("https://nwp-cgn.de/147/upload/images/main1.png");
  --bg-layer-sub1-url: url("https://nwp-cgn.de/147/upload/images/sub1.png");
  --bg-layer-barrels-url: url("https://nwp-cgn.de/147/upload/images/barrels1.png");
  --bg-pos-x: 0px;
}

.game-frame1 {
  // --layer-game-frame1-pos-x: var(--bg-pos-x);
  position: absolute;
  left: var(--layer-game-frame1-x);
  //   top: var(--layer-game-frame1-y);
  bottom: 0;
  width: var(--layer-game-frame1-w);
  height: var(--layer-game-frame1-h);
  transition: transform ease-in-out;
  transition-duration: var(--layer-game-frame1-dura);
  transform: translateX(var(--bg-pos-x));

  .sub1 {
    position: absolute;
    left: var(--layer-sub1-x);
    top: var(--layer-sub1-y);
    width: var(--layer-sub1-w);
    height: var(--layer-sub1-h);
    background-image: var(--bg-layer-sub1-url);
    background-repeat: no-repeat;
    background-size: 100% 100%;
    transition: transform ease-in-out;
    transition-duration: var(--layer-sub1-dura);
    transform: translateX(var(--layer-sub1-pos-x));
  }
  .main1 {
    position: absolute;
    left: var(--layer-main1-x);
    top: var(--layer-main1-y);
    width: var(--layer-main1-w);
    height: var(--layer-main1-h);
    background-image: var(--bg-layer-main1-url);
    background-repeat: no-repeat;
    background-size: 100% 100%;
    transition: transform ease-in-out;
    transition-duration: var(--layer-main1-dura);
    transform: translateX(var(--layer-main1-pos-x));
  }
  .foo1 {
    position: absolute;
    left: var(--layer-foo1-x);
    top: var(--layer-foo1-y);
    width: var(--layer-foo1-w);
    height: var(--layer-foo1-h);
    background-image: var(--bg-layer-foo1-url);
    background-repeat: no-repeat;
    background-size: 100% 100%;
    transition: transform ease-in-out;
    transition-duration: var(--layer-foo1-dura);
    transform: translateX(var(--layer-foo1-pos-x));
  }
  .barrels {
    position: absolute;
    left: var(--layer-barrels-x);
    top: var(--layer-barrels-y);
    width: var(--layer-barrels-w);
    height: var(--layer-barrels-h);
    background-image: var(--bg-layer-barrels-url);
    background-repeat: no-repeat;
    background-size: 100% 100%;
    transition: transform ease-in-out;
    transition-duration: var(--layer-barrels-dura);
    transform: translateX(var(--layer-barrels-pos-x));
  }
}
```













### Img Url

 name   | url                                                  
--------|------------------------------------------------------
 all    | https://nwp-cgn.de/147/upload/images/game-frame1.png 
 foo    | https://nwp-cgn.de/147/upload/images/foo1.png        
 main   | https://nwp-cgn.de/147/upload/images/main1.png       
 sub    | https://nwp-cgn.de/147/upload/images/sub1.png        
 barrel | https://nwp-cgn.de/147/upload/images/barrels1.png    





#### Yaml

```yaml title="data.yaml"
---
-
   name: all
   url: https://nwp-cgn.de/147/upload/images/game-frame1.png
-
   name: foo
   url: https://nwp-cgn.de/147/upload/images/foo1.png
-
   name: main
   url: https://nwp-cgn.de/147/upload/images/main1.png
-
   name: sub
   url: https://nwp-cgn.de/147/upload/images/sub1.png
-
   name: barrel
   url: https://nwp-cgn.de/147/upload/images/barrels1.png
---
```




>  url: `https://nwp-cgn.de/147/upload/images/game-frame1.png`

![https://nwp-cgn.de/147/upload/images/game-frame1.png](https://nwp-cgn.de/147/upload/images/game-frame1.png)
