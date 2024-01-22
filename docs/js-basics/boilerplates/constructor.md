---
title: "Constructor Pattern"
date: 2018-01-24T12:16:26-05:00
description: "Create multiple instances of script that share methods but contain unique information."
example: "a piggy bank library."
draft: false
weight: 30
noIndex: false
---

Change `MyLibrary` to whatever namespace you’d like to use for your library. Constructors start with a capital letter.

## Examples

```js
// Create new instances of the constructor
let dugg = new MyLibrary();
let kevin = new MyLibrary(4);

// Run methods
dugg.add(2);
kevin.subtract(1);
let total = dug.total;
```

## The Boilerplate

```js
/**
 * Constructor Pattern Boilerplate
 */
let MyLibrary = (function () {

	/**
	 * Create the Constructor object
	 * @param {Number} start The starting amount
	 */
	function Constructor (start = 0) {
		this.total = start;
	}

	/**
	 * Add money to the total
	 */
	Constructor.prototype.add = function (num = 1) {
		this.total = this.total + num;
	};

	/**
	 * Remove money from the total
	 */
	Constructor.prototype.subtract = function (num = 1) {
		this.total = this.total - num;
	};

	return Constructor;

})();
```