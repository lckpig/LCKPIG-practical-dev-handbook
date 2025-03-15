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

Numbers and strings form the basis of most data we work with in programming, from user input to calculation results to displayed output. 