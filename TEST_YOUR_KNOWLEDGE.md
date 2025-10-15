# Test Your Knowledge - NodeJS Fundamentals

This file contains practical exercises and quizzes to help you master NodeJS concepts. Complete the tasks for each topic, then challenge yourself with the comprehensive quizzes.

## Table of Contents
- [Topic 1: File System (fs) - Practice Tasks](#topic-1-file-system-fs---practice-tasks)
- [Topic 2: Path Module - Practice Tasks](#topic-2-path-module---practice-tasks)
- [Topic 3: Events and EventEmitter - Practice Tasks](#topic-3-events-and-eventemitter---practice-tasks)
- [Quiz 1: File System, Path, and Events](#quiz-1-file-system-path-and-events)
- [Topic 4: Streams - Practice Tasks](#topic-4-streams---practice-tasks)
- [Topic 5: HTTP Module - Practice Tasks](#topic-5-http-module---practice-tasks)
- [Topic 6: NPM and Packages - Practice Tasks](#topic-6-npm-and-packages---practice-tasks)
- [Quiz 2: Streams, HTTP, and NPM](#quiz-2-streams-http-and-npm)
- [Topic 7: Environment Variables - Practice Tasks](#topic-7-environment-variables---practice-tasks)
- [Topic 8: Asynchronous Patterns - Practice Tasks](#topic-8-asynchronous-patterns---practice-tasks)
- [Topic 9: Modules and Exports - Practice Tasks](#topic-9-modules-and-exports---practice-tasks)
- [Quiz 3: Environment Variables, Async Patterns, and Modules](#quiz-3-environment-variables-async-patterns-and-modules)
- [Final Challenge Project](#final-challenge-project)

---

## Topic 1: File System (fs) - Practice Tasks

### Task 1.1: Basic File Operations
Create a script that:
1. Creates a file named `student.txt` with your name and favorite programming language
2. Reads the file and displays its content
3. Appends your age to the file
4. Reads the file again to verify the changes
5. Deletes the file

**Expected Skills**: `writeFileSync()`, `readFileSync()`, `appendFileSync()`, `rmSync()`

### Task 1.2: Directory Management
Create a script that:
1. Creates a directory structure: `project/src/components`
2. Creates three files inside `components`: `Header.js`, `Footer.js`, `Sidebar.js`
3. Lists all files in the `components` directory
4. Deletes the entire `project` directory

**Expected Skills**: `mkdirSync()`, `readdirSync()`, `rmSync()` with recursive option

### Task 1.3: File Copy Utility
Create a script that copies a file from one location to another:
1. Create a source file `original.txt` with some content
2. Copy it to `backup.txt`
3. Verify both files exist and have the same content
4. Handle errors if the source file doesn't exist

**Expected Skills**: `readFileSync()`, `writeFileSync()`, `existsSync()`, error handling

---

## Topic 2: Path Module - Practice Tasks

### Task 2.1: Path Construction
Create a script that:
1. Constructs an absolute path to `data/users/profile.json` from the current directory
2. Extracts the filename, directory name, and file extension
3. Creates the directory structure if it doesn't exist
4. Creates the JSON file with sample user data

**Expected Skills**: `path.join()`, `path.basename()`, `path.dirname()`, `path.extname()`

### Task 2.2: Cross-Platform Path Utility
Create a utility module that:
1. Accepts a relative path as input
2. Converts it to an absolute path
3. Normalizes the path (removes `.` and `..`)
4. Returns an object with: `absolute`, `normalized`, `filename`, `extension`, `directory`

**Expected Skills**: `path.resolve()`, `path.normalize()`, `path.parse()`

### Task 2.3: File Organizer
Create a script that:
1. Creates files with different extensions: `.txt`, `.js`, `.json`, `.md`
2. Organizes them into folders based on extension
3. Uses the path module to ensure cross-platform compatibility

**Expected Skills**: `path.extname()`, `path.join()`, combining with `fs` operations

---

## Topic 3: Events and EventEmitter - Practice Tasks

### Task 3.1: Simple Event System
Create an event emitter that:
1. Emits a `userRegistered` event with username and email
2. Has two listeners: one logs the registration, another sends a "welcome email" message
3. Emits multiple user registrations
4. Shows that all listeners execute for each event

**Expected Skills**: `EventEmitter`, `on()`, `emit()`

### Task 3.2: Custom Class with Events
Create a `ShoppingCart` class that extends EventEmitter:
1. Has methods: `addItem()`, `removeItem()`, `checkout()`
2. Emits events: `itemAdded`, `itemRemoved`, `checkoutStarted`, `orderCompleted`
3. Each event should pass relevant data (item details, cart total, etc.)
4. Create listeners that log appropriate messages for each event

**Expected Skills**: Extending `EventEmitter`, custom events, passing data with events

### Task 3.3: Event-Driven Task Queue
Create a task processing system:
1. Create a `TaskQueue` class that extends EventEmitter
2. Tasks can be added with `addTask(taskName, priority)`
3. Process tasks and emit: `taskStarted`, `taskCompleted`, `taskFailed`
4. Track and display how many tasks are pending, completed, and failed

**Expected Skills**: Complex event handling, maintaining state, multiple event types

---

## Quiz 1: File System, Path, and Events

### Multiple Choice Questions

**Q1.** Which method should you use to read a file asynchronously in Node.js?
- A) `fs.readFileSync()`
- B) `fs.readFile()`
- C) `fs.read()`
- D) `fs.open()`

**Q2.** What does `path.join(__dirname, 'data', 'users.json')` do?
- A) Creates a new directory
- B) Constructs a cross-platform file path
- C) Reads a JSON file
- D) Joins strings together

**Q3.** How many times will "Hello" be printed?
```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('greet', () => console.log('Hello'));
emitter.on('greet', () => console.log('Hello'));
emitter.emit('greet');
```
- A) 0
- B) 1
- C) 2
- D) Error

**Q4.** What's the main advantage of using `fs.readFile()` over `fs.readFileSync()`?
- A) It's faster
- B) It doesn't block the event loop
- C) It uses less memory
- D) It's easier to use

**Q5.** Which of the following is true about EventEmitter?
- A) Events are processed synchronously
- B) You can only have one listener per event
- C) You must emit events before adding listeners
- D) Listeners execute in the order they were registered

### Coding Challenges

**Challenge 1: File Logger**
Create a logging system that:
- Writes log messages to a file with timestamps
- Uses the path module to store logs in `logs/YYYY-MM-DD.log`
- Emits events when logs are written: `logCreated`, `logWritten`, `logError`
- Handles errors gracefully

**Challenge 2: Directory Watcher**
Create a program that:
- Monitors a directory for new files
- Emits an event when a new file is detected
- Reads the file and processes it based on its extension
- Uses path utilities to organize processed files

<details>
<summary>Click to see answers</summary>

**Answers:**
- Q1: B
- Q2: B
- Q3: C
- Q4: B
- Q5: D

</details>

---

## Topic 4: Streams - Practice Tasks

### Task 4.1: Reading Large Files
Create a script that:
1. Reads a large file using `fs.createReadStream()`
2. Counts the number of lines in the file
3. Displays progress (bytes read) as chunks arrive
4. Compares memory usage vs `readFileSync()`

**Expected Skills**: `createReadStream()`, handling `data` and `end` events

### Task 4.2: File Copy with Streams
Create a file copy utility using streams:
1. Read from source file using readable stream
2. Write to destination using writable stream
3. Use `.pipe()` to connect them
4. Display progress percentage
5. Handle errors appropriately

**Expected Skills**: `pipe()`, readable/writable streams, error handling

### Task 4.3: Transform Stream
Create a custom transform stream that:
1. Reads a CSV file line by line
2. Transforms each line to uppercase
3. Adds a line number prefix
4. Writes to a new file
5. Reports total lines processed

**Expected Skills**: `Transform` stream, `transform()` method, data transformation

### Task 4.4: Stream Compression
Create a script that:
1. Reads a text file
2. Compresses it using `zlib.createGzip()`
3. Saves the compressed file
4. Then decompresses it back
5. Verifies the content matches the original

**Expected Skills**: `pipe()`, `zlib` module, chaining streams

---

## Topic 5: HTTP Module - Practice Tasks

### Task 5.1: Simple Web Server
Create an HTTP server that:
1. Listens on port 3000
2. Responds with "Hello, World!" on the root path `/`
3. Returns your name on `/about`
4. Returns current date/time on `/time`
5. Returns 404 for any other path

**Expected Skills**: `http.createServer()`, request routing, response headers

### Task 5.2: JSON API
Create a REST API that:
1. GET `/users` - Returns array of user objects
2. GET `/users/:id` - Returns a specific user (parse URL)
3. POST `/users` - Accepts JSON body and adds a user
4. Returns appropriate status codes (200, 201, 404, 400)
5. Sets correct `Content-Type` headers

**Expected Skills**: Request methods, JSON handling, status codes, URL parsing

### Task 5.3: File Server
Create a server that:
1. Serves HTML files from a `public` directory
2. Handles different file types (`.html`, `.css`, `.js`, `.json`)
3. Sets appropriate `Content-Type` for each file type
4. Returns 404 if file doesn't exist
5. Uses streams to serve files efficiently

**Expected Skills**: Static file serving, MIME types, combining streams with HTTP

### Task 5.4: HTTP Client
Create a script that:
1. Makes an HTTP GET request to a public API (e.g., JSONPlaceholder)
2. Parses the JSON response
3. Displays formatted output
4. Handles network errors
5. Makes a POST request with data

**Expected Skills**: `http.request()`, handling responses, JSON parsing

---

## Topic 6: NPM and Packages - Practice Tasks

### Task 6.1: Project Setup
Create a new Node.js project:
1. Initialize with `npm init`
2. Install `express`, `dotenv`, and `nodemon` as dependencies
3. Configure scripts: `start`, `dev`, `test`
4. Create a `.gitignore` file
5. Document your dependencies in README

**Expected Skills**: NPM basics, package.json configuration

### Task 6.2: Using Third-Party Packages
Create a utility app using popular packages:
1. Use `axios` to fetch data from an API
2. Use `lodash` to manipulate the data (filter, map, sort)
3. Use `chalk` to display colorful console output
4. Use `moment` or `date-fns` to format dates
5. Handle errors appropriately

**Expected Skills**: Installing packages, reading documentation, using package APIs

### Task 6.3: Express API
Create a simple Express application:
1. Install Express
2. Create routes: GET, POST, PUT, DELETE
3. Use middleware for logging
4. Add body parsing middleware
5. Implement error handling middleware

**Expected Skills**: Express basics, middleware, routing

### Task 6.4: Package Scripts
Set up useful NPM scripts:
1. `start` - Run the app in production mode
2. `dev` - Run with nodemon for development
3. `test` - Run tests (create a simple test)
4. `lint` - Run ESLint (install and configure)
5. `format` - Run Prettier for code formatting

**Expected Skills**: NPM scripts, development tools, automation

---

## Quiz 2: Streams, HTTP, and NPM

### Multiple Choice Questions

**Q1.** What is the main advantage of using streams?
- A) Faster processing
- B) Memory efficiency with large files
- C) Easier to code
- D) Better error handling

**Q2.** What does the `.pipe()` method do?
- A) Connects a readable stream to a writable stream
- B) Filters stream data
- C) Creates a new stream
- D) Closes a stream

**Q3.** Which HTTP status code indicates success?
- A) 404
- B) 500
- C) 200
- D) 301

**Q4.** What is the difference between dependencies and devDependencies?
- A) No difference
- B) devDependencies are only needed during development
- C) dependencies are faster
- D) devDependencies are optional

**Q5.** How do you import a package installed via NPM?
- A) `import packageName from 'package'`
- B) `const packageName = require('packageName')`
- C) `include packageName`
- D) `use packageName`

### Coding Challenges

**Challenge 1: Log Analyzer**
Create a program that:
- Uses streams to read a large log file
- Counts occurrences of different log levels (INFO, WARNING, ERROR)
- Uses a transform stream to filter only ERROR logs
- Writes errors to a separate file
- Displays statistics at the end

**Challenge 2: Mini REST API**
Create a complete REST API using raw HTTP module:
- CRUD operations for a resource (e.g., tasks)
- Store data in a JSON file
- Use streams to read/write the file
- Proper error handling and status codes
- Content-Type headers

**Challenge 3: Package Integration**
Create a weather app CLI:
- Use `axios` to fetch weather from an API
- Use `commander` for command-line arguments
- Use `chalk` for colored output
- Use `dotenv` for API key management
- Cache results to avoid repeated API calls

<details>
<summary>Click to see answers</summary>

**Answers:**
- Q1: B
- Q2: A
- Q3: C
- Q4: B
- Q5: B

</details>

---

## Topic 7: Environment Variables - Practice Tasks

### Task 7.1: Basic Environment Variables
Create a script that:
1. Reads environment variables for database connection
2. Uses default values if variables are not set
3. Displays configuration on startup
4. Hides sensitive information (e.g., passwords)

**Expected Skills**: `process.env`, default values, security awareness

### Task 7.2: Dotenv Configuration
Create a project using dotenv:
1. Install `dotenv` package
2. Create a `.env` file with various configurations
3. Create a `.env.example` file (without sensitive data)
4. Load environment variables at startup
5. Use them in your application

**Expected Skills**: dotenv package, `.env` files, security best practices

### Task 7.3: Multi-Environment Setup
Create configuration for different environments:
1. Create config files: `config.dev.js`, `config.prod.js`
2. Load appropriate config based on `NODE_ENV`
3. Validate required environment variables
4. Throw errors if critical variables are missing

**Expected Skills**: Environment-specific configuration, validation

### Task 7.4: Secure Configuration Manager
Create a configuration module that:
1. Loads environment variables
2. Validates required variables
3. Provides typed access to configuration values
4. Sanitizes logs to prevent exposing secrets
5. Exports a frozen configuration object

**Expected Skills**: Module design, validation, security, immutability

---

## Topic 8: Asynchronous Patterns - Practice Tasks

### Task 8.1: Callback Pattern
Create functions using callbacks:
1. A function that reads a file and processes its content
2. Handles errors in callbacks
3. Chains multiple asynchronous operations
4. Experiences "callback hell" firsthand

**Expected Skills**: Callback functions, error-first callbacks, callback chaining

### Task 8.2: Promises
Convert callback-based code to promises:
1. Create a promise-based file reader
2. Chain multiple promise operations
3. Use `.then()` and `.catch()`
4. Understand promise states (pending, fulfilled, rejected)

**Expected Skills**: Creating promises, promise chaining, error handling

### Task 8.3: Async/Await
Refactor promise code using async/await:
1. Create async functions
2. Use try/catch for error handling
3. Understand the difference from promise chains
4. Handle multiple async operations sequentially

**Expected Skills**: `async`/`await` syntax, error handling, control flow

### Task 8.4: Parallel vs Sequential
Create a comparison script:
1. Fetch data from 5 different URLs sequentially
2. Fetch the same data in parallel using `Promise.all()`
3. Measure and compare execution time
4. Handle errors in parallel operations (`Promise.allSettled()`)

**Expected Skills**: `Promise.all()`, `Promise.allSettled()`, performance optimization

### Task 8.5: Event Loop Understanding
Create a script demonstrating execution order:
1. Mix synchronous code, `setTimeout()`, `setImmediate()`, `process.nextTick()`
2. Add promises to the mix
3. Predict the output order
4. Verify your understanding of the event loop

**Expected Skills**: Event loop phases, execution priority, microtasks vs macrotasks

---

## Topic 9: Modules and Exports - Practice Tasks

### Task 9.1: Basic Module Creation
Create a math utilities module:
1. Export functions: `add`, `subtract`, `multiply`, `divide`
2. Include a constant: `PI`
3. Import and use in another file
4. Use destructuring to import specific functions

**Expected Skills**: `module.exports`, `require()`, destructuring

### Task 9.2: Class Module
Create a `User` class module:
1. Define a User class with properties and methods
2. Export the class
3. Import and instantiate in another file
4. Create multiple user instances

**Expected Skills**: Exporting classes, instantiation, module patterns

### Task 9.3: Module Organization
Create a modular application structure:
```
app/
  ├── models/
  │   ├── user.js
  │   └── product.js
  ├── services/
  │   ├── database.js
  │   └── email.js
  ├── utils/
  │   ├── logger.js
  │   └── validator.js
  └── index.js
```
- Each module exports specific functionality
- `index.js` imports and uses all modules
- Demonstrate proper separation of concerns

**Expected Skills**: Project structure, module organization, separation of concerns

### Task 9.4: ES6 Modules
Convert CommonJS modules to ES6:
1. Use `export` and `import` syntax
2. Configure package.json for ES modules
3. Use named exports and default exports
4. Import modules in different ways

**Expected Skills**: ES6 module syntax, modern JavaScript, module types

---

## Quiz 3: Environment Variables, Async Patterns, and Modules

### Multiple Choice Questions

**Q1.** Where should you store API keys and passwords?
- A) Directly in the code
- B) In environment variables
- C) In comments
- D) In the repository

**Q2.** What is the output of this code?
```js
console.log('A');
setTimeout(() => console.log('B'), 0);
Promise.resolve().then(() => console.log('C'));
console.log('D');
```
- A) A, B, C, D
- B) A, D, B, C
- C) A, D, C, B
- D) A, B, D, C

**Q3.** What does `async` keyword do to a function?
- A) Makes it run faster
- B) Makes it return a Promise
- C) Makes it wait for other functions
- D) Makes it synchronous

**Q4.** What's the difference between `module.exports` and `exports`?
- A) No difference
- B) `exports` is a reference to `module.exports`
- C) `exports` is faster
- D) `module.exports` is deprecated

**Q5.** How do you handle errors in async/await?
- A) `.catch()`
- B) `try/catch`
- C) `error` callback
- D) `on('error')`

**Q6.** What is the purpose of `.env` file?
- A) Store environment-specific configuration
- B) Store application code
- C) Store test cases
- D) Store documentation

**Q7.** What does `Promise.all()` do?
- A) Waits for any promise to resolve
- B) Waits for all promises to resolve
- C) Creates multiple promises
- D) Cancels all promises

**Q8.** Which executes first in the event loop?
- A) `setTimeout()`
- B) `setImmediate()`
- C) `Promise.resolve().then()`
- D) They execute randomly

### Coding Challenges

**Challenge 1: Configuration Manager**
Create a complete configuration system:
- Load environment variables from `.env`
- Support multiple environments (dev, staging, prod)
- Validate all required configurations
- Export a module with configuration access methods
- Include proper error handling

**Challenge 2: Async Data Pipeline**
Create a data processing pipeline:
- Fetch data from multiple sources (use mock functions)
- Process data asynchronously
- Transform data through multiple steps
- Handle errors at each step
- Use async/await with proper error handling
- Measure total execution time

**Challenge 3: Modular Application**
Build a mini task management system:
- Create modules: Task model, TaskManager service, Database mock, Logger
- Use proper exports and imports
- Implement async operations for CRUD
- Use environment variables for configuration
- Demonstrate the event loop with async operations
- Follow best practices for module organization

<details>
<summary>Click to see answers</summary>

**Answers:**
- Q1: B
- Q2: C (Synchronous code first: A, D; then microtasks: C; then macrotasks: B)
- Q3: B
- Q4: B
- Q5: B
- Q6: A
- Q7: B
- Q8: C (Promises are microtasks, execute before macrotasks)

</details>

---

## Final Challenge Project

### Project: Task Management API

Build a complete Task Management system that incorporates ALL topics covered:

#### Requirements:

1. **File System & Path**
   - Store tasks in a JSON file using proper path construction
   - Implement backup functionality
   - Create logs directory with dated log files

2. **Events**
   - Emit events for: task created, updated, deleted, completed
   - Create event listeners for logging and notifications
   - Use EventEmitter for application lifecycle events

3. **Streams**
   - Export tasks to CSV using streams
   - Import tasks from large CSV files efficiently
   - Stream logs to file

4. **HTTP Module**
   - Create REST API endpoints:
     - GET /tasks - List all tasks
     - GET /tasks/:id - Get single task
     - POST /tasks - Create task
     - PUT /tasks/:id - Update task
     - DELETE /tasks/:id - Delete task
   - Proper status codes and error handling

5. **NPM & Packages**
   - Use Express for routing (optional: compare with raw HTTP)
   - Use a validation library (joi, validator)
   - Use a date library (date-fns, moment)
   - Use nodemon for development

6. **Environment Variables**
   - Configure port, file paths, log level
   - Support different environments
   - Use dotenv for local development

7. **Async Patterns**
   - All file operations should be async
   - Use async/await throughout
   - Handle concurrent requests properly
   - Implement request queueing or rate limiting

8. **Modules**
   - Organized structure:
     ```
     task-api/
       ├── models/
       │   └── Task.js
       ├── services/
       │   ├── TaskService.js
       │   └── FileService.js
       ├── utils/
       │   ├── logger.js
       │   └── validator.js
       ├── routes/
       │   └── tasks.js
       ├── config/
       │   └── index.js
       ├── .env
       ├── .env.example
       ├── package.json
       └── server.js
     ```

#### Bonus Features:
- Add search and filter functionality
- Implement task categories/tags
- Add user authentication (basic)
- Create a simple CLI client
- Add unit tests
- Implement request logging middleware
- Add API documentation

#### Evaluation Criteria:
- ✅ Correct use of all Node.js fundamentals
- ✅ Proper error handling
- ✅ Clean code organization
- ✅ Efficient use of streams for large data
- ✅ Proper async/await usage
- ✅ Security (no hardcoded secrets)
- ✅ Event-driven architecture
- ✅ RESTful API design

### Getting Started:
1. Plan your application structure
2. Set up package.json and dependencies
3. Create configuration system
4. Build models and services
5. Implement HTTP server and routes
6. Add events and logging
7. Test thoroughly
8. Document your code

**Good luck! This project will demonstrate your mastery of Node.js fundamentals!** 🚀

---

## Additional Resources

### Practice Tips:
1. **Don't skip the basics** - Master file system and paths before moving to complex topics
2. **Type code manually** - Don't copy-paste, you learn by typing
3. **Break things** - Intentionally cause errors to understand error handling
4. **Read documentation** - Official Node.js docs are excellent
5. **Build small projects** - Apply concepts to real-world scenarios

### Recommended Practice Order:
1. Complete all individual topic tasks first
2. Take each quiz to identify weak areas
3. Revisit topics where you scored low
4. Combine concepts in small projects
5. Finally, tackle the final challenge project

### Learning Resources:
- Official Node.js Documentation: https://nodejs.org/docs
- Node.js GitHub: https://github.com/nodejs/node
- NPM Package Registry: https://www.npmjs.com
- MDN Web Docs (JavaScript): https://developer.mozilla.org

### When You Get Stuck:
1. Read error messages carefully
2. Console.log intermediate values
3. Check the documentation
4. Break the problem into smaller parts
5. Search for similar issues online
6. Take a break and come back fresh

---

**Remember: The key to mastering Node.js is consistent practice and building real projects!** 💪
