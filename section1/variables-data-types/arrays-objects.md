# Arrays and Objects

Arrays and objects are composite data types that allow you to store multiple values in a structured way. They form the foundation of complex data structures in programming.

## Arrays

Arrays are ordered collections of values that can be accessed by their position (index).

![Array Structure Visualization](https://i.ytimg.com/vi/55l-aZ7_F24/maxresdefault.jpg)

### Creating Arrays

```javascript
// JavaScript arrays
let fruits = ['apple', 'banana', 'orange'];
let numbers = [1, 2, 3, 4, 5];
let mixed = [42, 'hello', true, null, { name: 'John' }];
let empty = [];
let matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]; // 2D array
```

### Accessing Array Elements

```python
# Python array access
fruits = ['apple', 'banana', 'orange', 'mango', 'kiwi']

# Access by index (0-based)
first_fruit = fruits[0]  # 'apple'
last_fruit = fruits[-1]  # 'kiwi'

# Slicing
first_three = fruits[0:3]  # ['apple', 'banana', 'orange']
last_two = fruits[-2:]    # ['mango', 'kiwi']
```

### Modifying Arrays

```javascript
// JavaScript array modification
let colors = ['red', 'green', 'blue'];

// Adding elements
colors.push('yellow');        // Add to end: ['red', 'green', 'blue', 'yellow']
colors.unshift('purple');     // Add to beginning: ['purple', 'red', 'green', 'blue', 'yellow']

// Removing elements
let lastColor = colors.pop(); // Remove from end: 'yellow'
let firstColor = colors.shift(); // Remove from beginning: 'purple'

// Changing elements
colors[1] = 'emerald';        // ['red', 'emerald', 'blue']

// Splicing (removing/replacing elements)
colors.splice(1, 1, 'lime', 'teal'); // ['red', 'lime', 'teal', 'blue']
```

### Array Iteration

```javascript
// JavaScript array iteration
const numbers = [1, 2, 3, 4, 5];

// Using for loop
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}

// Using for...of loop
for (const number of numbers) {
  console.log(number);
}

// Using forEach method
numbers.forEach(number => {
  console.log(number);
});

// Using map to create a new array
const doubled = numbers.map(number => number * 2); // [2, 4, 6, 8, 10]

// Using filter to create a filtered array
const even = numbers.filter(number => number % 2 === 0); // [2, 4]
```

## Objects

Objects are collections of key-value pairs, where each key acts as a unique identifier for its associated value.

### Creating Objects

```javascript
// JavaScript objects
let person = {
  firstName: 'John',
  lastName: 'Doe',
  age: 30,
  email: 'john.doe@example.com',
  isEmployed: true,
  hobbies: ['reading', 'hiking', 'photography'],
  address: {
    street: '123 Main St',
    city: 'Anytown',
    zipCode: '12345'
  }
};

// Empty object
let empty = {};

// Using constructor
let user = new Object();
user.name = 'Jane';
user.age = 25;
```

### Accessing Object Properties

```javascript
// JavaScript object property access
const product = {
  id: 'PRD001',
  name: 'Laptop',
  price: 999.99,
  specs: {
    cpu: 'Intel i7',
    ram: '16GB',
    storage: '512GB SSD'
  }
};

// Dot notation
console.log(product.name);       // 'Laptop'
console.log(product.specs.cpu);  // 'Intel i7'

// Bracket notation
console.log(product['price']);   // 999.99

// With variables
const propertyName = 'name';
console.log(product[propertyName]); // 'Laptop'
```

### Modifying Objects

```javascript
// JavaScript object modification
const car = {
  make: 'Toyota',
  model: 'Camry',
  year: 2020
};

// Adding properties
car.color = 'blue';
car['fuelType'] = 'hybrid';

// Updating properties
car.year = 2021;
car['model'] = 'Corolla';

// Deleting properties
delete car.color;

// Checking if property exists
const hasModel = 'model' in car; // true
```

### Object Methods

```javascript
// JavaScript object methods
const calculator = {
  result: 0,
  add: function(a, b) {
    this.result = a + b;
    return this.result;
  },
  subtract(a, b) { // Shorthand method syntax
    this.result = a - b;
    return this.result;
  },
  multiply: (a, b) => a * b, // Arrow function
  getResult() {
    return this.result;
  }
};

calculator.add(5, 3);      // 8
calculator.subtract(10, 4); // 6
console.log(calculator.multiply(2, 3)); // 6
```

## Common Array Methods and Properties

Here's a comprehensive table of the most common array methods and properties:

| Method/Property        | Description                                     | Example                           | Return Value              |
| ---------------------- | ----------------------------------------------- | --------------------------------- | ------------------------- |
| **Properties**         |                                                 |                                   |
| `length`               | Returns the number of elements                  | `[1,2,3].length`                  | `3`                       |
| **Adding/Removing**    |                                                 |                                   |
| `push()`               | Adds elements to the end                        | `arr.push('x')`                   | New length                |
| `pop()`                | Removes the last element                        | `arr.pop()`                       | Removed element           |
| `unshift()`            | Adds elements to the beginning                  | `arr.unshift('x')`                | New length                |
| `shift()`              | Removes the first element                       | `arr.shift()`                     | Removed element           |
| `splice()`             | Changes array by removing/replacing elements    | `arr.splice(1, 2, 'a')`           | Array of removed items    |
| `slice()`              | Returns a portion of the array                  | `arr.slice(1, 3)`                 | New array                 |
| **Search and Access**  |                                                 |                                   |
| `indexOf()`            | Returns first index of an element               | `[1,2,3].indexOf(2)`              | `1` or `-1` if not found  |
| `lastIndexOf()`        | Returns last index of an element                | `[1,2,2].lastIndexOf(2)`          | `2` or `-1` if not found  |
| `includes()`           | Checks if array contains element                | `[1,2,3].includes(2)`             | `true` or `false`         |
| `find()`               | Returns first element that passes test          | `arr.find(x => x > 3)`            | Element or `undefined`    |
| `findIndex()`          | Returns index of first element that passes test | `arr.findIndex(x => x > 3)`       | Index or `-1`             |
| **Transformation**     |                                                 |                                   |
| `map()`                | Creates new array with results of callback      | `[1,2,3].map(x => x*2)`           | New array `[2,4,6]`       |
| `filter()`             | Creates new array with elements that pass test  | `[1,2,3].filter(x => x > 1)`      | New array `[2,3]`         |
| `reduce()`             | Reduces array to single value                   | `[1,2,3].reduce((a,b) => a+b, 0)` | Result of reduction `6`   |
| `forEach()`            | Executes callback for each element              | `[1,2,3].forEach(console.log)`    | `undefined`               |
| `sort()`               | Sorts the array                                 | `[3,1,2].sort()`                  | Sorted array              |
| `reverse()`            | Reverses array order                            | `[1,2,3].reverse()`               | Reversed array            |
| **Joining/Flattening** |                                                 |                                   |
| `concat()`             | Merges arrays                                   | `[1,2].concat([3,4])`             | New merged array          |
| `join()`               | Joins elements into string                      | `[1,2,3].join('-')`               | String `"1-2-3"`          |
| `flat()`               | Creates new array with all sub-arrays flattened | `[1,[2,[3]]].flat(2)`             | Flattened array `[1,2,3]` |
| `flatMap()`            | Maps then flattens                              | `[1,2].flatMap(x => [x, x*2])`    | New array `[1,2,2,4]`     |
| **Checking**           |                                                 |                                   |
| `every()`              | Tests if all elements pass test                 | `[1,2,3].every(x => x > 0)`       | `true` or `false`         |
| `some()`               | Tests if any element passes test                | `[1,2,3].some(x => x > 2)`        | `true` or `false`         |
| `isArray()`            | Checks if value is an array                     | `Array.isArray([1,2,3])`          | `true` or `false`         |
| **Conversion**         |                                                 |                                   |
| `from()`               | Creates array from array-like object            | `Array.from('123')`               | New array `['1','2','3']` |
| `of()`                 | Creates array from arguments                    | `Array.of(1, 2, 3)`               | New array `[1,2,3]`       |
| `toString()`           | Converts array to string                        | `[1,2,3].toString()`              | String `"1,2,3"`          |
| **ES2022+**            |                                                 |                                   |
| `at()`                 | Returns element at index (allows negative)      | `[1,2,3].at(-1)`                  | `3`                       |

## Advanced Array and Object Patterns

Modern JavaScript and other programming languages offer several advanced patterns for working with arrays and objects.

### Array Destructuring

Array destructuring allows you to extract values from arrays and assign them to variables:

```javascript
// Basic array destructuring
const colors = ['red', 'green', 'blue'];
const [firstColor, secondColor, thirdColor] = colors;
console.log(firstColor);  // 'red'

// Skipping elements
const [first, , third] = colors;
console.log(third);  // 'blue'

// With default values
const incomplete = ['one', 'two'];
const [a, b, c = 'three'] = incomplete;
console.log(c);  // 'three'

// Rest pattern
const numbers = [1, 2, 3, 4, 5];
const [head, ...tail] = numbers;
console.log(head);  // 1
console.log(tail);  // [2, 3, 4, 5]
```

### Object Destructuring

Similarly, object destructuring extracts properties from objects:

```javascript
// Basic object destructuring
const person = { name: 'Alice', age: 30, city: 'New York' };
const { name, age } = person;
console.log(name, age);  // 'Alice' 30

// Renaming variables
const { name: fullName, city: location } = person;
console.log(fullName, location);  // 'Alice' 'New York'

// Default values
const incomplete = { title: 'Developer' };
const { title, level = 'Junior' } = incomplete;
console.log(title, level);  // 'Developer' 'Junior'
```

### Spread Operator with Arrays and Objects

The spread operator (`...`) allows for powerful operations:

```javascript
// Combining arrays
const odds = [1, 3, 5];
const evens = [2, 4, 6];
const combined = [...odds, ...evens];
console.log(combined);  // [1, 3, 5, 2, 4, 6]

// Creating copies
const original = [1, 2, 3];
const copy = [...original];

// Merging objects
const defaults = { theme: 'light', fontSize: 12 };
const userPrefs = { fontSize: 14, showSidebar: true };
const settings = { ...defaults, ...userPrefs };
console.log(settings);  
// { theme: 'light', fontSize: 14, showSidebar: true }
```

Arrays and objects are foundational data structures in programming. Arrays are ideal for ordered collections of data, while objects excel at representing entities with named properties. Understanding these data types is crucial for effective programming in any language. 