# Express.js Views Directory Configuration Guide

When building applications with **Express** and a templating engine like **EJS**, you might encounter pathing issues if you start your application from a directory other than the root folder. 

This guide explains how to make your Express app robust so it can be safely executed from any terminal location using Node's built-in `path` module and `__dirname`.

---

## 🛑 The Problem: Working Directory vs. File Directory

By default, Express looks for your templates in a folder named `views` relative to the **current working directory** (`process.cwd()`). This is the folder you are actively standing in when you run `node` or `nodemon`.

* **Scenario A (Works):** You `cd` directly into the project folder and run `nodemon index.js`. Express looks inside the current folder for `/views`. Everything works.
* **Scenario B (Breaks):** You back out one level and run `nodemon project-folder/index.js`. The server starts, but visiting the page throws a `Failed to lookup view "home.ejs" in the views directory` error. Express is looking for the `views` folder in your parent directory instead of your project folder.

---

## 🛠️ The Solution: Using `__dirname` and `path.join()`

To fix this, you can hardcode the template location relative to `index.js` itself, rather than where the terminal command was executed.

### 💻 Code Implementation

Add the following configuration to your main application file (e.g., `index.js` or `app.js`):

```javascript
const express = require('express');
const app = express();
// 1. Require Node's built-in path module
const path = require('path'); 

// 2. Set your view engine (e.g., EJS)
app.set('view engine', 'ejs');

// 3. Dynamically point to the views folder relative to this file
app.set('views', path.join(__dirname, '/views')); 
```

---

## 🧩 Key Terminology

* **`process.cwd()`**: The directory you are currently in within your terminal. It changes depending on where you `cd`.
* **`__dirname`** (pronounced *Dunder Dirname*): A global variable in Node.js that always returns the absolute path of the directory where the *currently executing JavaScript file* lives. It never changes, no matter where you run the command from.
* **`path.join()`**: A utility method from Node's `path` module that glues separate path segments together. It automatically normalizes the path slashes (`/` vs `\`) so your application runs seamlessly across Windows, Mac, and Linux.
