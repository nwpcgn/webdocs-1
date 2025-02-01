---
sidebar_label: CSS Game
sidebar_position: 123
created: 2024-01-23T14:59:36 (UTC +01:00)
tags:
  - Game
  - Js
source: https://medium.com/cssclass-com/how-to-create-pure-css-games-2a29c777bf4
author: Elad Shechter
---


## How I Built a Pure CSS Game — No JS

The dynamic of the internet is associated most of the time with JavaScript language. But it has been quite a while since CSS has been able to provide you with some of those possibilities.

In order to test these possibilities, I have decided to create my own [**Pure CSS Game — Coronavirus Invaders**](https://codepen.io/elad2412/pen/wvabjXy). In this article, I will teach you how to create those things you thought you could only create with JavaScript. I will do so by demonstrating these capabilities through [my pure CSS game](https://codepen.io/elad2412/pen/wvabjXy).

Note: Some of these CSS tricks are possible with CSS, but from a perspective of semantics and accessibility, it doesn’t mean that I recommend to use them on a daily basis.

![](https://miro.medium.com/v2/resize:fit:750/1*ZN5DJn-gj8DHeiEiSXWWng.gif)

## Creating Functional Main Navigation

In order to create a functional navigation, I used the upper checkbox trick.

1.  In this trick, I have inserted a few checkboxes at the beginning of the HTML. These checkboxes represent the primary states of the game. To hide them visually, I have styled them with `position: absolute` with `top: -40px` and `opacity: 0.001`. Besides that, I gave each one of them a unique ID in order to control them, for example: `id=”whoAmIPopup”`.
2.  The visual buttons of the game are `<label>` elements with an attribute of `for`. The value of the `for` attribute is connected to the unique ID attribute of the checkbox in section 1. Now, when you click on one of the `<label>` elements it will check/uncheck the connected checkbox. In this way, I can target the checkbox, and save states.
3.  In CSS, I can check with the pseudo selector- `:checked`, if a checkbox is checked. And with the sibling selector of `~` I can target any sibling element (and even every child of those sibling elements) and decide if to show or hide them.
4.  In the same way (of section 3) the Start Playing (“Save the World”) is a trigger checkbox, which `display: none` the main screen, and `display: block` the frame of the game.

![](https://miro.medium.com/v2/resize:fit:750/1*tpHI9Y0J-TZH1Djo-aCrVw.gif)

**HTML**

```html
<input class="logic-checkbox" type="checkbox" id="**toggleGame**">  
<input class="logic-checkbox" type="checkbox" id="**HowToPlayPopup**">  
<input class="logic-checkbox" type="checkbox" id="**whoAmIPopup**"><!--game navigation-->  
<section class="game-menu-frame">   
  <ul class="game-nav-list">  
    <li class="game-nav-item">  
      <label for="**toggleGame**" tabindex="0">Save the World</label>  
    </li>  
    <li class="game-nav-item">  
      <label for="**HowToPlayPopup**" tabindex="0">How to Play</label>  
    </li>  
    <li class="game-nav-item">  
      <label for="**whoAmIPopup**" tabindex="0">Who Am I</label>  
    </li>  
  </ul>  
</section><section class="game-frame">  
    <!--game frame-->  
</section><section class="popup common-content" id="whoAmI">  
    <!-- Who Am I popup -->  
</section><section class="popup common-content" id="HowToPlay">  
    <!-- How to play popup -->  
</section>
```

**CSS (Scss)**
```css
/* checkbox which triggering popup */  
#whoAmIPopup:checked ~ #whoAmI,  
#HowToPlayPopup:checked ~ #HowToPlay{  
  display: block;  
}/* checkbox of start playing -   
 trigger by pressing "save the world" button */  
#toggleGame:checked {  
    ~ .game-menu-frame{ display: none; }  
    ~ .game-frame{ display: block; }      
}/* hide popup and game frame */  
#whoAmI,  
#HowToPlay,  
#game-frame{display: none;}
```


[Playable CodePen Example](https://codepen.io/elad2412/pen/42e74f1f1c51c5fade553b6ebe0606ac)

## Killing Coronavirus

The floating Coronavirus in this game are working very similar to the — Functional Main Navigation, but with two small tweaks:

![](https://miro.medium.com/v2/resize:fit:750/1*UiRAjq3bZtA8xRq4flN63g.gif)

Click on Viruses disappearing them

**First**, every coronavirus is a floating `<label>` , But instead of trigger `<input type=”checkbox”>` we are triggering here `<input type=”radio”>`. This way, when clicking on a virus, it will be in the status of checked. This checked status represents that the virus is dead. Because it’s a radio button input, we can change the state of the virus only once (`:checked` = dead).

**Second**, the `<input type=”radio”>` is placed inside the `<label>`. This way when we click on the `<label>` element, it will immediately target the `<input type=”radio”>` . In this case, we don’t need to give `id` attribute for the inputs and the `for` attribute to the `<label>`. This nested connection triggers itself.

The logic of the styles is when the `<input type=”radio”>` is `:checked`, the sibling `<div class=”body”>` element of the corona will get `opacity: 0`. In this way, I make the Coronavirus disappear from the screen.

**HTML**

```html
<label class="corona-virus">  
    <input type="radio">  
    <div class="body">   
        ...  
    </div>  
</label>
```


**CSS (Scss)**

```css
/* checkbox which triggering popup */  
#whoAmIPopup:checked ~ #whoAmI,  
#HowToPlayPopup:checked ~ #HowToPlay{  
  display: block;  
}/* checkbox of start playing -   
 trigger by pressing "save the world" button */  
#toggleGame:checked {  
    ~ .game-menu-frame{ display: none; }  
    ~ .game-frame{ display: block; }      
}/* hide popup and game frame */  
#whoAmI,  
#HowToPlay,  
#game-frame{display: none;}
```


[Playable CodePen Example](https://codepen.io/elad2412/pen/e10a0026b9ab93610300dc0580b3a1ca)

## Calculating the Score Result on Run-time

After we have pressed on some viruses `<label>` while playing the game, those `<input type=”radio”>` got `:checked`. Now, we need to count all those `input[type=”radio”]` which are `:checked`. We do so using CSS Counters, which exist since CSS 2.1 version.

![](https://miro.medium.com/v2/resize:fit:750/1*6XU2A95kne4YuyKP0w6cuw.gif)

CSS Counter calculates radio inputs which are :checked

**Let’s do it step by step:**

1.  First, we define the CSS counter with the `counter-reset` property. The value is a name that we give — it is called **counter value**. For it to work, it needs to be defined on HTML element which is before all the `<input type=”radio”>` elements in our HTML. In our case: `body{ counter-reset: **corona**; }`.
2.  Now we want to count all of those `input[type=”radio”]` which are `:checked` . We will create a CSS selector of `input[type=”radio”]:checked` and we will give it the `counter-increment` property, with the same **counter value** we created in the first section. In our case: `counter-increment: **corona**`.
3.  To show the summarize of the score on the screen, we will add an HTML element, for example: `<div class=”sum”>`, which has to be after all `<input type=”radio”>` elements of the HTML.
4.  Now, what we have left to do is to print it on the screen, using the pseudo-element `::after` selector while using the `content` property style.  
    The CSS `content` property can get a CSS native function called `counter` which will get the `**corona**` **counter value,** that we have already created in section 1, and increment it on section 2.  
    **Example:** `.sum::after{ content: counter(**corona**) “/100”;}`.

**HTML**

```html
<body> <!-- define counter --><ul class="corona-world">  
  <li class="corona-location corona-virus-1-location">  
    <label class="corona-virus">  
      <input type="radio">  
    </label>  
  </li>  
  ... <!-- 100 items of viruses -->  
</ul><div class="sum">   
  <span class="text">Score:</span>  
</div>  
...</body>
```

**CSS**

```css
body{   
  counter-reset: **corona**;   
}input[type="radio"]:checked{  
    counter-increment: **corona**;  
}.sum::after{  
  content: counter(**corona**) "/100";  
}
```

[Playable CodePen Example](https://codepen.io/elad2412/pen/d084e34a3f2a3b98c5a28b1cc0f96f5d)

## Creating the Countdown Clock

In this example, I create the countdown with CSS Animation and with the pseudo-elements `::before` & `::after`. Every pseudo-element represents one of the numbers of the countdown. The `::before` pseudo-element represents the counter of every ten seconds, and the second number represents every second. This way, I can create a counter that counts from 99 to 0 seconds. I gave each one of the pseudo-elements an animation on the `content` property value, that runs from 9 to 0.

![](https://miro.medium.com/v2/resize:fit:325/1*F1XBWVPvJOrob6z8dwptBg.gif)

CSS Pure Countdown

They both got the same animation but with a different timeline and different amount of iterations. The `::before` is iterating one time for 100 seconds. The `::after` is iterating ten times of 10 seconds each. In this way, It looks like the counter is running from 99 to 0.

For it to work well, I used the `animation-timing-function` property with a value of `step-end` , which creates precise jumping of the seconds. The second important thing is to use the `animation-fill-mode` property with the value of `forwards`. This way, when the animation will end, it will remain at the end.

**HTML**

```html
<div class="count-down">  
  <span class="icon">⏲️</span>  
  <span class="count">  
    ::before (pseudo element)  
    ::after  (pseudo element)  
  </span>  
</div>
```

**CSS**

```css
.count-down{  
  .count{  
    &::before{  
      content: "";   
      animation:**countdown** 100s step-end 1 forwards;  
    }      
    &::after{  
      content: "";   
      animation: **countdown** 10s step-end 10 forwards;  
    }    
  }   
}[@keyframes](http://twitter.com/keyframes) **countdown** {  
  0%  { content: "9" }  
  10% { content: "8" }  
  20% { content: "7" }  
  30% { content: "6" }  
  40% { content: "5" }  
  50% { content: "4" }  
  60% { content: "3" }  
  70% { content: "2" }  
  80% { content: "1" }  
  90%, 100% { content: "0" }  
}
```

**Note:** CSS step animation function isn’t supported in the Safari browser.

[Playable CodePen Example](https://codepen.io/elad2412/pen/6bb1857414ee6abb9ea4c554c400f625)

## Creating a Visual Timer

Because of the lack of support of the regular countdown in Safari, I created another visual timer, which will show as well.

![](https://miro.medium.com/v2/resize:fit:338/1*BrAjH44-19aji8TBFC-wUw.gif)

Pure CSS Visual Timer

This timer has fixed `width` and `height`. To show the status of the timeline, I used the pseudo-element `::before`, which will update in `width` with the animation property according to the time that has passed.

**HTML**

```html
<div class="timer">  
  ::before  
</div>
```

**CSS**

```css
.timer {  
  width: 150px;   
  height: 10px;   
  background-color: #fff;   
  border: solid 1px #333;&::before{  
    content: "";  
    width: 0px;   
    height: 10px;   
    display: block;   
    background-color: #ccc;animation: **timer** 100s linear 0s;   
    animation-fill-mode: forwards;  
  }  
}[@keyframes](http://twitter.com/keyframes) **timer**{  
  0%{width: 3%;}  
  85%{width: 85%; background-color: #ccc;}  
  90%{width: 90%; background-color: red;}  
  100%{width: 100%; background-color: red;}  
}
```

[Playable CodePen Example](https://codepen.io/elad2412/pen/936fd38d75035d7fa123d88c71fe167e)

## Schedule the Game-Over Curtain

The game-over curtain is placed inside the `class=”game-frame”` with `height: 0` and `opacity: 0` and `overflow: hidden`. The animation of the curtain updates the styles to `height: 100vh` and `opacity:1`.

![](https://miro.medium.com/v2/resize:fit:750/1*SFvhdrPgwuQTPjvFFndLdA.gif)

Schedule Game Over Curtain

Now, the trick here is to give the curtain `animation-delay` according to the time of the game, for example: `animation-delay: 100s` . This way, the curtain will appear only after 100 seconds. Besides, we will give the animation `animation-fill-mode: forwards` because we are aiming that when the animation ends, it will remain at the end of the timeline**.**

**HTML**

```html
<section class="game-frame">...HTML of Viruses  
   ...HTML of Countdown & Timer<section class="**game-over**">  
     <h2 class="game-title">Game Over</h2>  
   </section></section>
```

**CSS**

```css
.game-over{  
  height: 0;  
  overflow: hidden;  
  opacity: 0;position:fixed; z-index:20;  
  left:0; right:0; top:0;background-color: rgba(0, 0, 0, 0.6);animation: **curtain** 0.6s ease-in;       
  **animation-delay: 100s;   
  animation-fill-mode: forwards;**  
}[@keyframes](http://twitter.com/keyframes) **curtain** {  
  from{height:100vh; opacity:0;}  
  to {height:100vh; opacity:1;}  
}
```

[Playable CodePen Example](https://codepen.io/elad2412/pen/5686b88f096860318e9a8bfe38201e64)

## To Summarize

In this article, I demonstrated the most important tips & tricks for how to create pure CSS games by breaking apart most of its logic.

## My new talk on How to Create Pure CSS Games

This is my full talk on how to create Pure CSS Games :

from DevFest Nantes, France — October 21st, 2021

## Final Words

I hope that in this article, I gave you some inspiration on how to create pure CSS games, and maybe even inspire you to create your own pure CSS game.

If you like this article, I would appreciate applause and sharing 🙂.

You can follow me via [**Twitter**](https://twitter.com/eladsc) and [**Linkedin**](https://www.linkedin.com/in/eladshechter/)**.  
**Besides, you can find more of my content on [my website — **eladsc.com**](https://eladsc.com/).

**Who Am I?**  
**I am Elad Shechter**, a Web Developer specializing in CSS & HTML design and architecture.

![](https://miro.medium.com/v2/resize:fit:875/0*Oib822PXe_ibUZ2E.jpeg)

[**My Full Pure CSS Game — Coronavirus Invaders CodePen**](https://codepen.io/elad2412/pen/wvabjXy)**:**

[Coronavirus Invaders — CSS Pure Game (No JS!)](https://codepen.io/elad2412/pen/wvabjXy)

