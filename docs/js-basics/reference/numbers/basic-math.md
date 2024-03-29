---
title: "Basic math"
date: 2018-01-14T21:38:44-05:00
draft: false
noTitle: false
fullWidth: false
description: "Do basic arithmetic with JavaScript."
weight: 60
---

JavaScript includes a handful of basic arithmetic operators:

| Operator | Action    |
|----------|-----------|
| `+`      | Add       |
| `-`      | Subtract  |
| `*`      | Multiply  |
| `/`      | Divide    |
| `%`      | Remainder |

```javascript
// Add
// logs 4
console.log(2 + 2);

// Subtract
// logs 3
console.log(4 - 1);

// Divide
// logs 2
console.log(4 / 2);

// Multiply
// logs 10
console.log(5 * 2);

// Remainder
// logs 1 (2 goes into 5 twice, with 1 left over)
console.log(5 % 2);
```

You can combine operators. Group the items you want to run first/together with parentheses.

```javascript
let total = (3 + 1) * 2;

// logs 8 (3 plus 1 is 4, multiplied by 2)
console.log(total);
```