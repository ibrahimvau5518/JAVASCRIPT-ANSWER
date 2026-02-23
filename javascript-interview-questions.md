# JavaScript Interview Questions

---

## JavaScript Basics

### 1. What is JavaScript and where is it executed?

JavaScript is a lightweight, interpreted programming language primarily used to make web pages interactive. Here's where it runs:

- **In the browser** — every modern browser has a built-in JavaScript engine (Chrome uses V8, Firefox uses SpiderMonkey, Safari uses JavaScriptCore)
- **On the server** — thanks to Node.js, JavaScript can also run outside the browser on the backend
- **In other environments** — desktop apps (Electron), mobile apps (React Native), and IoT devices

> *"Think of HTML as the skeleton, CSS as the skin, and JavaScript as the muscles that make everything move."*

---

### 2. What are the primitive data types in JavaScript?

JavaScript has **7 primitive types** — and knowing them cold is a must for interviews:

- **String** — `"hello"`
- **Number** — `42`, `3.14`, `Infinity`, `NaN`
- **BigInt** — `9007199254740991n` (for integers beyond `Number.MAX_SAFE_INTEGER`)
- **Boolean** — `true` or `false`
- **undefined** — a variable declared but not assigned
- **null** — intentional absence of value
- **Symbol** — a unique, immutable identifier

Everything else (arrays, functions, objects) is a **non-primitive** (object type).

---

### 3. What is the difference between `var`, `let`, and `const`?

A classic. Here's how they stack up:

- **`var`** — function-scoped, hoisted, can be re-declared and updated. Avoid in modern code.
- **`let`** — block-scoped, not re-declarable, can be updated. Use when the value changes.
- **`const`** — block-scoped, not re-declarable, cannot be reassigned. Use by default.

Key gotcha:
- `const` doesn't make objects immutable — you can still mutate the object's properties; you just can't reassign the variable itself.

```js
const obj = { name: "Alice" };
obj.name = "Bob";   // ✅ works fine
obj = {};           // ❌ TypeError
```

---

### 4. Explain hoisting in JavaScript.

Hoisting is JavaScript's behavior of moving declarations to the top of their scope before code executes. But it's more nuanced than that:

- **`var` declarations** are hoisted and initialized to `undefined`
- **`let` and `const`** are hoisted but NOT initialized — accessing them before their declaration throws a `ReferenceError` (this is the "Temporal Dead Zone")
- **Function declarations** are fully hoisted — you can call them before they appear in the code
- **Function expressions and arrow functions** are NOT hoisted (only the variable declaration is)

```js
console.log(x); // undefined (not an error)
var x = 5;

sayHi(); // ✅ works
function sayHi() { console.log("Hi!"); }

greet(); // ❌ ReferenceError
const greet = () => console.log("Hey!");
```

---

### 5. What is the difference between `==` and `===`?

- **`==`** (loose equality) — compares values **after type coercion**
- **`===`** (strict equality) — compares values **and types**, no coercion

```js
0 == "0"   // true  (coercion: string "0" → number 0)
0 === "0"  // false (different types)
null == undefined  // true
null === undefined // false
```

> *"In interviews and in production: always use `===` unless you have a very specific reason not to."*

---

### 6. What is `undefined` vs `null`?

They're both "nothing" — but with different intent:

- **`undefined`** — JavaScript sets this automatically when a variable is declared but not assigned, or a function returns nothing
- **`null`** — set explicitly by the developer to indicate "no value here on purpose"

```js
let x;        // undefined (JS assigned it)
let y = null; // null (developer assigned it intentionally)
```

Fun quirk: `typeof null === "object"` — this is a long-standing bug in JavaScript.

---

### 7. What is `NaN`? How do you check if a value is NaN?

- **`NaN`** stands for "Not a Number" — it's the result of invalid numeric operations like `"abc" / 2` or `parseInt("hello")`
- It's a `Number` type, but it's the only value in JavaScript that is **not equal to itself**: `NaN === NaN` is `false`

To check for NaN:
- Use `Number.isNaN(value)` — the reliable modern way (doesn't coerce)
- Avoid the global `isNaN()` — it coerces its argument first, leading to surprises

```js
isNaN("hello")        // true — coerces "hello" to NaN first
Number.isNaN("hello") // false — "hello" is a string, not NaN
Number.isNaN(NaN)     // true ✅
```

---

### 8. What is a JavaScript object?

An object is a collection of **key-value pairs** (properties). Keys are strings (or Symbols), and values can be anything — including other objects and functions.

- Objects are the foundation of JavaScript — arrays, functions, and even `null` have object-related behavior
- Created with object literals `{}`, `new Object()`, or `Object.create()`
- Properties are accessed with dot notation (`obj.name`) or bracket notation (`obj["name"]`)

```js
const person = {
  name: "Alice",
  greet() { return `Hi, I'm ${this.name}`; }
};
```

---

### 9. What is an array and how is it different from an object?

An array is a **special type of object** for storing ordered, indexed data. Here's how they differ:

- **Arrays** use numeric indices (`arr[0]`, `arr[1]`), have a `length` property, and come with built-in methods like `push`, `map`, `filter`
- **Objects** use named keys, no inherent order guarantee (though modern engines preserve insertion order), and have no `length`

```js
typeof []  // "object" — arrays ARE objects
Array.isArray([]) // true — the right way to check
```

---

### 10. What is the `typeof` operator?

`typeof` returns a string indicating the type of a value. Useful, but with a few gotchas:

- `typeof 42` → `"number"`
- `typeof "hi"` → `"string"`
- `typeof true` → `"boolean"`
- `typeof undefined` → `"undefined"`
- `typeof null` → `"object"` ← **the famous bug**
- `typeof []` → `"object"` ← use `Array.isArray()` instead
- `typeof function(){}` → `"function"` ← functions are special objects

---

### 11. What is a function in JavaScript?

A function is a reusable block of code designed to perform a specific task. In JavaScript, functions are **first-class citizens** — they can be:

- Assigned to variables
- Passed as arguments to other functions
- Returned from other functions
- Stored in arrays or objects

```js
function add(a, b) { return a + b; }
const result = add(2, 3); // 5
```

---

### 12. What is the difference between function declaration and function expression?

- **Function declaration** — uses the `function` keyword at the statement level; fully hoisted
- **Function expression** — assigns a function to a variable; only the variable is hoisted, not the function itself

```js
// Declaration — hoisted, callable before definition
function greet() { return "Hello"; }

// Expression — NOT hoisted
const greet = function() { return "Hello"; };

// Named function expression
const greet = function sayHello() { return "Hello"; };
```

---

### 13. What is an arrow function?

Arrow functions are a concise syntax for writing function expressions, introduced in ES6. Key differences from regular functions:

- **No `this` binding** — they inherit `this` from the surrounding lexical scope
- **No `arguments` object**
- **Cannot be used as constructors**
- **Implicit return** for single expressions (no curly braces needed)

```js
const add = (a, b) => a + b;
const square = n => n * n;
const greet = () => "Hello!";
```

---

### 14. How does JavaScript handle type coercion?

Type coercion is JavaScript's automatic conversion of values from one type to another. There are two kinds:

- **Implicit coercion** — happens automatically during operations like `+`, `==`, or `if` checks
- **Explicit coercion** — done manually with `Number()`, `String()`, `Boolean()`, etc.

Common surprises:
```js
"5" + 3     // "53" — number 3 coerced to string
"5" - 3     // 2   — string "5" coerced to number
[] + {}     // "[object Object]"
{} + []     // 0   — depends on context!
```

---

### 15. What is a template literal?

Template literals (backtick strings) allow embedded expressions and multi-line strings:

- Use backticks instead of quotes: `` ` ``
- Embed expressions with `${expression}`
- Support multi-line strings without `\n`
- Enable tagged templates for advanced string processing

```js
const name = "Alice";
const msg = `Hello, ${name}! 2 + 2 = ${2 + 2}`;
```

---

### 16. What are truthy and falsy values?

In a boolean context (like `if`), every value is either truthy or falsy. There are only **6 falsy values**:

- `false`
- `0` (and `-0`, `0n`)
- `""` (empty string)
- `null`
- `undefined`
- `NaN`

Everything else is **truthy** — including `[]`, `{}`, `"false"`, and `"0"`.

```js
if ([]) console.log("truthy!"); // ✅ prints — empty array is truthy
```

---

### 17. What is the difference between `slice`, `splice`, and `substring`?

These are commonly confused:

- **`slice(start, end)`** — works on arrays AND strings; non-mutating; returns a copy of the section
- **`splice(start, deleteCount, ...items)`** — arrays only; **mutating**; removes/replaces/inserts elements in place
- **`substring(start, end)`** — strings only; similar to `slice` but treats negative indices as `0`

```js
const arr = [1, 2, 3, 4, 5];
arr.slice(1, 3)           // [2, 3] — original unchanged
arr.splice(1, 2)          // removes [2, 3] from arr — arr is now [1, 4, 5]

"hello".slice(1, 3)       // "el"
"hello".substring(1, 3)   // "el"
"hello".slice(-2)         // "lo"
"hello".substring(-2)     // "hello" — negative treated as 0
```

---

### 18. What is the `this` keyword?

`this` refers to the object that is currently executing the code. Its value depends on **how** the function is called, not where it's defined:

- In a method: `this` = the object the method belongs to
- In a regular function (non-strict): `this` = the global object (`window` in browsers)
- In strict mode: `this` = `undefined`
- In an arrow function: `this` = inherited from the enclosing lexical scope
- With `new`: `this` = the newly created object

---

### 19. What is the difference between `null` and `undefined` in practical use?

- Use **`undefined`** to mean "not yet assigned" or "this property doesn't exist" — let JS handle it naturally
- Use **`null`** to explicitly communicate "this value is intentionally empty" — like a database field with no value yet

In APIs and codebases, `null` is a deliberate signal; `undefined` often indicates something wasn't set up yet.

---

### 20. What is strict mode (`"use strict"`)?

Strict mode makes JavaScript run in a stricter variant that:

- Eliminates some silent errors by throwing exceptions instead
- Prevents use of undeclared variables
- Disallows duplicate parameter names
- Makes `this` `undefined` in regular functions (instead of defaulting to global)
- Forbids writing to read-only properties

```js
"use strict";
x = 5; // ❌ ReferenceError — x is not declared
```

ES6 modules are always in strict mode by default.

---

## JavaScript Core Concepts

### 21. What is scope in JavaScript?

Scope determines **where variables are accessible** in your code. JavaScript has three main scopes:

- **Global scope** — accessible everywhere
- **Function scope** — variables declared inside a function are only accessible within it
- **Block scope** — variables declared with `let`/`const` inside `{}` are block-scoped

---

### 22. What is block scope vs function scope?

- **Function scope** (`var`) — the variable lives for the entire function, regardless of which block it's declared in
- **Block scope** (`let`, `const`) — the variable only exists within the enclosing `{}` block (like an `if` or `for` block)

```js
function example() {
  if (true) {
    var x = 1;   // function-scoped — accessible outside the if
    let y = 2;   // block-scoped — only accessible inside the if
  }
  console.log(x); // 1 ✅
  console.log(y); // ❌ ReferenceError
}
```

---

### 23. What is a closure?

A closure is when a function **remembers the variables from its outer scope** even after that outer function has finished executing. It's one of the most powerful (and interview-tested) concepts in JavaScript.

- Every function in JavaScript forms a closure over its outer scope
- Useful for data privacy, factory functions, and maintaining state

```js
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
counter(); // 1
counter(); // 2
counter(); // 3
// `count` is private — can't access it directly
```

---

### 24. What is the execution context?

The execution context is the environment in which JavaScript code is evaluated and executed. Every time a function runs, a new execution context is created. It contains:

- **Variable Environment** — all variables and function declarations in scope
- **Scope Chain** — links to outer scopes
- **`this` binding** — the value of `this` for that context

There are two types: the **Global Execution Context** (created once when the script runs) and **Function Execution Contexts** (created each time a function is called).

---

### 25. What is the call stack?

The call stack is a data structure that tracks the **currently executing functions**. It works LIFO (Last In, First Out):

- When a function is called, it's pushed onto the stack
- When it returns, it's popped off
- If the stack overflows (e.g., infinite recursion), you get a "Maximum call stack size exceeded" error

```
main()         ← bottom
  ↳ fetchData()
      ↳ parseJSON()  ← currently executing (top of stack)
```

---

### 26. What is the event loop?

The event loop is what allows JavaScript to be **non-blocking** despite being single-threaded. It continuously checks:

- Is the **call stack** empty?
- If yes, are there callbacks in the **task queue** (macrotasks) or **microtask queue**?
- Microtasks (Promises, `queueMicrotask`) run **before** macrotasks (`setTimeout`, `setInterval`)

This is why `setTimeout(fn, 0)` doesn't run immediately — it waits for the stack to clear.

---

### 27. What is the difference between synchronous and asynchronous code?

- **Synchronous** — code executes line-by-line; each operation must complete before the next begins
- **Asynchronous** — some operations are offloaded (I/O, timers, network requests) and their callbacks run later, allowing other code to continue in the meantime

```js
// Synchronous
console.log("1");
console.log("2"); // waits for above

// Asynchronous
console.log("1");
setTimeout(() => console.log("2"), 0);
console.log("3");
// Output: 1, 3, 2
```

---

### 28. What are promises?

A Promise is an object representing the eventual **completion or failure** of an asynchronous operation. It's a cleaner alternative to callbacks:

- Created with `new Promise((resolve, reject) => { ... })`
- Consumed with `.then()`, `.catch()`, and `.finally()`
- Chainable — each `.then()` returns a new promise

```js
fetch("/api/data")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

### 29. What are the states of a Promise?

A Promise has exactly three states — and it can only move forward, never backward:

- **Pending** — initial state; the operation is still in progress
- **Fulfilled** — the operation completed successfully; `resolve()` was called
- **Rejected** — the operation failed; `reject()` was called

Once settled (fulfilled or rejected), a promise's state and value are immutable.

---

### 30. What is `async`/`await`?

`async`/`await` is syntactic sugar over Promises that makes asynchronous code look and read like synchronous code:

- Mark a function with `async` — it always returns a Promise
- Use `await` inside to pause execution until a Promise resolves
- Much easier to read and reason about than chained `.then()` calls

```js
async function loadUser(id) {
  const res = await fetch(`/users/${id}`);
  const user = await res.json();
  return user;
}
```

---

### 31. How does error handling work in `async`/`await`?

Since `await` can throw if a Promise rejects, you wrap it in a standard `try`/`catch`:

```js
async function loadUser(id) {
  try {
    const res = await fetch(`/users/${id}`);
    if (!res.ok) throw new Error("Not found");
    return await res.json();
  } catch (err) {
    console.error("Failed to load user:", err.message);
  }
}
```

- You can also `.catch()` on the returned Promise from the async function
- Without error handling, rejected promises inside `async` functions become unhandled rejections

---

### 32. What is callback hell?

Callback hell (also called the "pyramid of doom") is deeply nested callbacks that make code hard to read, debug, and maintain:

```js
getData(function(a) {
  getMoreData(a, function(b) {
    getEvenMore(b, function(c) {
      doSomething(c, function(d) {
        // 😱
      });
    });
  });
});
```

Solutions:
- **Named functions** — break out callbacks into named functions
- **Promises** — chain `.then()` calls
- **`async`/`await`** — the modern, cleanest solution

---

### 33. What is the difference between `setTimeout` and `setInterval`?

- **`setTimeout(fn, delay)`** — runs `fn` **once** after `delay` milliseconds
- **`setInterval(fn, delay)`** — runs `fn` **repeatedly** every `delay` milliseconds until cleared

```js
const id = setInterval(() => console.log("tick"), 1000);
clearInterval(id); // stops it
```

Important: both are macrotasks processed by the event loop, so the delay is a **minimum** — not a guarantee.

---

### 34. What are higher-order functions?

A higher-order function is one that either:

- **Takes a function as an argument**, or
- **Returns a function**

This is possible because functions are first-class citizens in JavaScript.

```js
// Takes a function
[1, 2, 3].map(x => x * 2); // [2, 4, 6]

// Returns a function
function multiplier(factor) {
  return (num) => num * factor;
}
const double = multiplier(2);
double(5); // 10
```

---

### 35. Explain `map`, `filter`, and `reduce`

These are the three workhorses of functional array operations:

- **`map(fn)`** — transforms each element; returns a new array of the **same length**
- **`filter(fn)`** — keeps elements where `fn` returns `true`; returns a **subset**
- **`reduce(fn, initialValue)`** — accumulates all elements into a **single value**

```js
const nums = [1, 2, 3, 4, 5];

nums.map(n => n * 2);             // [2, 4, 6, 8, 10]
nums.filter(n => n % 2 === 0);    // [2, 4]
nums.reduce((sum, n) => sum + n, 0); // 15
```

---

### 36. What is immutability?

Immutability means a value **cannot be changed after it's created**. Instead of mutating, you create new values.

- Primitives in JavaScript are inherently immutable
- Objects and arrays are mutable by default — you have to be intentional
- Immutability makes code more predictable, easier to test, and avoids hard-to-track bugs

```js
// Mutable ❌
const arr = [1, 2, 3];
arr.push(4); // mutates original

// Immutable ✅
const newArr = [...arr, 4]; // creates a new array
```

---

### 37. What is shallow copy vs deep copy?

- **Shallow copy** — copies the top-level structure, but nested objects/arrays still point to the same reference
- **Deep copy** — recursively copies everything; completely independent

```js
const obj = { a: 1, nested: { b: 2 } };

// Shallow copy
const shallow = { ...obj };
shallow.nested.b = 99; // also mutates obj.nested.b!

// Deep copy
const deep = JSON.parse(JSON.stringify(obj)); // simple but has limits
const deep2 = structuredClone(obj); // modern, handles more types
```

---

### 38. What is the spread operator?

The spread operator (`...`) expands an iterable (array, string, object) into individual elements. Common uses:

- **Copying arrays/objects** — `const copy = [...arr]`
- **Merging** — `const merged = { ...obj1, ...obj2 }`
- **Spreading into function arguments** — `Math.max(...[1, 2, 3])`
- **Creating new arrays** — `const newArr = [...arr, 4, 5]`

```js
const a = [1, 2];
const b = [3, 4];
const combined = [...a, ...b]; // [1, 2, 3, 4]
```

---

### 39. What is destructuring?

Destructuring is a shorthand syntax to extract values from arrays or objects into variables:

```js
// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];
// first = 1, second = 2, rest = [3, 4, 5]

// Object destructuring
const { name, age, city = "Unknown" } = { name: "Alice", age: 30 };
// city defaults to "Unknown"

// In function parameters
function greet({ name, role = "user" }) {
  return `Hello ${name}, you're a ${role}`;
}
```

---

### 40. What is optional chaining (`?.`)?

Optional chaining lets you safely access deeply nested properties **without throwing** if an intermediate value is `null` or `undefined`. Instead of an error, it returns `undefined`:

```js
const user = { profile: { address: { city: "NY" } } };

// Old way — verbose and error-prone
const city = user && user.profile && user.profile.address && user.profile.address.city;

// With optional chaining
const city = user?.profile?.address?.city; // "NY"
const zip  = user?.profile?.address?.zip;  // undefined (no error!)

// Also works on methods and arrays
user?.getRole?.();
arr?.[0];
```

---

## Intermediate & Practical JavaScript

### 41. What is debouncing?

Debouncing delays execution of a function until **after a specified time has passed since it was last called**. Great for events that fire rapidly:

- Search input (don't hit the API on every keystroke — wait until the user stops typing)
- Window resize handlers

```js
function debounce(fn, delay) {
  let timer;
  return function(...args) {
    clearTimeout(timer);
    timer = setTimeout(() => fn.apply(this, args), delay);
  };
}

const search = debounce((query) => fetchResults(query), 300);
input.addEventListener("input", e => search(e.target.value));
```

---

### 42. What is throttling?

Throttling ensures a function is called **at most once per specified time interval**, no matter how many times it's triggered:

- Scroll handlers (don't run on every pixel)
- Button click rate limiting
- API rate limiting on the frontend

```js
function throttle(fn, limit) {
  let lastCall = 0;
  return function(...args) {
    const now = Date.now();
    if (now - lastCall >= limit) {
      lastCall = now;
      fn.apply(this, args);
    }
  };
}
```

> *"Debounce = wait for quiet. Throttle = pace yourself."*

---

### 43. What is memoization?

Memoization is an optimization technique where you **cache the results of expensive function calls** and return the cached result when the same inputs occur again:

- Trades memory for speed
- Only useful for **pure functions** (same input → always same output)

```js
function memoize(fn) {
  const cache = new Map();
  return function(n) {
    if (cache.has(n)) return cache.get(n);
    const result = fn(n);
    cache.set(n, result);
    return result;
  };
}

const factorial = memoize(n => n <= 1 ? 1 : n * factorial(n - 1));
```

---

### 44. What is currying?

Currying transforms a function that takes multiple arguments into a **sequence of functions, each taking one argument**:

- Enables partial application — fix some arguments in advance
- Promotes function reuse and composition

```js
// Normal
const add = (a, b) => a + b;

// Curried
const curriedAdd = a => b => a + b;
const add5 = curriedAdd(5);
add5(3); // 8
add5(10); // 15
```

---

### 45. What is event delegation?

Event delegation attaches **a single event listener to a parent element** to handle events from its children, using event bubbling:

- More efficient than attaching listeners to every child (especially for dynamic lists)
- Works because events bubble up from the target to its ancestors

```js
// Instead of adding a listener to every <li>:
document.querySelector("ul").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked:", e.target.textContent);
  }
});
```

---

### 46. How does `this` behave in arrow functions?

Arrow functions do **not have their own `this`** — they inherit it from the lexical (surrounding) scope at the time they're defined:

- This makes them great for callbacks inside methods where you need to keep the parent `this`
- But they're wrong for object methods where you need dynamic `this`

```js
const obj = {
  name: "Alice",
  greet: function() {
    setTimeout(() => {
      console.log(this.name); // "Alice" ✅ — arrow inherits obj's `this`
    }, 100);
  }
};
```

---

### 47. What is the difference between `bind`, `call`, and `apply`?

All three let you explicitly set `this` for a function:

- **`call(thisArg, arg1, arg2)`** — invokes the function **immediately** with individual args
- **`apply(thisArg, [argsArray])`** — invokes the function **immediately** with args as an **array**
- **`bind(thisArg, ...args)`** — returns a **new function** with `this` permanently bound (doesn't invoke)

```js
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const user = { name: "Alice" };

greet.call(user, "Hello", "!");   // "Hello, Alice!"
greet.apply(user, ["Hi", "?"]);   // "Hi, Alice?"
const boundGreet = greet.bind(user, "Hey");
boundGreet(".");                   // "Hey, Alice."
```

---

### 48. What is prototype inheritance?

In JavaScript, objects can inherit properties and methods from other objects via their **prototype**. When you access a property on an object:

- JavaScript first looks at the object itself
- If not found, it looks up the **prototype chain** until it finds it or reaches `null`

```js
function Animal(name) { this.name = name; }
Animal.prototype.speak = function() {
  return `${this.name} makes a sound.`;
};

const dog = new Animal("Rex");
dog.speak(); // "Rex makes a sound." — found on prototype
```

---

### 49. What is the prototype chain?

The prototype chain is the **linked series of objects** that JavaScript traverses when looking up a property. Every object has an internal `[[Prototype]]` link (accessible via `Object.getPrototypeOf()` or `__proto__`):

```
dog → Animal.prototype → Object.prototype → null
```

- If a property isn't found anywhere in the chain, you get `undefined`
- `Object.prototype` is at the top — it's where methods like `toString()` and `hasOwnProperty()` live

---

### 50. What are ES6 modules?

ES6 modules are a native module system that lets you split code into separate files with explicit imports and exports:

- **Named exports** — `export const foo = 1;` / `import { foo } from './module.js'`
- **Default exports** — `export default fn;` / `import fn from './module.js'`
- Modules are **statically analyzed** (imports resolved at parse time)
- They run in **strict mode** by default
- Each module has its own scope

---

### 51. Difference between CommonJS and ES modules

Two different module systems with different behavior:

- **CommonJS (`require` / `module.exports`)** — synchronous, used in Node.js, dynamic (you can require inside conditions), loaded at runtime
- **ES Modules (`import` / `export`)** — asynchronous-friendly, native in browsers and modern Node.js, static (analyzed at parse time), supports tree shaking

```js
// CommonJS
const fs = require("fs");
module.exports = { myFn };

// ES Module
import fs from "fs";
export { myFn };
```

---

### 52. How does garbage collection work in JavaScript?

JavaScript uses **automatic memory management** — you don't manually free memory. The engine does it via garbage collection:

- The most common algorithm is **mark-and-sweep**: the GC marks all values reachable from the root (global scope), then sweeps and frees everything that wasn't marked
- A value is eligible for collection when no references to it remain
- Modern engines (V8) use generational GC — short-lived objects (most objects) are collected quickly in the "young generation"

---

### 53. What causes memory leaks in JavaScript?

A memory leak occurs when **memory that's no longer needed isn't released**. Common causes:

- **Global variables** — accidentally creating globals (e.g., `x = 5` without `let/const`)
- **Forgotten event listeners** — especially on DOM elements that get removed
- **Closures** — holding references to large objects unnecessarily
- **Detached DOM nodes** — DOM elements removed from the tree but still referenced in JS
- **Timers not cleared** — `setInterval` with no `clearInterval`

---

### 54. How does JavaScript handle concurrency?

JavaScript is **single-threaded** — it can only do one thing at a time. But it handles concurrent work through:

- **The event loop** — offloads async operations (I/O, timers) to browser APIs or the OS, then processes callbacks when the call stack is free
- **Web Workers** — true parallel threads for CPU-intensive work, though they can't access the DOM
- **Async/await and Promises** — cooperative concurrency (tasks voluntarily yield)

---

### 55. What is a pure function?

A pure function always:

- Returns the **same output for the same input** (deterministic)
- Has **no side effects** (doesn't modify external state, no I/O, no mutation)

```js
// Pure ✅
const add = (a, b) => a + b;

// Impure ❌ — depends on external state
let total = 0;
const addToTotal = (n) => { total += n; return total; };
```

Pure functions are easy to test, cache, and reason about — they're the foundation of functional programming.

---

### 56. What is functional programming in JavaScript?

Functional programming (FP) is a paradigm that treats computation as the evaluation of mathematical functions. Core ideas in JavaScript FP:

- **Pure functions** — avoid side effects
- **Immutability** — don't mutate state; create new values
- **Higher-order functions** — `map`, `filter`, `reduce`, `compose`
- **Function composition** — building complex behavior from small, reusable functions
- **Avoiding shared state** — reduces bugs and race conditions

---

### 57. What is lazy loading?

Lazy loading defers loading of a resource until it's **actually needed**, rather than loading everything upfront:

- **Images** — load images only when they scroll into the viewport (`loading="lazy"` attribute or Intersection Observer)
- **JavaScript modules** — use dynamic `import()` to load code on demand
- **Routes in SPAs** — load page components only when the user navigates to them

```js
// Dynamic import (lazy loading a module)
button.addEventListener("click", async () => {
  const { heavyFeature } = await import("./heavyFeature.js");
  heavyFeature.run();
});
```

---

### 58. What is tree shaking?

Tree shaking is a **dead code elimination** technique used by bundlers (like Webpack, Rollup, Vite) to remove unused exports from your final bundle:

- Relies on ES Module static structure — bundlers can analyze which exports are imported
- Doesn't work with CommonJS (`require`) because it's dynamic
- Ensures you don't ship code that's never used, reducing bundle size

```js
// Only `add` is used — `subtract` gets tree-shaken out
import { add } from "./math.js";
```

---

### 59. What happens when you `await` a non-Promise value?

Awaiting a non-Promise value is perfectly valid — it's equivalent to `await Promise.resolve(value)`. It simply resolves immediately with that value:

```js
async function example() {
  const x = await 42; // same as await Promise.resolve(42)
  console.log(x);     // 42
}
```

This is useful when you have a function that might return either a Promise or a plain value.

---

### 60. How do you optimize JavaScript performance in a web application?

Performance optimization is a broad topic — here are the key strategies:

- **Reduce bundle size** — tree shaking, code splitting, lazy loading, and removing unused dependencies
- **Minimize DOM operations** — batch reads and writes; use `DocumentFragment` for bulk inserts
- **Debounce/throttle** event handlers — especially scroll, resize, and input events
- **Avoid memory leaks** — clean up event listeners, timers, and subscriptions
- **Use Web Workers** for CPU-heavy tasks to keep the main thread free
- **Cache expensive computations** with memoization
- **Prefer `requestAnimationFrame`** for animations over `setTimeout`
- **Profile first** — use browser DevTools (Performance tab, Memory tab) to find actual bottlenecks before optimizing

---

*Good luck with your interviews! The best way to internalize these concepts is to write code that uses them — not just read about them.*
