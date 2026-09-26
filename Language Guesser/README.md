# Language Guesser 🌍

A simple command-line utility built with **Node.js** that accepts a string of text in any language and accurately predicts what language it belongs to. 

This project explores handling command-line arguments in Node.js, managing npm packages, and dealing with external library data structures.

---

## Features
* **Language Detection:** Uses text sample analysis to accurately track down language origins.
* **Color-Coded Output:** Utilizes bright terminal formatting (green for success, red for errors/failures).
* **Safe Error Handling:** Built-in validation to prevent app crashes (`TypeError`) if a language code cannot be mapped.

## Prerequisites
* **Node.js** (v14 or higher recommended)
* **npm** (Node Package Manager)

## Installed Dependencies
The application relies on the following npm packages:
* **`franc`**: Language detection engine.
* **`langs`**: Conversational language profile database to translate 3-letter ISO codes into full names.
* **`colors`**: Terminal text color customization tools.

## How to Run It
Open your terminal inside the project directory and run the script by passing your sample text as a command-line argument:

```bash
node index.js "Bonjour tout le monde, comment allez-vous today?"
```

> **Note:** The underlying analyzer works best with longer sample text or complete sentences rather than short, single words.
