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
