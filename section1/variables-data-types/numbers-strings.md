# Numbers and Strings

Numbers and strings are two of the most commonly used primitive data types in programming.

## Working with Numbers

Programming languages typically support multiple types of numbers:

### Integers

Whole numbers without decimal points:

```python
# Python integers
age = 30
count = -5
population = 7_900_000_000  # Underscores for readability
```

### Floating-Point Numbers

Numbers with decimal points:

```python
# Python floating-point numbers
price = 19.99
temperature = -2.5
pi_approximate = 3.14159
scientific = 6.02e23  # Scientific notation
```

### Number Operations

```javascript
// JavaScript number operations
let x = 10;
let y = 3;

// Basic arithmetic
console.log(x + y);  // Addition: 13
console.log(x - y);  // Subtraction: 7
console.log(x * y);  // Multiplication: 30
console.log(x / y);  // Division: 3.3333...
console.log(x % y);  // Modulus (remainder): 1
console.log(x ** y); // Exponentiation: 1000

// Increment/decrement
x++;  // x is now 11
y--;  // y is now 2
```

## Working with Strings

Strings are sequences of characters used to represent text.

### Creating Strings

```java
// Java string creation
String singleQuotes = 'Hello';
String doubleQuotes = "World";
String multiLine = """
    This is a multi-line
    string in Java.
    It preserves formatting.
    """;
```

### String Operations

```python
# Python string operations
first_name = "John"
last_name = "Doe"

# Concatenation
full_name = first_name + " " + last_name  # "John Doe"

# String methods
uppercase = full_name.upper()  # "JOHN DOE"
lowercase = full_name.lower()  # "john doe"
replaced = full_name.replace("Doe", "Smith")  # "John Smith"

# String length
name_length = len(full_name)  # 8

# String indexing
first_char = full_name[0]  # "J"
last_char = full_name[-1]  # "e"

# Substring (slicing)
first_three = full_name[:3]  # "Joh"
```

### String Interpolation

Modern languages provide convenient ways to embed variables within strings:

```javascript
// JavaScript template literals
const name = "Alice";
const age = 30;
const message = `Hello, my name is ${name} and I am ${age} years old.`;
```

```python
# Python f-strings
name = "Bob"
age = 25
message = f"Hello, my name is {name} and I am {age} years old."
```

## Comparative Table: Numbers vs Strings

Here's a detailed comparison between the Number and String data types:

| Feature              | Numbers                                  | Strings                                          |
| -------------------- | ---------------------------------------- | ------------------------------------------------ |
| **Definition**       | Numeric values used for calculations     | Sequences of characters used for text            |
| **Common Types**     | Integers, Floating-point, BigInt         | Single-line, Multi-line                          |
| **Memory Usage**     | Typically 4-8 bytes for standard numbers | Variable (depends on string length)              |
| **Operations**       | +, -, \*, /, %, \*\*, ++, --             | Concatenation, Slicing, Searching                |
| **Methods**          | toFixed(), parseInt(), parseFloat()      | toUpperCase(), toLowerCase(), replace(), split() |
| **Conversion**       | toString(), Number(), parseInt()         | parseInt(), parseFloat(), Number()               |
| **Comparison**       | ==, ===, <, >, <=, >=                    | ==, ===, localeCompare()                         |
| **Common Errors**    | Division by zero, Precision errors       | Encoding issues, Escape sequence errors          |
| **Special Values**   | Infinity, -Infinity, NaN                 | Empty string ("")                                |
| **Immutability**     | Immutable (primitive type)               | Immutable in most languages                      |
| **Common Use Cases** | Calculations, Counting, Measurements     | User input, Text display, Data formatting        |
| **Type Coercion**    | Automatic in loosely typed languages     | Can be coerced to numbers or booleans            |
| **Example Literals** | 42, 3.14, 1e6                            | "Hello", 'World', `Template ${var}`              |

Numbers and strings form the basis of most data we work with in programming, from user input to calculation results to displayed output.

## Best Practices for Working with Numbers and Strings

When working with numbers and strings in your code, following these best practices can help avoid common pitfalls and make your code more maintainable.

### For Numbers

1. **Be Careful with Floating-Point Precision**

```javascript
// Avoid direct equality comparison with floating points
// BAD
if (0.1 + 0.2 === 0.3) { // This will be false!
  console.log("Equal");
}

// GOOD
const isEqual = Math.abs((0.1 + 0.2) - 0.3) < Number.EPSILON;
console.log(isEqual); // true
```

2. **Use Numeric Separators for Readability**

```javascript
// Without separators
const largeNumber = 1000000000;

// With separators
const largeNumberReadable = 1_000_000_000; // More readable
```

3. **Avoid NaN Bugs with Validation**

```javascript
// Check if a value is a valid number
function safeMultiply(a, b) {
  if (isNaN(a) || isNaN(b)) {
    return 0; // Default value or throw an error
  }
  return a * b;
}
```

### For Strings

1. **Prefer Template Literals for Complex Strings**

```javascript
// Complex string concatenation
const name = 'John';
const items = ['apple', 'banana', 'orange'];

// BAD
const message = 'Hello, ' + name + '! You have ' + items.length + ' items: ' + items.join(', ') + '.';

// GOOD
const betterMessage = `Hello, ${name}! You have ${items.length} items: ${items.join(', ')}.`;
```

2. **Be Mindful of String Immutability**

```javascript
// Strings are immutable - methods return new strings
let greeting = 'hello';
greeting.toUpperCase(); // This doesn't change 'greeting'
console.log(greeting); // Still 'hello'

// You need to reassign
greeting = greeting.toUpperCase();
console.log(greeting); // Now 'HELLO'
```

3. **Use String Methods Instead of Regular Expressions When Possible**

```javascript
const sentence = 'The quick brown fox';

// Simple operations are faster with string methods
const hasWord = sentence.includes('fox'); // Cleaner than regex for simple searches
const position = sentence.indexOf('quick'); // 4
```

Numbers and strings form the basis of most data we work with in programming, from user input to calculation results to displayed output.
