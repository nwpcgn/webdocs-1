---
title: "Number.toFixed()"
date: 2018-01-14T21:38:44-05:00
draft: false
noTitle: false
fullWidth: false
description: "Format a number to a fixed number of decimal places."
weight: 40
---

Format a number to a fixed number of decimal places. Pass in the number of decimal places as an argument.

```javascript
let pi = 3.14159;
let eleven = 11;

// returns 3.14
pi.toFixed(2);

// returns 11.000
eleven.toFixed(3);
```