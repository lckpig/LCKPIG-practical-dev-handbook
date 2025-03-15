# RESTful APIs

REST (Representational State Transfer) is an architectural style for designing networked applications. RESTful APIs use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.

## Key Principles of REST

1. **Stateless** - The server doesn't store client state between requests
2. **Client-Server Architecture** - Separates client concerns from server concerns
3. **Uniform Interface** - Resources have consistent identifiers (URIs)
4. **Cacheable** - Responses can be cached to improve performance
5. **Layered System** - Client doesn't know if it's connected directly to the server
6. **Resource-Based** - Everything is a resource identified by a unique URI

## HTTP Methods in REST

RESTful APIs use standard HTTP methods:

| Method | Purpose               | Example                                     |
| ------ | --------------------- | ------------------------------------------- |
| GET    | Retrieve data         | GET /api/users (get all users)              |
| POST   | Create new data       | POST /api/users (create a user)             |
| PUT    | Update/replace data   | PUT /api/users/1 (update user 1)            |
| PATCH  | Partially update data | PATCH /api/users/1 (update specific fields) |
| DELETE | Remove data           | DELETE /api/users/1 (delete user 1)         |

## Building a RESTful API

Here's an example of a simple RESTful API built with Node.js and Express:

```javascript
const express = require('express');
const router = express.Router();

// In-memory "database"
let books = [
  { id: 1, title: "The Great Gatsby", author: "F. Scott Fitzgerald", year: 1925 },
  { id: 2, title: "To Kill a Mockingbird", author: "Harper Lee", year: 1960 },
  { id: 3, title: "1984", author: "George Orwell", year: 1949 }
];

// GET all books
router.get('/books', (req, res) => {
  res.json(books);
});

// GET a specific book
router.get('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const book = books.find(b => b.id === bookId);
  
  if (!book) {
    return res.status(404).json({ error: "Book not found" });
  }
  
  res.json(book);
});

// POST a new book
router.post('/books', (req, res) => {
  const { title, author, year } = req.body;
  
  if (!title || !author) {
    return res.status(400).json({ error: "Title and author are required" });
  }
  
  const newId = books.length > 0 ? Math.max(...books.map(b => b.id)) + 1 : 1;
  const newBook = { id: newId, title, author, year: year || null };
  
  books.push(newBook);
  res.status(201).json(newBook);
});

// PUT (update) a book
router.put('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const bookIndex = books.findIndex(b => b.id === bookId);
  
  if (bookIndex === -1) {
    return res.status(404).json({ error: "Book not found" });
  }
  
  const { title, author, year } = req.body;
  
  if (!title || !author) {
    return res.status(400).json({ error: "Title and author are required" });
  }
  
  books[bookIndex] = { id: bookId, title, author, year: year || null };
  res.json(books[bookIndex]);
});

// DELETE a book
router.delete('/books/:id', (req, res) => {
  const bookId = parseInt(req.params.id);
  const bookIndex = books.findIndex(b => b.id === bookId);
  
  if (bookIndex === -1) {
    return res.status(404).json({ error: "Book not found" });
  }
  
  books.splice(bookIndex, 1);
  res.status(204).end();
});

module.exports = router;
```

## Status Codes in REST APIs

Proper HTTP status codes are essential for RESTful APIs:

| Code | Category     | Examples                                           |
| ---- | ------------ | -------------------------------------------------- |
| 2xx  | Success      | 200 OK, 201 Created, 204 No Content                |
| 3xx  | Redirection  | 301 Moved Permanently, 304 Not Modified            |
| 4xx  | Client Error | 400 Bad Request, 401 Unauthorized, 404 Not Found   |
| 5xx  | Server Error | 500 Internal Server Error, 503 Service Unavailable |

## Testing RESTful APIs

You can test APIs using tools like Postman, cURL, or fetch:

```javascript
// Testing with JavaScript fetch API
async function testApi() {
  try {
    // GET all books
    const response = await fetch('http://localhost:3000/api/books');
    const books = await response.json();
    console.log('All books:', books);
    
    // POST a new book
    const newBook = {
      title: "The Hobbit",
      author: "J.R.R. Tolkien",
      year: 1937
    };
    
    const createResponse = await fetch('http://localhost:3000/api/books', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(newBook)
    });
    
    const createdBook = await createResponse.json();
    console.log('Created book:', createdBook);
    
  } catch (error) {
    console.error('API Error:', error);
  }
}

testApi();
```

## RESTful API Security Best Practices

Security is a critical aspect of API design. Here are some best practices for securing your RESTful APIs:

### Authentication and Authorization

Implement robust authentication and authorization:

```javascript
// Example of JWT authentication middleware in Express
const jwt = require('jsonwebtoken');

function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN
  
  if (!token) {
    return res.status(401).json({ error: "Unauthorized" });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) {
      return res.status(403).json({ error: "Forbidden" });
    }
    
    req.user = user;
    next();
  });
}

// Protected route
router.get('/user/profile', authenticateToken, (req, res) => {
  // Only accessible with valid JWT
  res.json({ user: req.user });
});
```

### HTTPS Everywhere

Always use HTTPS to encrypt data in transit:

```javascript
// In your server setup (e.g., Node.js with Express)
const https = require('https');
const fs = require('fs');
const express = require('express');
const app = express();

// SSL certificate options
const options = {
  key: fs.readFileSync('private-key.pem'),
  cert: fs.readFileSync('certificate.pem')
};

// Create HTTPS server
https.createServer(options, app).listen(443, () => {
  console.log('HTTPS server running on port 443');
});

// Redirect HTTP to HTTPS
const http = require('http');
http.createServer((req, res) => {
  res.writeHead(301, { "Location": "https://" + req.headers['host'] + req.url });
  res.end();
}).listen(80);
```

### Input Validation

Always validate and sanitize input data:

```javascript
// Using a validation library like Joi
const Joi = require('joi');

// Schema validation for book creation
const bookSchema = Joi.object({
  title: Joi.string().min(1).max(100).required(),
  author: Joi.string().min(1).max(100).required(),
  year: Joi.number().integer().min(1000).max(new Date().getFullYear())
});

router.post('/books', (req, res) => {
  // Validate request body
  const validation = bookSchema.validate(req.body);
  
  if (validation.error) {
    return res.status(400).json({ 
      error: validation.error.details[0].message 
    });
  }
  
  // Proceed with creating the book...
});
```

### Rate Limiting

Protect your API from abuse with rate limiting:

```javascript
// Using Express Rate Limit middleware
const rateLimit = require('express-rate-limit');

// Create limiter middleware
const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: {
    error: "Too many requests, please try again after 15 minutes"
  }
});

// Apply to all API endpoints
app.use('/api/', apiLimiter);

// Or apply to specific routes
app.use('/api/auth/', rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 5, // 5 login attempts per hour
  message: {
    error: "Too many login attempts, please try again after an hour"
  }
}));
```

### API Documentation

Document your API thoroughly using tools like Swagger/OpenAPI:

```yaml
# OpenAPI specification example (openapi.yaml)
openapi: 3.0.0
info:
  title: Book API
  description: API for managing books
  version: 1.0.0
paths:
  /books:
    get:
      summary: Get all books
      responses:
        '200':
          description: List of books
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Book'
    post:
      summary: Create a new book
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/BookInput'
      responses:
        '201':
          description: Book created
components:
  schemas:
    Book:
      type: object
      properties:
        id:
          type: integer
        title:
          type: string
        author:
          type: string
        year:
          type: integer
    BookInput:
      type: object
      required:
        - title
        - author
      properties:
        title:
          type: string
        author:
          type: string
        year:
          type: integer
```

RESTful APIs provide a standardized way for applications to communicate, making them essential for modern web and mobile application development. 