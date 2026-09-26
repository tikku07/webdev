# Node.js: Requiring an Entire Directory (Notes)

## Folder Structure
project-directory/
│
├── app.js
└── shelter/
    ├── index.js
    ├── blue.js
    ├── janet.js
    └── sadie.js

## 1. Individual Modules (shelter/ directory)

### shelter/blue.js
module.exports = {
    name: 'blue',
    color: 'grey'
};

### shelter/janet.js
module.exports = {
    name: 'janet',
    color: 'orange Tabby'
};

### shelter/sadie.js
module.exports = {
    name: 'sadie',
    color: 'black'
};

## 2. Directory Entry Point (shelter/index.js)
const blue = require('./blue');
const sadie = require('./sadie');
const janet = require('./janet');

const allCats = [blue, sadie, janet];
module.exports = allCats;

## 3. Main Application (app.js in root)
const cats = require('./shelter');
console.log("required an entire directory", cats);

## Execution Command
$ node app.js
