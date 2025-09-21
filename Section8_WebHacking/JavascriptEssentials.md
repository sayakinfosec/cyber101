## JavaScript Essentials

### 🔹 Essential Concepts

**Variables**  
Variables are containers for storing values. In JavaScript, we declare them using:  
- `var` → function-scoped  
- `let` → block-scoped  
- `const` → block-scoped & immutable  

```js
var x = 10;
let y = 20;
const z = 30;
````

---

**Data Types**

* `string` → "Hello"
* `number` → 42
* `boolean` → true / false
* `null` → intentional empty value
* `undefined` → declared but not assigned
* `object` → arrays, objects, complex structures

---

**Functions**
Functions are reusable blocks of code. Example:

```html
<script>
    function PrintResult(rollNum) {
        alert("Username with roll number " + rollNum + " has passed the exam");
    }

    for (let i = 0; i < 100; i++) {
        PrintResult(rollNumbers[i]);
    }
</script>
```

---

**Loops**
Used to repeat tasks.

```js
// Function
function PrintResult(rollNum) {
    alert("Username with roll number " + rollNum + " has passed the exam");
}

// Loop calls PrintResult 100 times
for (let i = 0; i < 100; i++) {
    PrintResult(rollNumbers[i]);
}
```

**Q:** What term allows you to run a code block multiple times as long as a condition?
**A:** loop ✅

---

**Request-Response Cycle**

* Browser (client) → sends request
* Server → responds with HTML, CSS, JS, or data

---

### 🔹 JavaScript Overview

JavaScript is **interpreted** (runs directly in the browser).

```js
// Hello World
console.log("Hello, World!");

// Variable
let age = 25;

// Control flow
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}

// Function
function greet(name) {
    console.log("Hello, " + name + "!");
}

greet("Bob");
```

Test in **Chrome Console** (`Ctrl + Shift + I → Console`):

```js
let x = 5;
let y = 10;
let result = x + y;
console.log("The result is: " + result);
```

**Q:** If `x=10`, what is the output?
**A:** The result is: 20 ✅

**Q:** JavaScript is compiled or interpreted?
**A:** Interpreted ✅

---

### 🔹 Integrating JavaScript in HTML

**Internal JS**
Placed inside `<script>` tags within HTML.

```html
<!DOCTYPE html>
<html>
<head>
    <title>Internal JS</title>
</head>
<body>
    <h1>Addition of Two Numbers</h1>
    <p id="result"></p>

    <script>
        let x = 5, y = 10;
        let result = x + y;
        document.getElementById("result").innerHTML = "The result is: " + result;
    </script>
</body>
</html>
```

**External JS**
Code is stored in `.js` file and linked via `src`.

```html
<script src="script.js"></script>
```

**Q:** Which type places code directly inside HTML?
**A:** Internal ✅

**Q:** Which is better for reuse across multiple pages?
**A:** External ✅

**Q:** Name of external JS file used by `external_test.html`?
**A:** thm\_external.js ✅

**Q:** What attribute links an external JS file?
**A:** src ✅

---

### 🔹 Abusing Dialogue Functions

* **alert("Hi")** → Shows message box
* **prompt("What is your name?")** → Asks input
* **confirm("Are you sure?")** → OK / Cancel

Example malicious HTML:

```html
<!DOCTYPE html>
<html>
<head><title>Hacked</title></head>
<body>
<script>
    for (let i = 0; i < 3; i++) {
        alert("Hacked");
    }
</script>
</body>
</html>
```

**Q:** In `invoice.html`, how many times is "Hacked" shown?
**A:** 5 ✅

**Q:** Which JS element asks for input?
**A:** prompt ✅

**Q:** If user enters Tesla in `carName=prompt("...")`, what is stored?
**A:** Tesla ✅

---

### 🔹 Bypassing Control Flow Statements

**If-Else Example**

```html
<!DOCTYPE html>
<html>
<head><title>Age Verification</title></head>
<body>
    <h1>Age Verification</h1>
    <p id="message"></p>

    <script>
        age = prompt("What is your age")
        if (age >= 18) {
            document.getElementById("message").innerHTML = "You are an adult.";
        } else {
            document.getElementById("message").innerHTML = "You are a minor.";
        }
    </script>
</body>
</html>
```

**Q:** What if age < 18?
**A:** You are a minor. ✅

**Bypassing Login Forms**
Developers sometimes insecurely hardcode logic in JS → attackers can bypass.

**Q:** What is the password for user `admin`?
**A:** ComplexPassword ✅

---

### 🔹 Exploring Minified Files

**Minification** → removes spaces/comments for performance.
**Obfuscation** → makes code intentionally unreadable.

Example original:

```js
function hi() {
  alert("Welcome to THM");
}
hi();
```

After obfuscation → unreadable gibberish, but same functionality.

**Q:** What alert is shown in `hello.html`?
**A:** Welcome to THM ✅

**Q:** Value of age in code:

```js
age=0x1*0x247e+0x35*-0x2e+-0x1ae3;
```

**A:** 21 ✅

---

### 🔹 Best Practices

* ❌ Don’t rely only on **client-side validation** → always validate on server too.
* ❌ Don’t include **untrusted external libraries**.
* ❌ Don’t hardcode **secrets** in JS (API keys, tokens).

```js
// Bad practice
const privateAPIKey = 'pk_TryHackMe-1337';
```

* ✅ Minify & Obfuscate JS before production.

**Q:** Is it good practice to blindly include JS from any source?
**A:** nay ✅

