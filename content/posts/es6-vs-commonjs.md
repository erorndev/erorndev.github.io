---
title: "ES6 Modules vs CommonJS"
draft: false
date: 2025-08-14T14:12:47.295Z
tags: [javascript, nodejs, es6]
weight: 1
---

### Introduction
I think we are all familiar with this syntax:

```js
// math.js
function add(a, b) {
  return a + b;
}
module.exports = { add };

// main.js
const math = require('./math');
console.log(math.add(2, 3)); // 5
```

This snippet shows that `math.js` exports a function using `module.exports`, and `main.js` imports it using `require()`. In CommonJS, this is typically how you import/export modules. CommonJS has been the standard in Node.js for years, but JavaScript evolved, and a new module system emerged: ES6 Modules.

#### ES6 Modules
Let’s see the same example with ES6 syntax:

```js
// math.js
export function add(a, b) {
  return a + b;
}

// main.js
import { add } from './math.js';
console.log(add(2, 3)); // 5
```

Some might argue that the ES6 syntax is cleaner and easier to read, which I personally prefer as well.  
At first glance, they do the exact same thing — they let you split your code into pieces, but it’s really different under the hood.

#### Syntax
CommonJS uses `require()` and `module.exports`, while ES6 uses `import` and `export` keywords.  
At first glance, it seems like just a different way to write the same thing, right? **WRONG**.  
There’s more going on under the hood than just `require()` vs `import`. The way these modules load and handle exports is entirely different.

#### Loading differences
```js
// CommonJS dynamic require
if (process.env.KENDRICK_MUSTARD) {
  const mustard = require('./mustard');
  mustard.blud();
}
```

CommonJS modules are typically loaded at runtime. This means your code decides what to pull in while it’s running.  
It’s flexible, but Node.js can’t know ahead of time exactly what your program will need.

```js
// ES6 static import
import { blud } from './mustard.js';
blud();
```

ES6 is different because ES6 modules are statically analyzed. The JavaScript engine knows ahead of time exactly what will be imported and where.  
This allows tools like bundlers to optimize your code.

#### Default vs Named Exports
Another key difference between CommonJS and ES6 modules is how exports are handled. CommonJS only has `module.exports`, so you can export anything—an object, a function, or a class—but it’s usually a single default export:

```js
// math.js
function add(a, b) {
  return a + b;
}
module.exports = add;

// main.js
const add = require('./math');
console.log(add(2, 3));
```

ES6 allows both named and default exports:

```js
// math.js
export function add(a, b) {
  return a + b;
}

export default function subtract(a, b) {
  return a - b;
}

// main.js
import subtract, { add } from './math.js';
console.log(add(2, 3));      // 5
console.log(subtract(5, 2)); // 3
```

#### Interoperability
Sometimes you might need to mix CommonJS and ES6 modules:

```js
// CommonJS importing an ES6 module
const { add } = require('./math.mjs'); 

// ES6 importing a CommonJS module
import math from './math.cjs';
```

Node.js has improved support for this, though i wouldnt recommend. It’s best to pick one style per project if possible.

#### Choosing between ES6 and CommonJS
Which one should you use? Well, it depends:

1. **New Projects** – Go with ES6 modules. The syntax is clear, and most modern tutorials and libraries use ES6.
2. **Existing Node.js Projects** – Go with CommonJS. Migrating can be a headache, and CommonJS still works perfectly fine in Node.js.