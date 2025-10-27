---
title: "Node.js - JavaScript Runtime cho Backend Development"
date: 2024-02-25T10:00:00+07:00
draft: false
description: "Giới thiệu về Node.js, npm, modules, HTTP Server, và xây dựng REST API đơn giản"
tags: ["Node.js", "JavaScript", "Backend", "npm", "REST API"]
categories: ["JavaScript", "Backend"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
🚀 Discover **Node.js** - bring JavaScript to the server! Build REST APIs, real-time apps, microservices và hơn thế nữa với JavaScript!
{{< /lead >}}

## Giới thiệu về Node.js

{{< alert "star" >}}
**⭐ Game Changer:** Node.js changed everything! JavaScript không còn chỉ cho frontend - giờ đây build **FULL-STACK** apps with one language!
{{< /alert >}}

**Node.js** là JavaScript runtime built trên V8 JavaScript engine của Chrome. Node.js cho phép chạy JavaScript trên **server-side**, không chỉ trong browser.

### Đặc điểm của Node.js

{{< mermaid >}}
graph TB
    A[Node.js] --> B[V8 Engine]
    A --> C[Event Loop]
    A --> D[NPM Ecosystem]
    A --> E[Non-blocking I/O]
    
    B --> F[Fast Execution]
    C --> G[Async Operations]
    D --> H[Millions of Packages]
    E --> I[High Scalability]
    
    style A fill:#68a063
    style B fill:#339af0
    style C fill:#ff6b6b
    style D fill:#ffd43b
{{< /mermaid >}}

**5 đặc điểm nổi bật:**
1. {{< badge >}}Asynchronous{{< /badge >}} Non-blocking I/O - handle thousands of connections!
2. {{< badge >}}Single-threaded{{< /badge >}} But highly scalable with Event Loop
3. {{< badge >}}V8 Engine{{< /badge >}} Lightning fast - compile JS to machine code
4. {{< badge >}}NPM{{< /badge >}} Largest package ecosystem in the world!
5. {{< badge >}}Cross-platform{{< /badge >}} Windows, Linux, MacOS

### Ứng dụng của Node.js

- REST APIs
- Real-time applications (Chat, Gaming)
- Microservices
- Streaming applications
- Command-line tools
- Server-side rendering

## Cài đặt Node.js

### Download và cài đặt

1. Truy cập: [https://nodejs.org/](https://nodejs.org/)
2. Download LTS version (recommended)
3. Cài đặt theo hướng dẫn

### Kiểm tra version

```bash
# Node.js version
node --version
# hoặc
node -v

# npm version
npm --version
npm -v
```

## Node.js REPL

REPL (Read-Eval-Print Loop) là interactive shell để test JavaScript code.

```bash
# Vào REPL
node

# Test code
> console.log("Hello Node.js!")
Hello Node.js!
undefined

> 5 + 3
8

> const greeting = "Hello"
undefined

> greeting + " World"
'Hello World'

# Thoát REPL
> .exit
# hoặc Ctrl + C (2 lần)
```

## Chương trình Node.js đầu tiên

### Hello World

```javascript
// File: app.js

console.log("Hello, Node.js!");
console.log("Node version:", process.version);
console.log("Platform:", process.platform);
```

Chạy file:

```bash
node app.js
```

## Node.js Modules

Node.js sử dụng CommonJS module system.

### Built-in Modules

```javascript
// File System module
const fs = require('fs');

// Path module
const path = require('path');

// HTTP module
const http = require('http');

// OS module
const os = require('os');

console.log("OS:", os.platform());
console.log("CPU cores:", os.cpus().length);
console.log("Free memory:", os.freemem());
```

### Creating Custom Modules

```javascript
// File: math.js - Export module

// Cách 1: Export individual functions
exports.add = function(a, b) {
    return a + b;
};

exports.subtract = function(a, b) {
    return a - b;
};

// Cách 2: Export object
module.exports = {
    multiply: function(a, b) {
        return a * b;
    },
    
    divide: function(a, b) {
        if (b === 0) {
            throw new Error("Cannot divide by zero");
        }
        return a / b;
    }
};
```

```javascript
// File: app.js - Import module

const math = require('./math');

console.log("5 + 3 =", math.add(5, 3));
console.log("5 - 3 =", math.subtract(5, 3));
console.log("5 * 3 =", math.multiply(5, 3));
console.log("6 / 3 =", math.divide(6, 3));
```

### ES6 Modules (import/export)

```javascript
// File: math.mjs - hoặc dùng "type": "module" trong package.json

export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

export default {
    multiply: (a, b) => a * b,
    divide: (a, b) => a / b
};
```

```javascript
// File: app.mjs

import math, { add, subtract } from './math.mjs';

console.log(add(5, 3));
console.log(math.multiply(5, 3));
```

## File System Operations

```javascript
const fs = require('fs');

// 1. Read file - Synchronous (blocking)
const dataSync = fs.readFileSync('file.txt', 'utf8');
console.log(dataSync);

// 2. Read file - Asynchronous (non-blocking) - RECOMMENDED
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) {
        console.error("Error reading file:", err);
        return;
    }
    console.log("File content:", data);
});

// 3. Write file - Asynchronous
const content = "Hello from Node.js!";
fs.writeFile('output.txt', content, 'utf8', (err) => {
    if (err) {
        console.error("Error writing file:", err);
        return;
    }
    console.log("File written successfully!");
});

// 4. Append to file
fs.appendFile('output.txt', '\nNew line!', (err) => {
    if (err) throw err;
    console.log("Content appended!");
});

// 5. Delete file
fs.unlink('output.txt', (err) => {
    if (err) throw err;
    console.log("File deleted!");
});

// 6. Check if file exists
if (fs.existsSync('file.txt')) {
    console.log("File exists!");
}

// 7. Create directory
fs.mkdir('new-folder', (err) => {
    if (err) throw err;
    console.log("Directory created!");
});

// 8. Read directory
fs.readdir('./', (err, files) => {
    if (err) throw err;
    console.log("Files:", files);
});
```

## HTTP Server

### Simple HTTP Server

```javascript
const http = require('http');

// Create server
const server = http.createServer((req, res) => {
    // Set response header
    res.writeHead(200, {'Content-Type': 'text/html'});
    
    // Send response
    res.write('<h1>Hello from Node.js Server!</h1>');
    res.write('<p>Welcome to my first server</p>');
    res.end();
});

// Start server
const PORT = 3000;
server.listen(PORT, () => {
    console.log(`🚀 Server running on http://localhost:${PORT}`);
});
```

### Routing

```javascript
const http = require('http');

const server = http.createServer((req, res) => {
    const url = req.url;
    const method = req.method;
    
    // Home page
    if (url === '/' || url === '/home') {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write('<h1>Home Page</h1>');
        res.write('<a href="/about">About</a>');
        res.end();
    }
    // About page
    else if (url === '/about') {
        res.writeHead(200, {'Content-Type': 'text/html'});
        res.write('<h1>About Page</h1>');
        res.write('<p>This is the about page</p>');
        res.end();
    }
    // API endpoint
    else if (url === '/api/users') {
        res.writeHead(200, {'Content-Type': 'application/json'});
        const users = [
            {id: 1, name: 'Trung Tín'},
            {id: 2, name: 'Nguyễn Văn A'}
        ];
        res.end(JSON.stringify(users));
    }
    // 404 Not Found
    else {
        res.writeHead(404, {'Content-Type': 'text/html'});
        res.write('<h1>404 - Page Not Found</h1>');
        res.end();
    }
});

server.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

## NPM (Node Package Manager)

### Khởi tạo project

```bash
# Tạo package.json
npm init

# Hoặc sử dụng default values
npm init -y
```

### Cài đặt packages

```bash
# Cài đặt package (dependencies)
npm install express
npm i express

# Cài đặt dev dependencies
npm install --save-dev nodemon
npm i -D nodemon

# Cài đặt global
npm install -g nodemon

# Cài đặt specific version
npm install express@4.17.1
```

### package.json

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "My first Node.js app",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "dev": "nodemon app.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": ["nodejs", "javascript"],
  "author": "Trung Tín",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  },
  "devDependencies": {
    "nodemon": "^2.0.22"
  }
}
```

### Scripts

```bash
# Chạy scripts
npm start
npm run dev
npm test
```

## Express.js - Web Framework

{{< badge >}}
🔥 Most Popular
{{< /badge >}}

{{< alert "lightbulb" >}}
**💡 Express Power:** Framework **ĐƠN GIẢN NHẤT** để build REST APIs và web apps. Used by Netflix, Uber, IBM!
{{< /alert >}}

### Cài đặt Express

```bash
npm install express
```

### Simple Express Server

```javascript
const express = require('express');
const app = express();

// Middleware để parse JSON
app.use(express.json());

// Routes
app.get('/', (req, res) => {
    res.send('<h1>Welcome to Express!</h1>');
});

app.get('/api/users', (req, res) => {
    const users = [
        {id: 1, name: 'Trung Tín', age: 20},
        {id: 2, name: 'Nguyễn Văn A', age: 22}
    ];
    res.json(users);
});

app.get('/api/users/:id', (req, res) => {
    const id = parseInt(req.params.id);
    const user = {id, name: 'User ' + id};
    res.json(user);
});

app.post('/api/users', (req, res) => {
    const newUser = req.body;
    console.log('New user:', newUser);
    res.status(201).json({
        message: 'User created',
        user: newUser
    });
});

// Start server
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`🚀 Server running on port ${PORT}`);
});
```

## REST API với Express

```javascript
const express = require('express');
const app = express();

app.use(express.json());

// In-memory database
let books = [
    {id: 1, title: 'JavaScript: The Good Parts', author: 'Douglas Crockford'},
    {id: 2, title: 'Eloquent JavaScript', author: 'Marijn Haverbeke'}
];

// GET - Lấy tất cả books
app.get('/api/books', (req, res) => {
    res.json(books);
});

// GET - Lấy book theo ID
app.get('/api/books/:id', (req, res) => {
    const book = books.find(b => b.id === parseInt(req.params.id));
    
    if (!book) {
        return res.status(404).json({error: 'Book not found'});
    }
    
    res.json(book);
});

// POST - Tạo book mới
app.post('/api/books', (req, res) => {
    const {title, author} = req.body;
    
    // Validation
    if (!title || !author) {
        return res.status(400).json({error: 'Title and author required'});
    }
    
    const newBook = {
        id: books.length + 1,
        title,
        author
    };
    
    books.push(newBook);
    res.status(201).json(newBook);
});

// PUT - Update book
app.put('/api/books/:id', (req, res) => {
    const book = books.find(b => b.id === parseInt(req.params.id));
    
    if (!book) {
        return res.status(404).json({error: 'Book not found'});
    }
    
    const {title, author} = req.body;
    if (title) book.title = title;
    if (author) book.author = author;
    
    res.json(book);
});

// DELETE - Xóa book
app.delete('/api/books/:id', (req, res) => {
    const index = books.findIndex(b => b.id === parseInt(req.params.id));
    
    if (index === -1) {
        return res.status(404).json({error: 'Book not found'});
    }
    
    books.splice(index, 1);
    res.status(204).send();
});

app.listen(3000, () => {
    console.log('Books API running on port 3000');
});
```

### Test API với curl

```bash
# GET all books
curl http://localhost:3000/api/books

# GET book by ID
curl http://localhost:3000/api/books/1

# POST new book
curl -X POST http://localhost:3000/api/books \
  -H "Content-Type: application/json" \
  -d '{"title":"Node.js Design Patterns","author":"Mario Casciaro"}'

# PUT update book
curl -X PUT http://localhost:3000/api/books/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"Updated Title"}'

# DELETE book
curl -X DELETE http://localhost:3000/api/books/1
```

## Environment Variables

```javascript
// File: .env
PORT=3000
DB_URL=mongodb://localhost:27017/mydb
API_KEY=secret123

// Install dotenv
// npm install dotenv

// File: app.js
require('dotenv').config();

const PORT = process.env.PORT || 3000;
const DB_URL = process.env.DB_URL;
const API_KEY = process.env.API_KEY;

console.log('Port:', PORT);
console.log('Database:', DB_URL);
```

## Middleware

```javascript
const express = require('express');
const app = express();

// Logger middleware
app.use((req, res, next) => {
    console.log(`${req.method} ${req.url} - ${new Date().toISOString()}`);
    next(); // Chuyển sang middleware tiếp theo
});

// Authentication middleware
const authenticate = (req, res, next) => {
    const token = req.headers['authorization'];
    
    if (!token) {
        return res.status(401).json({error: 'No token provided'});
    }
    
    // Verify token (simplified)
    if (token === 'secret-token') {
        next();
    } else {
        res.status(403).json({error: 'Invalid token'});
    }
};

// Public route
app.get('/api/public', (req, res) => {
    res.json({message: 'This is public'});
});

// Protected route
app.get('/api/protected', authenticate, (req, res) => {
    res.json({message: 'This is protected'});
});

app.listen(3000);
```

## Kết luận

{{< timeline >}}

{{< timelineItem icon="box" header="Node.js Core" badge="Foundation" >}}
{{< badge >}}Modules{{< /badge >}} {{< badge >}}File System{{< /badge >}} {{< badge >}}HTTP Server{{< /badge >}} - Build từ fundamental đến advanced!
{{< /timelineItem >}}

{{< timelineItem icon="cube" header="NPM Ecosystem" badge="Power" >}}
{{< badge >}}npm{{< /badge >}} {{< badge >}}package.json{{< /badge >}} - Truy cập millions of packages, never reinvent the wheel!
{{< /timelineItem >}}

{{< timelineItem icon="rocket" header="Express.js" badge="Production" >}}
{{< badge >}}Routing{{< /badge >}} {{< badge >}}Middleware{{< /badge >}} {{< badge >}}REST API{{< /badge >}} - Build production-ready applications!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Node.js Success Path:**
- ✅ Master **async/await** - non-blocking là chìa khóa
- ✅ Learn **Express.js** - standard cho web apps
- ✅ Practice **REST API** design - API là ngôn ngữ của backend
- ✅ Explore **npm packages** - đừng code từ đầu khi đã có package!
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://nodejs.org/docs/" target="_blank" >}}
📚 Node.js Official Docs
{{< /button >}}

{{< button href="https://docs.npmjs.com/" target="_blank" >}}
📦 NPM Documentation
{{< /button >}}

{{< button href="https://expressjs.com/en/guide/routing.html" target="_blank" >}}
🚂 Express.js Guide
{{< /button >}}

{{< button href="https://www.nodejsdesignpatterns.com/" target="_blank" >}}
📘 Node.js Design Patterns
{{< /button >}}

{{< button href="https://developer.mozilla.org/en-US/docs/Learn/Server-side" target="_blank" >}}
🎓 MDN Server-side JS
{{< /button >}}

