---
title: CP Backgrounds 3
description: Cyberpunk Backgrounds 3
sidebar_position: 9
sidebar_label: CP Backgrounds 3
---

# Logo


![https://nwp-cgn.de/img/poser/cp_logo_1.gif](https://nwp-cgn.de/img/poser/cp_logo_1.gif)

```json title="logo.json"
{
  "frames": [
    {
      "id": 0,
      "img": {
        "width": 720,
        "height": 146
      },
      "url": "https://nwp-cgn.de/img/poser/cp_log_00.png"
    }
  ],
  "animated": {
    "id": 14,
    "img": {
      "width": 500,
      "height": 101
    },
    "url": "https://nwp-cgn.de/img/poser/cp_logo_1.gif"
  }
}
```



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


**URL**

> `https://nwp-cgn.de/147/upload/images/game-frame1.png`

![https://nwp-cgn.de/147/upload/images/game-frame1.png](https://nwp-cgn.de/147/upload/images/game-frame1.png)

## Layer

 name   | url                                                  
--------|------------------------------------------------------
 all    | https://nwp-cgn.de/147/upload/images/game-frame1.png 
 foo    | https://nwp-cgn.de/147/upload/images/foo1.png        
 main   | https://nwp-cgn.de/147/upload/images/main1.png       
 sub    | https://nwp-cgn.de/147/upload/images/sub1.png        
 barrel | https://nwp-cgn.de/147/upload/images/barrels1.png    



### Foo

`https://nwp-cgn.de/147/upload/images/foo1.png`


![https://nwp-cgn.de/147/upload/images/foo1.png](https://nwp-cgn.de/147/upload/images/foo1.png)


### Main

`https://nwp-cgn.de/147/upload/images/main1.png`

![https://nwp-cgn.de/147/upload/images/main1.png](https://nwp-cgn.de/147/upload/images/main1.png)


### Sub

`https://nwp-cgn.de/147/upload/images/sub1.png`

![https://nwp-cgn.de/147/upload/images/sub1.png](https://nwp-cgn.de/147/upload/images/sub1.png)


### Barrels

`https://nwp-cgn.de/147/upload/images/barrels1.png`


![https://nwp-cgn.de/147/upload/images/barrels1.png](https://nwp-cgn.de/147/upload/images/barrels1.png)


| **name**   | **url**                                              |
|------------|------------------------------------------------------|
| **all**    | https://nwp-cgn.de/147/upload/images/game-frame1.png |
| **foo**    | https://nwp-cgn.de/147/upload/images/foo1.png        |
| **main**   | https://nwp-cgn.de/147/upload/images/main1.png       |
| **sub**    | https://nwp-cgn.de/147/upload/images/sub1.png        |
| **barrel** | https://nwp-cgn.de/147/upload/images/barrels1.png    |


## CSS (Dev)


#### vars

```css title="_vars.css"
:root {
    --bg-layer-all-url: url('https://nwp-cgn.de/147/upload/images/game-frame1.png');
    --bg-layer-foo-url: url('https://nwp-cgn.de/147/upload/images/foo1.png');
    --bg-layer-main-url: url('https://nwp-cgn.de/147/upload/images/main1.png');
    --bg-layer-sub-url: url('https://nwp-cgn.de/147/upload/images/sub1.png');
    --bg-layer-barrel-url: url('https://nwp-cgn.de/147/upload/images/barrels1.png');
}


.layer-all {
    --bg-layer-url: url('https://nwp-cgn.de/147/upload/images/game-frame1.png');
}

.layer-foo {
    --bg-layer-url: url('https://nwp-cgn.de/147/upload/images/foo1.png');
}

.layer-main {
    --bg-layer-url: url('https://nwp-cgn.de/147/upload/images/main1.png');
}

.layer-sub {
    --bg-layer-url: url('https://nwp-cgn.de/147/upload/images/sub1.png');
}

.layer-barrel {
    --bg-layer-url: url('https://nwp-cgn.de/147/upload/images/barrels1.png');
}
```



```css title="style.css"
:root {
  --bg-img: url('https://nwp-cgn.de/147/upload/images/game-frame1.png');
}


#game-frame {
  position: absolute;
  left: -4411px;
  top: -2254px;
  width: 6912px;
  height: 641px;
}
#sub1 {
  position: absolute;
  left: 919px;
  top: 0px;
  width: 832px;
  height: 448px;
}

#top_sub {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 832px;
  height: 448px;
}



#main1 {
  position: absolute;
  left: 3392px;
  top: 64px;
  width: 2368px;
  height: 384px;
}

#top_main {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 2368px;
  height: 384px;
}




#foo1 {
  position: absolute;
  left: 0px;
  top: 448px;
  width: 6912px;
  height: 193px;
}

#foo_main {
  position: absolute;
  left: 0px;
  top: 0px;
  width: 6912px;
  height: 193px;
}
```
