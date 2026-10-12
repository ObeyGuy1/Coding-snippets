# Coding Snippets

A beginner-friendly collection of 10 essential coding snippets every developer should know. Learn practical techniques for arrays, error handling, API requests, email validation, duplicate removal, debouncing, environment variables, Python file handling, and retry logic. Perfect for beginners building their programming skills.

These examples use **JavaScript** unless marked otherwise. Copy, run, and adapt them to your projects.

## Contents

1. [Find an item in an array](#1-find-an-item-in-an-array)
2. [Filter an array](#2-filter-an-array)
3. [Handle errors with try/catch](#3-handle-errors-with-trycatch)
4. [Make an API request](#4-make-an-api-request)
5. [Basic email validation](#5-basic-email-validation)
6. [Remove duplicate values](#6-remove-duplicate-values)
7. [Debounce a function](#7-debounce-a-function)
8. [Read an environment variable](#8-read-an-environment-variable)
9. [Read a file in Python](#9-read-a-file-in-python)
10. [Retry a failed operation](#10-retry-a-failed-operation)

---

## 1. Find an item in an array

Use `.find()` to return the first item that matches a condition.

```javascript
const users = [
  { id: 1, name: "Alex" },
  { id: 2, name: "Sam" },
];

const user = users.find((person) => person.id === 2);

console.log(user); // { id: 2, name: "Sam" }
```

**Tip:** If no item matches, `.find()` returns `undefined`.

## 2. Filter an array

Use `.filter()` to create a new array containing only items that pass a test.

```javascript
const tasks = [
  { title: "Study", done: true },
  { title: "Code", done: false },
  { title: "Exercise", done: true },
];

const completedTasks = tasks.filter((task) => task.done);

console.log(completedTasks);
```

**Tip:** The original array is not changed.

## 3. Handle errors with try/catch

Use `try/catch` to handle errors instead of letting them crash the current operation unexpectedly.

```javascript
async function loadUser() {
  try {
    const user = await getUserFromServer();
    console.log(user);
  } catch (error) {
    console.error("Could not load user:", error.message);
  }
}
```

Replace `getUserFromServer()` with a real function from your project.

## 4. Make an API request

Use `fetch()` to request data from an API.

```javascript
async function getPosts() {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/posts");

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const posts = await response.json();
    console.log(posts);
  } catch (error) {
    console.error("Request failed:", error.message);
  }
}

getPosts();
```

**Tip:** `fetch()` does not automatically throw for HTTP errors such as 404 or 500, so check `response.ok`.

## 5. Basic email validation

This regular expression checks whether an input resembles an email address.

```javascript
function looksLikeEmail(email) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

console.log(looksLikeEmail("hello@example.com")); // true
console.log(looksLikeEmail("not-an-email"));      // false
```

**Important:** This is a basic format check, not proof that the address exists. For real accounts, use email verification.

## 6. Remove duplicate values

Use a `Set` to keep only unique values.

```javascript
const numbers = [1, 2, 2, 3, 4, 4, 5];

const uniqueNumbers = [...new Set(numbers)];

console.log(uniqueNumbers); // [1, 2, 3, 4, 5]
```

This works for primitive values such as strings and numbers. Objects are considered unique by reference.

## 7. Debounce a function

Debouncing waits until calls stop for a set amount of time before running the function. It is useful for search boxes and resize handlers.

```javascript
function debounce(callback, delay = 300) {
  let timeoutId;

  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => callback(...args), delay);
  };
}

const searchLater = debounce((query) => {
  console.log("Search for:", query);
}, 300);

searchLater("co");
searchLater("cod");
searchLater("code"); // Only the last call runs after the delay.
```

## 8. Read an environment variable

Keep API keys and other secrets outside your source code. In Node.js, read them from environment variables.

```javascript
const apiKey = process.env.API_KEY;

if (!apiKey) {
  throw new Error("Missing API_KEY environment variable");
}

// Use apiKey with your trusted API client.
```

Set `API_KEY` in your local environment or secret manager. **Never commit real API keys, passwords, or tokens to GitHub.** If a secret is exposed, revoke and replace it.

## 9. Read a file in Python

Use `pathlib` to read a UTF-8 text file.

```python
from pathlib import Path

file_path = Path("notes.txt")

try:
    text = file_path.read_text(encoding="utf-8")
    print(text)
except FileNotFoundError:
    print("The file was not found.")
except OSError as error:
    print(f"Could not read the file: {error}")
```

Put `notes.txt` in the working directory, or change the path to the file you want to read.

## 10. Retry a failed operation

Retry temporary failures with a short delay between attempts.

```javascript
const sleep = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

async function retry(operation, attempts = 3, delayMs = 500) {
  let lastError;

  for (let attempt = 1; attempt <= attempts; attempt++) {
    try {
      return await operation();
    } catch (error) {
      lastError = error;

      if (attempt < attempts) {
        await sleep(delayMs * attempt);
      }
    }
  }

  throw lastError;
}

// Example:
// const result = await retry(() => someUnreliableOperation());
```

**Tip:** This retries only when `operation` throws or rejects. If it makes an HTTP request, check `response.ok` and throw for retryable statuses inside the operation. Avoid retrying non-repeatable actions, such as charging a card, unless the API supports idempotency.

---

## How to use this collection

- Pick one snippet and run it in a small test project.
- Change the sample data and observe the result.
- Read the comments and adapt the pattern to your own code.
- Check the documentation for your language and framework before using a snippet in production.

Happy coding! 🚀
