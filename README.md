# Lecture cheat sheet

## Table of Contents
1. [File System (fs)](#1-file-system-fs)
2. [Path Module](#2-path-module)
   - [When to Use](#when-to-use)
3. [Events and EventEmitter](#3-events-and-eventemitter)
   - [Creating Custom Event Emitters](#creating-custom-event-emitters)
   - [When to Use](#when-to-use-1)
4. [Streams](#4-streams)
   - [Types of Streams](#types-of-streams)
   - [Piping Streams](#piping-streams)
   - [Transform Streams](#transform-streams)
   - [When to Use](#when-to-use-2)
5. [HTTP Module](#5-http-module)
   - [Creating a Simple HTTP Server](#creating-a-simple-http-server)
   - [Handling Different HTTP Methods](#handling-different-http-methods)
   - [Making HTTP Requests](#making-http-requests)
   - [When to Use](#when-to-use-3)
6. [Working with NPM and Packages](#6-working-with-npm-and-packages)
   - [Initializing a Project](#initializing-a-project)
   - [Installing Packages](#installing-packages)
   - [Commonly Used Packages](#commonly-used-packages)
   - [When to Use NPM Packages](#when-to-use-npm-packages)
7. [Environment Variables](#7-environment-variables)
   - [Using process.env](#using-processenv)
   - [Using dotenv Package](#using-dotenv-package)
   - [Different Environments](#different-environments)
   - [When to Use](#when-to-use-4)
8. [Asynchronous Patterns](#8-asynchronous-patterns)
   - [Understanding the Event Loop](#understanding-the-event-loop)
   - [Callbacks (Traditional Pattern)](#callbacks-traditional-pattern)
   - [Promises](#promises)
   - [Async/Await (Modern Pattern)](#asyncawait-modern-pattern)
   - [Parallel Execution](#parallel-execution)
   - [Sequential Execution](#sequential-execution)
   - [Error Handling with Async/Await](#error-handling-with-asyncawait)
   - [When to Use](#when-to-use-5)
9. [Modules and Exports](#9-modules-and-exports)
   - [Creating Modules](#creating-modules)
   - [Using Modules](#using-modules)
   - [Single Export](#single-export)
   - [ES6 Modules (ESM)](#es6-modules-esm)
   - [When to Use](#when-to-use-6)

---

## 1. File system (fs)
In this lesson, we will learn how to use the `fs` module in Node.js to perform file operations such as reading, writing, and deleting files.
```js
// Import the fs module 
const fs = require('fs');

// Read a file
let file = fs.readFileSync('test.txt');
console.log(file.toString());

// Create a new file
let fName = "newFile.txt";
let fContent = "This is a new file created by Node.js";
fs.writeFileSync(fName,fContent);

// Remove a file
fs.rmSync(fName);
console.log(`${fName} removed`);
```
## 2. Path module
When working with file systems, it's important to handle file paths correctly. The `path` module in Node.js provides utilities for working with file and directory paths.  
We know that 
1. test.txt is a file in the current directory
2. /home/user/docs/file.txt is an absolute path
3. ../images/photo.jpg is a relative path

if NodeJS there's some global variables that can help us to get the current directory and file name
- `__dirname`: The directory name of the current module.
- `__filename`: The full path to the current module file.

assume we have the following directory structure:
```
project/
│├── index.js
│├── data/
││   ├── users.json
││   └── products.json
│└── images/
│    ├── logo.png
│    └── banner.jpg
```
we can use the `path` module to construct paths to files in the `data` and `images` directories:
```js
// Import the path module
const path = require('path');

// Construct paths
let usersPath = path.join(__dirname, 'data', 'users.json');
let logoPath = path.join(__dirname, 'images', 'logo.png');

console.log(`Users path: ${usersPath}`);
console.log(`Logo path: ${logoPath}`);
```
This will output:
```
Users path: /home/user/project/data/users.json
Logo path: /home/user/project/images/logo.png
```

Why use the `path` module? and why is full path important?
1. Cross-platform compatibility: Different operating systems use different path separators (e.g., `/` on Unix-based systems and `\` on Windows). The `path` module handles these differences for you.
2. Avoiding errors: Manually constructing paths can lead to errors, especially when dealing with relative paths. The `path` module provides methods to normalize and resolve paths, reducing the risk of mistakes.
3. Readability and maintainability: Using the `path` module makes your code more readable and easier to maintain, as it clearly indicates that you are working with file paths.

### When to Use:
- **File Upload Systems**: When users upload files and you need to save them in organized directories (e.g., `/uploads/2025/10/user_123/profile.jpg`)
- **Static File Serving**: Building paths to serve CSS, JS, images in web applications
- **Log File Management**: Creating dated log files in specific directories (e.g., `/logs/2025-10-15/app.log`)
- **Configuration Files**: Loading config files from different environments (dev, staging, production)

## 3. Events and EventEmitter
Node.js is built around an event-driven architecture. The `events` module provides the `EventEmitter` class, which allows you to create, emit, and handle custom events.

```js
// Import the events module
const EventEmitter = require('events');

// Create an EventEmitter instance
const myEmitter = new EventEmitter();

// Register an event listener
myEmitter.on('userLogin', (username) => {
  console.log(`User ${username} logged in at ${new Date().toLocaleString()}`);
});

// Register another listener for the same event
myEmitter.on('userLogin', (username) => {
  console.log(`Sending welcome email to ${username}`);
});

// Emit the event
myEmitter.emit('userLogin', 'john_doe');
```

**Output:**
```
User john_doe logged in at 10/15/2025, 10:30:00 AM
Sending welcome email to john_doe
```

### Creating Custom Event Emitters:
```js
const EventEmitter = require('events');

class OrderProcessor extends EventEmitter {
  processOrder(orderId, items) {
    console.log(`Processing order ${orderId}...`);
    
    // Emit event when order is validated
    this.emit('orderValidated', orderId);
    
    // Emit event when payment is processed
    setTimeout(() => {
      this.emit('paymentProcessed', orderId, items.length);
    }, 1000);
    
    // Emit event when order is shipped
    setTimeout(() => {
      this.emit('orderShipped', orderId);
    }, 2000);
  }
}

const processor = new OrderProcessor();

processor.on('orderValidated', (orderId) => {
  console.log(`Order ${orderId} validated`);
});

processor.on('paymentProcessed', (orderId, itemCount) => {
  console.log(`Payment processed for order ${orderId} with ${itemCount} items`);
});

processor.on('orderShipped', (orderId) => {
  console.log(`Order ${orderId} shipped!`);
});

processor.processOrder('ORD-123', ['item1', 'item2', 'item3']);
```

### When to Use:
- **Real-time Applications**: Chat applications where messages need to be broadcasted to multiple users
- **Microservices Communication**: Decoupling services by emitting events when tasks complete
- **Database Change Tracking**: Notifying different parts of your app when data changes
- **Job Queue Systems**: Emitting events when jobs start, complete, or fail
- **User Activity Tracking**: Triggering analytics, notifications, and logging based on user actions

## 4. Streams
Streams are collections of data that might not be available all at once and don't have to fit in memory. They allow you to process data piece by piece, which is memory-efficient for large files or data sources.

### Types of Streams:
1. **Readable**: Streams you can read from (e.g., `fs.createReadStream()`)
2. **Writable**: Streams you can write to (e.g., `fs.createWriteStream()`)
3. **Duplex**: Streams that are both readable and writable (e.g., TCP sockets)
4. **Transform**: Duplex streams that can modify data as it's read or written (e.g., compression)

```js
const fs = require('fs');

// Reading a large file using streams (memory-efficient)
const readStream = fs.createReadStream('largefile.txt', { encoding: 'utf8' });

readStream.on('data', (chunk) => {
  console.log('Received chunk:', chunk.length, 'bytes');
});

readStream.on('end', () => {
  console.log('Finished reading file');
});

readStream.on('error', (err) => {
  console.error('Error:', err);
});
```

### Piping Streams:
```js
const fs = require('fs');
const zlib = require('zlib');

// Copy and compress a file
const readStream = fs.createReadStream('input.txt');
const writeStream = fs.createWriteStream('output.txt.gz');
const gzip = zlib.createGzip();

// Pipe: read -> compress -> write
readStream.pipe(gzip).pipe(writeStream);

writeStream.on('finish', () => {
  console.log('File compressed successfully');
});
```

### Transform Streams:
```js
const { Transform } = require('stream');

// Create a custom transform stream to uppercase text
const upperCaseTransform = new Transform({
  transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
});

process.stdin.pipe(upperCaseTransform).pipe(process.stdout);
```

### When to Use:
- **Video/Audio Streaming**: Netflix-like platforms streaming large media files
- **File Uploads**: Processing large file uploads without loading entire file in memory
- **CSV Processing**: Reading and processing large CSV files row by row
- **Log Processing**: Analyzing large log files in real-time
- **Data ETL**: Extracting, transforming, and loading data from one source to another
- **HTTP Responses**: Sending large responses to clients chunk by chunk

## 5. HTTP Module
The `http` module allows Node.js to create HTTP servers and make HTTP requests.

### Creating a Simple HTTP Server:
```js
const http = require('http');

const server = http.createServer((req, res) => {
  // Set response header
  res.writeHead(200, { 'Content-Type': 'text/html' });
  
  // Route handling
  if (req.url === '/') {
    res.write('<h1>Home Page</h1>');
  } else if (req.url === '/about') {
    res.write('<h1>About Page</h1>');
  } else {
    res.writeHead(404);
    res.write('<h1>404 - Page Not Found</h1>');
  }
  
  res.end();
});

const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

### Handling Different HTTP Methods:
```js
const http = require('http');

const server = http.createServer((req, res) => {
  const { method, url } = req;
  
  if (method === 'GET' && url === '/users') {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ users: ['Alice', 'Bob', 'Charlie'] }));
  } 
  else if (method === 'POST' && url === '/users') {
    let body = '';
    
    req.on('data', (chunk) => {
      body += chunk.toString();
    });
    
    req.on('end', () => {
      const newUser = JSON.parse(body);
      res.writeHead(201, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ message: 'User created', user: newUser }));
    });
  }
  else {
    res.writeHead(404);
    res.end('Not Found');
  }
});

server.listen(3000);
```

### Making HTTP Requests:
```js
const http = require('http');

const options = {
  hostname: 'api.example.com',
  port: 80,
  path: '/users',
  method: 'GET',
  headers: {
    'Content-Type': 'application/json'
  }
};

const req = http.request(options, (res) => {
  let data = '';
  
  res.on('data', (chunk) => {
    data += chunk;
  });
  
  res.on('end', () => {
    console.log('Response:', JSON.parse(data));
  });
});

req.on('error', (error) => {
  console.error('Error:', error);
});

req.end();
```

### When to Use:
- **REST APIs**: Building backend APIs for web and mobile applications
- **Webhooks**: Creating endpoints to receive webhook notifications from third-party services
- **Microservices**: Building lightweight HTTP-based microservices
- **Proxy Servers**: Creating proxy servers to forward requests
- **API Gateway**: Building custom routing and load balancing solutions
- **Health Check Endpoints**: Creating monitoring endpoints for your services

## 6. Working with NPM and Packages
NPM (Node Package Manager) is the default package manager for Node.js. It allows you to install, manage, and share JavaScript packages.

### Initializing a Project:
```bash
npm init -y
```

This creates a `package.json` file:
```json
{
  "name": "my-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js",
    "dev": "nodemon index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

### Installing Packages:
```bash
# Install a package and add to dependencies
npm install express

# Install a dev dependency (only needed for development)
npm install --save-dev nodemon

# Install a specific version
npm install lodash@4.17.21

# Install globally
npm install -g pm2
```

### Commonly Used Packages:
```js
// Express - Web framework
const express = require('express');
const app = express();

// Lodash - Utility library
const _ = require('lodash');
const numbers = [1, 2, 3, 4, 5];
const doubled = _.map(numbers, n => n * 2);

// Axios - HTTP client
const axios = require('axios');
const response = await axios.get('https://api.example.com/data');

// Dotenv - Environment variables
require('dotenv').config();
const apiKey = process.env.API_KEY;

// Moment - Date manipulation (or use date-fns)
const moment = require('moment');
const formattedDate = moment().format('YYYY-MM-DD');
```

### When to Use NPM Packages:
- **Authentication**: Use `passport`, `jsonwebtoken` for user authentication
- **Database Operations**: Use `mongoose` (MongoDB), `sequelize` (SQL databases)
- **Validation**: Use `joi`, `validator` for input validation
- **File Processing**: Use `multer` for file uploads, `sharp` for image processing
- **Email Sending**: Use `nodemailer` to send emails
- **Testing**: Use `jest`, `mocha`, `chai` for unit and integration testing
- **Logging**: Use `winston`, `pino` for structured logging
- **Security**: Use `helmet`, `bcrypt` for security enhancements

## 7. Environment Variables
Environment variables allow you to configure your application without hardcoding sensitive information like API keys, database credentials, and configuration settings.

### Using process.env:
```js
// Accessing environment variables
const port = process.env.PORT || 3000;
const dbUrl = process.env.DATABASE_URL;
const apiKey = process.env.API_KEY;

console.log(`Server will run on port ${port}`);
```

### Using dotenv Package:
Create a `.env` file:
```
PORT=3000
DATABASE_URL=mongodb://localhost:27017/mydb
API_KEY=your_secret_api_key_here
NODE_ENV=development
EMAIL_USER=noreply@example.com
EMAIL_PASS=secret_password
```

In your code:
```js
require('dotenv').config();

const config = {
  port: process.env.PORT,
  database: process.env.DATABASE_URL,
  apiKey: process.env.API_KEY,
  environment: process.env.NODE_ENV,
  email: {
    user: process.env.EMAIL_USER,
    pass: process.env.EMAIL_PASS
  }
};

// Use configuration
if (config.environment === 'development') {
  console.log('Running in development mode');
}
```

### Different Environments:
```js
// config.js
const config = {
  development: {
    port: 3000,
    database: 'mongodb://localhost:27017/myapp_dev',
    logLevel: 'debug'
  },
  production: {
    port: process.env.PORT,
    database: process.env.DATABASE_URL,
    logLevel: 'error'
  }
};

const env = process.env.NODE_ENV || 'development';
module.exports = config[env];
```

### When to Use:
- **API Keys**: Store third-party API keys (Stripe, SendGrid, AWS)
- **Database Credentials**: Different database URLs for dev, staging, production
- **Feature Flags**: Enable/disable features based on environment
- **Service Configuration**: Different settings for different deployment environments
- **Secrets Management**: Store JWT secrets, encryption keys
- **Multi-tenant Applications**: Configure different settings per client/tenant

## 8. Asynchronous Patterns
Node.js is asynchronous and non-blocking. Understanding async patterns is crucial for writing efficient Node.js applications.

### Understanding the Event Loop
The Event Loop is the core mechanism that makes Node.js asynchronous and non-blocking. It allows Node.js to handle multiple operations concurrently despite JavaScript being single-threaded.

#### How the Event Loop Works:

```
   ┌───────────────────────────┐
┌─>│           timers          │  <- setTimeout, setInterval
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │  <- I/O callbacks deferred
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │  <- internal use only
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │  <- setImmediate()
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │  <- socket.on('close', ...)
   └───────────────────────────┘
```

#### Event Loop Phases:
1. **Timers**: Executes callbacks scheduled by `setTimeout()` and `setInterval()`
2. **Pending Callbacks**: Executes I/O callbacks deferred to the next loop iteration
3. **Poll**: Retrieves new I/O events; executes I/O related callbacks
4. **Check**: Executes `setImmediate()` callbacks
5. **Close Callbacks**: Executes close event callbacks (e.g., `socket.on('close')`)

#### Visualizing Execution Order:
```js
console.log('1. Start');

setTimeout(() => {
  console.log('4. Timeout callback (Timers phase)');
}, 0);

setImmediate(() => {
  console.log('5. Immediate callback (Check phase)');
});

Promise.resolve().then(() => {
  console.log('3. Promise callback (Microtask queue)');
});

console.log('2. End');

// Output:
// 1. Start
// 2. End
// 3. Promise callback (Microtask queue)
// 4. Timeout callback (Timers phase)
// 5. Immediate callback (Check phase)
```

#### Call Stack, Task Queue, and Microtask Queue:
```js
// Understanding the execution priority
console.log('Script start');

setTimeout(() => {
  console.log('setTimeout 1');
  Promise.resolve().then(() => {
    console.log('Promise inside setTimeout');
  });
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Promise 1');
  })
  .then(() => {
    console.log('Promise 2');
  });

setTimeout(() => {
  console.log('setTimeout 2');
}, 0);

console.log('Script end');

/* Output:
Script start
Script end
Promise 1
Promise 2
setTimeout 1
Promise inside setTimeout
setTimeout 2

Explanation:
1. Synchronous code runs first (Call Stack)
2. Microtasks (Promises) execute after current script, before next task
3. Macrotasks (setTimeout) execute in subsequent event loop iterations
*/
```

#### Blocking vs Non-Blocking:
```js
// ❌ BLOCKING - Bad Practice
const fs = require('fs');
console.log('Start');

// This blocks the entire event loop!
const data = fs.readFileSync('largefile.txt', 'utf8');
console.log('File read:', data.length);

console.log('End');

// ✅ NON-BLOCKING - Good Practice
console.log('Start');

fs.readFile('largefile.txt', 'utf8', (err, data) => {
  if (err) throw err;
  console.log('File read:', data.length);
});

console.log('End');
// Output:
// Start
// End
// File read: 1000000 (whenever file finishes reading)
```

#### Process.nextTick() and setImmediate():
```js
// process.nextTick() executes BEFORE the event loop continues
// setImmediate() executes in the Check phase

console.log('Start');

setImmediate(() => {
  console.log('setImmediate');
});

process.nextTick(() => {
  console.log('nextTick');
});

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');

/* Output:
Start
End
nextTick
Promise
setImmediate

Priority: Synchronous > nextTick > Promises (Microtasks) > setImmediate
*/
```

#### Real-World Event Loop Example:
```js
const fs = require('fs');
const http = require('http');

const server = http.createServer((req, res) => {
  console.log('Request received');
  
  // File reading is offloaded to the system
  // Server can handle other requests while waiting
  fs.readFile('data.json', 'utf8', (err, data) => {
    if (err) {
      res.writeHead(500);
      res.end('Error reading file');
      return;
    }
    
    // This callback executes when file reading completes
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(data);
    console.log('Response sent');
  });
  
  console.log('Handler completed (file still reading in background)');
});

server.listen(3000);
// Server can handle thousands of concurrent requests
// because it doesn't wait (block) for file operations
```

#### Key Takeaways:
- **Single-threaded**: JavaScript runs on one thread, but I/O operations are handled by the system
- **Non-blocking**: Operations don't wait; they use callbacks/promises
- **Event-driven**: Code reacts to events (file read complete, HTTP request received, etc.)
- **Microtasks have priority**: Promises execute before setTimeout/setInterval
- **Don't block the loop**: Avoid CPU-intensive synchronous operations

### Callbacks (Traditional Pattern):
```js
const fs = require('fs');

fs.readFile('file.txt', 'utf8', (err, data) => {
  if (err) {
    console.error('Error reading file:', err);
    return;
  }
  console.log('File content:', data);
});

console.log('This runs before file is read');
```

### Promises:
```js
const fs = require('fs').promises;

fs.readFile('file.txt', 'utf8')
  .then(data => {
    console.log('File content:', data);
  })
  .catch(err => {
    console.error('Error reading file:', err);
  });
```

### Async/Await (Modern Pattern):
```js
const fs = require('fs').promises;

async function readFileContent() {
  try {
    const data = await fs.readFile('file.txt', 'utf8');
    console.log('File content:', data);
    return data;
  } catch (err) {
    console.error('Error reading file:', err);
    throw err;
  }
}

readFileContent();
```

### Parallel Execution:
```js
const fs = require('fs').promises;

async function readMultipleFiles() {
  try {
    // Execute all promises in parallel
    const [file1, file2, file3] = await Promise.all([
      fs.readFile('file1.txt', 'utf8'),
      fs.readFile('file2.txt', 'utf8'),
      fs.readFile('file3.txt', 'utf8')
    ]);
    
    console.log('All files read successfully');
    return { file1, file2, file3 };
  } catch (err) {
    console.error('Error reading files:', err);
  }
}
```

### Sequential Execution:
```js
async function processDataSequentially() {
  try {
    const user = await fetchUser(userId);
    const orders = await fetchOrders(user.id);
    const orderDetails = await fetchOrderDetails(orders[0].id);
    
    return { user, orders, orderDetails };
  } catch (err) {
    console.error('Error in sequential processing:', err);
  }
}
```

### Error Handling with Async/Await:
```js
async function robustApiCall() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (err) {
    if (err.name === 'TypeError') {
      console.error('Network error:', err);
    } else {
      console.error('API error:', err);
    }
    throw err; // Re-throw to let caller handle it
  } finally {
    console.log('API call completed');
  }
}
```

### When to Use:
- **Database Operations**: Querying databases with async operations
- **API Calls**: Making HTTP requests to external services
- **File Operations**: Reading/writing files without blocking
- **Batch Processing**: Processing multiple items in parallel
- **Data Pipeline**: Sequential data transformation steps
- **Microservices**: Coordinating multiple service calls
- **Cron Jobs**: Running scheduled tasks asynchronously

## 9. Modules and Exports
Node.js uses the CommonJS module system to organize code into reusable modules.

### Creating Modules:
```js
// math.js
function add(a, b) {
  return a + b;
}

function subtract(a, b) {
  return a - b;
}

const PI = 3.14159;

// Export specific items
module.exports = {
  add,
  subtract,
  PI
};

// Or export individually
// exports.add = add;
// exports.subtract = subtract;
// exports.PI = PI;
```

### Using Modules:
```js
// app.js
const math = require('./math');

console.log(math.add(5, 3));      // 8
console.log(math.subtract(10, 4)); // 6
console.log(math.PI);              // 3.14159

// Destructuring
const { add, subtract } = require('./math');
console.log(add(2, 3)); // 5
```

### Single Export:
```js
// logger.js
class Logger {
  log(message) {
    console.log(`[${new Date().toISOString()}] ${message}`);
  }
  
  error(message) {
    console.error(`[${new Date().toISOString()}] ERROR: ${message}`);
  }
}

module.exports = Logger;

// Using it
const Logger = require('./logger');
const logger = new Logger();
logger.log('Application started');
```

### ES6 Modules (ESM):
```js
// math.mjs (or use "type": "module" in package.json)
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export const PI = 3.14159;

// Default export
export default class Calculator {
  add(a, b) { return a + b; }
  subtract(a, b) { return a - b; }
}

// Using ES6 imports
import Calculator, { add, subtract, PI } from './math.mjs';

const calc = new Calculator();
console.log(calc.add(5, 3));
console.log(add(2, 2));
```

### When to Use:
- **Code Organization**: Breaking large applications into manageable modules
- **Reusability**: Creating utility functions used across the application
- **Testing**: Isolating code into testable units
- **Business Logic**: Separating business logic from route handlers
- **Configuration**: Creating config modules for different environments
- **Services Layer**: Creating service modules for database, email, payment, etc.
- **Middleware**: Creating reusable middleware for Express applications
