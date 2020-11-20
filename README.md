# Wag! JavaScript Style Guide 

Other Style Guides

- [React](https://github.com/johnsantos-wag/wag-javascript-style-guide/tree/master/react)

## Table of Contents

1. [Variables](#variables)
2. [Functions](#functions)
3. [Types](#types)
4. [Accessors](#accessors)
5. [Naming conventions](#naming-conventions)
6. [Modules](#modules)

## Variables:

- Prefer `const` over `let` or `var`

> You can use `let` if it's really needed but as much as possible use `const` as it provides better immutability. This removes the unintended mutation or side-effects. Sometimes restructuring or rethinking the approach is needed to adjust it

```js
// bad
var foo = 'foo';

// ok
let foo = 'foo';

// good
const foo = 'foo';

```

- Put the environment variables in a constant file instead of accessing directly

```js
// bad

// foo.js
function getUsers() {
  const baseUrl = process.env.BASE_URL || 'https://my-default-api.com';
  const url = process.env.BASE_URL + '/api/users';
  return fetch(url);
}

// good

// endpoints.constants.js
export const BASE_URL = process.env.BASE_URL || 'https://my-default-api.com';

// foo.js
function getUsers() {
  const url = BASE_URL + '/api/users';
  return fetch(url);
}

```

## Functions:

- Use function expression instead of function declaration

> This avoids function hoisted which can be confusing to track on

```js
// bad
function getDogs() {}

// good
const getDogs = () => {}
```

## Types:

- Use `type` when creating a typing definition
> both works but `type` is much shorter to work on

```ts
// bad 
interface Shape {}

// good
type Shape = {}
```

## Accessors:

- When accessing a value, use a guard style or an early-return approach

> This allows the code to exclude the error cases early on and provides a clear path to the intent

```js
// bad
const person = {
  age: 0,
};

if (person.age) { 
  ...
} else {
  throw new Error('Invalid age');
}

// good
const person = {
  age: 0,
};

if (!person.age) {
  throw new Error('Invalid age');  
}
```

## Naming conventions:

- Acronyms should be in PascalCase or camelCase (https://stackoverflow.com/questions/15526107/acronyms-in-camelcase)

> When using acronyms, use Pascal case or camel case for acronyms more than two characters long. 

```js
// bad 
const imageURL = '';

const sendSMS = () => {};

const systemIo = {};

export const apiKey = 'qwerty'; // constant

// good
const imageUrl = '';

const sendSms = () => {};

const systemIO = {}; // if the acrnonym consist of two letters, uppercase

export const API_KEY = 'qwerty'; //constant
```

## Iterators

- Prefer functional loops when iterating instead of imperative loops (`for loop`, `for in`, `while loop`, etc)

> This will prevent creating side-effects that is often the cause of bugs and this also provides better readability

```js
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}
sum === 15;

// good
let sum = 0;
numbers.forEach((num) => {
  sum += num;
});
sum === 15;

// best (use the functional force)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;

// bad
const increasedByOne = [];
for (let i = 0; i < numbers.length; i++) {
  increasedByOne.push(numbers[i] + 1);
}

// good
const increasedByOne = [];
numbers.forEach((num) => {
  increasedByOne.push(num + 1);
});

// best (keeping it functional)
const increasedByOne = numbers.map((num) => num + 1);
```


## Modules 

- Use `import`/`export` over `require`

```js
// bad
const fs = require('fs');
const { writeFile } = require('fs');

// good
import { writeFile } from 'fs';
```

- Use named export when exporting 

```js

// bad
const formatUsername = () => {};
export default formatUsername;

// good
export const formatUsername = () => {};
```

- Separate `import` statements or group by "type" and add whitespaces in between

> A type can be a third party package, utils path, assets path  

```js

// bad
import _ from 'lodash';
import { formatName } from './utils';
import moment from 'moment';
import dogIcon from '../assets/dog.svg';

// ok
import _ from 'lodash';
import moment from 'moment';
import { formatName } from './utils';
import dogIcon from '../assets/dog.svg';

// good
import _ from 'lodash';
import moment from 'moment';

import { formatName } from './utils';

import dogIcon from '../assets/dog.svg';
```
