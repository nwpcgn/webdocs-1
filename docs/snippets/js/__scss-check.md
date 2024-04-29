---
title: Checkbox Toggle
sidebar_position: 14
sidebar_label: ToggleScss
---

# SCSS Gameboy


## Content List



```json title="content-list.json"
{
    "contList": [{
        "slug": "gb-main",
        "name": "Main Gb",
        "trig": "trigger-gb-main",
        "cont": "content-gb-main",
        "type": "checkbox"
    }, {
        "slug": "gb1",
        "name": "GB 1",
        "trig": "trigger-gb1",
        "cont": "content-gb1"
    }, {
        "slug": "gb2",
        "name": "GB 2",
        "trig": "trigger-gb2",
        "cont": "content-gb2"
    }, {
        "slug": "gb3",
        "name": "GB 3",
        "trig": "trigger-gb3",
        "cont": "content-gb3"
    }, {
        "slug": "gb4",
        "name": "GB 4",
        "trig": "trigger-gb4",
        "cont": "content-gb4"
    }, {
        "slug": "gb5",
        "name": "GB 5",
        "trig": "trigger-gb5",
        "cont": "content-gb5"
    }]
}
```


## JS Generate


```javascript title="content-list.js"
let sd = [{
        slug: 'gb-main',
        name: 'Main Gb',
        trig: 'trigger-gb-main',
        cont: 'content-gb-main',
        type: 'checkbox'
    },
    {
        slug: 'gb1',
        name: 'GB 1',
        trig: 'trigger-gb1',
        cont: 'content-gb1'
    },
    {
        slug: 'gb2',
        name: 'GB 2',
        trig: 'trigger-gb2',
        cont: 'content-gb2'
    },
    {
        slug: 'gb3',
        name: 'GB 3',
        trig: 'trigger-gb3',
        cont: 'content-gb3'
    },
    {
        slug: 'gb4',
        name: 'GB 4',
        trig: 'trigger-gb4',
        cont: 'content-gb4'
    },
    {
        slug: 'gb5',
        name: 'GB 5',
        trig: 'trigger-gb5',
        cont: 'content-gb5'
    }
]
const createScssList = (d) =>
    `$inputToggleClassNamesX: (${sd
		.map((d) => `'${d.trig}' '${d.cont}' ${d.type ? `'true'` : ``}`)
		.join(',')});`
```


### SCSS


```scss title="content-list.scss"
$inputToggleClassNames: ('trigger-block-main''content-block-main''true',
    'trigger-block1''content-block1',
    'trigger-block2''content-block2',
    'trigger-block3''content-block3',
    'trigger-block4''content-block4',
    'trigger-block5''content-block5'
);
$duration: 0.9s;
$height: 256px;

:root {
    --gb-win-dur: #{$duration};
    --gb-menu-height-1: #{$height};
}

@mixin contHeight($typeCheckbox: false) {
    @if $typeCheckbox {
        min-height: calc($height / 4);
        height: auto;
    }

    @else {
        min-height: $height;
        height: auto;
    }
}

@each $triggerClassName,
$contentClassName,
$typeCheckbox in $inputToggleClassNames {
    .#{$triggerClassName}~.#{$contentClassName} {
        position: relative;
        opacity: 0;
        visibility: hidden;
        height: 0px;
        overflow: hidden;
        transition: opacity var(--gb-win-dur) ease,
            visibility 0s var(--gb-win-dur), height var(--gb-win-dur) ease;
    }

    .#{$triggerClassName}:checked~.#{$contentClassName} {
        opacity: 1;
        visibility: visible;
        @include contHeight($typeCheckbox);
        transition-delay: 0s, 0s, var(--gb-win-dur);
    }
}
```