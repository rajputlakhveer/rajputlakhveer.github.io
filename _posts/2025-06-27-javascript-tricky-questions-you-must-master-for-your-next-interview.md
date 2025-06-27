---
layout: home
title: "JavaScript Tricky Questions You Must Master for Your Next Interview"
date: 2025-06-27
categories: "Javascript"
tags: [JavaScript, ReactJS, Question, Tricky, Answer, Interview]
image: 'https://github.com/user-attachments/assets/71d1e921-1efb-43b1-b149-4ea3a738b43b'
---

# 🧠 **JavaScript Tricky Questions You Must Master for Your Next Interview! 🚀**

JavaScript may *look* friendly, but under the hood, it's full of quirks that even experienced developers get stumped by! 😅 Whether you're prepping for your next big tech interview or want to level up your JS brain, here are some tricky and mind-bending questions with real-world examples. 💡

Let’s dive deep! 🏊‍♂️

![accitental-global-variables](https://github.com/user-attachments/assets/71d1e921-1efb-43b1-b149-4ea3a738b43b)

---

### 🔥 1. **What is the result of `[] + []` and why?**

**Answer:** `""` (an empty string)

```javascript
console.log([] + []); // ""
```

**Why?**

When using `+` with arrays, JavaScript tries to **convert both operands to strings** and then concatenate them.

```javascript
[].toString(); // ""
```

So essentially it becomes: `"" + "" => ""`

🤯 **Mind blown? It gets crazier!**

---

### 🌀 2. **What is the output of `[1,2,3] + [4,5,6]`?**

**Answer:** `"1,2,34,5,6"`

```javascript
console.log([1,2,3] + [4,5,6]); // "1,2,34,5,6"
```

JavaScript converts both arrays to strings:

```javascript
[1,2,3].toString(); // "1,2,3"
[4,5,6].toString(); // "4,5,6"
```

Then: `"1,2,3" + "4,5,6" => "1,2,34,5,6"`

📌 **Pro Tip:** Always double-check data types before combining!

---

### 🤖 3. **What does `typeof NaN` return?**

**Answer:** `"number"`

```javascript
console.log(typeof NaN); // "number"
```

Even though **NaN** stands for *Not a Number*, it's still considered a **number type** in JS.

🧠 **Takeaway:** Don’t rely solely on `typeof` for validation—use `Number.isNaN()`.

---

### 🕳️ 4. **What will be the result of this code?**

```javascript
let a = [1, 2, 3];
a[10] = 99;
console.log(a.length); // ?
```

**Answer:** `11`

Sparse arrays! JavaScript sets `a[10]`, leaving holes (undefineds) in between.

```javascript
// Array looks like:
[1, 2, 3, <7 empty items>, 99]
```

🧨 **Trap:** Don’t assume sequential indexes when working with arrays.

---

### 🧊 5. **Is `{} + []` the same as `[] + {}`?**

```javascript
console.log({} + []); // 0
console.log([] + {}); // "[object Object]"
```

**Why?**

In the first case, `{}` is treated as a block scope and `+[]` becomes `0`.

```javascript
+[] // 0
```

In the second case:

```javascript
[] + {} 
// "" + "[object Object]" => "[object Object]"
```

📘 **Lesson:** Operator precedence + expression context = head-scratcher 😵

---

### 🧪 6. **What is the output of `console.log(0.1 + 0.2 === 0.3)`?**

**Answer:** `false`

```javascript
console.log(0.1 + 0.2 === 0.3); // false
```

Due to floating-point precision errors in JS:

```javascript
0.1 + 0.2 = 0.30000000000000004
```

✅ **Fix:** Use a precision tolerance:

```javascript
Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON
```

---

### 🪝 7. **What does this return?**

```javascript
let foo = function() {};
console.log(foo.prototype); 
```

**Answer:** `{}`

Every function (not arrow function) has a `.prototype` object, used for inheritance when using `new`.

But arrow functions don’t have `.prototype`:

```javascript
const bar = () => {};
console.log(bar.prototype); // undefined
```

💡 **Note:** This is crucial when working with constructors and classes!

---

### 🐍 8. **What will `let x = (1, 2, 3);` return?**

**Answer:** `3`

```javascript
let x = (1, 2, 3);
console.log(x); // 3
```

Comma operator evaluates each expression but returns **only the last one**.

📌 Rare but useful in minified or inline expressions.

---

### 🧟 9. **Why does this return `'object'`?**

```javascript
console.log(typeof null); // "object"
```

**Answer:** This is a **legacy bug** from the early days of JS.

Internally, null was represented with a null pointer, which was `0`—but the type tag system also used `0` for "object."

So...

🧠 **Takeaway:** Use `value === null` to check for null explicitly.

---

### 🔒 10. **What does this return?**

```javascript
let a = {};
let b = { key: "b" };
let c = { key: "c" };

a[b] = 123;
a[c] = 456;

console.log(a[b]); // ??
```

**Answer:** `456`

🤯 **Why?** JS converts object keys to strings. So:

```javascript
b.toString() => "[object Object]"
c.toString() => "[object Object]"
```

So both `a[b]` and `a[c]` are actually `a["[object Object]"]`.

📌 **Fix:** Use `Map` if you want objects as keys:

```javascript
let map = new Map();
map.set(b, 123);
map.set(c, 456);
console.log(map.get(b)); // 123
```

---

### 👀 Wrapping Up…

Mastering these tricky questions gives you a huge edge in interviews! 🏆

✅ They test:

* Deep knowledge of JavaScript internals
* Awareness of quirks and bugs
* Practical debugging mindset
