# Variables and Data Types

Variables and data types form the foundation of programming. They allow us to store, manipulate, and organize information in our programs.

## What Are Variables?

Variables are containers that hold data. Think of them as labeled boxes where you can store different types of information and retrieve them later by their name.

## Common Data Types

Most programming languages provide several built-in data types:

1. **Primitive Types**
   - Numbers (integers, floating-point)
   - Strings (text)
   - Booleans (true/false)
   - Null/None/nil (absence of value)

2. **Composite Types**
   - Arrays/Lists
   - Objects/Dictionaries/Maps
   - Sets
   - Tuples

## Working with Variables

Here's a simple example showing how to declare and use variables in JavaScript:

```javascript
// Declaring variables
let userName = "Alice";        // String
const userAge = 28;            // Number (integer)
let isActive = true;           // Boolean
let accountBalance = 1250.75;  // Number (floating-point)
let userPreferences = null;    // Null value

// Using variables
console.log(`User ${userName} is ${userAge} years old.`);

// Modifying variables
userName = "Alice Smith";
console.log(`Updated name: ${userName}`);

// Working with numbers
let newBalance = accountBalance + 100;
console.log(`New balance: $${newBalance}`);
```

In the following pages, we'll explore different data types in more detail and learn how to effectively work with them in your programs. 