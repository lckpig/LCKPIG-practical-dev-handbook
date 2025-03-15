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

RESTful APIs provide a standardized way for applications to communicate, making them essential for modern web and mobile application development. 