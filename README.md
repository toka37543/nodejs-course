# Lecture cheat sheet

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
