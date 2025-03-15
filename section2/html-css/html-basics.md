# HTML Basics

HTML (HyperText Markup Language) is the standard markup language for creating web pages. It describes the structure of a web page using a series of elements or tags that define different parts of the content.

## Basic Structure of an HTML Document

Every HTML document follows a standard structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My First Web Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is my first web page.</p>
</body>
</html>
```

Let's break down the key components:

- `<!DOCTYPE html>`: Declares the document type and HTML version
- `<html>`: The root element of an HTML page
- `<head>`: Contains meta-information about the document
- `<meta>`: Provides metadata such as character encoding and viewport settings
- `<title>`: Specifies the title shown in the browser tab
- `<body>`: Contains the visible content of the page

## Common HTML Elements

### Headings

HTML provides six levels of headings:

```html
<h1>This is a Heading 1</h1>
<h2>This is a Heading 2</h2>
<h3>This is a Heading 3</h3>
<h4>This is a Heading 4</h4>
<h5>This is a Heading 5</h5>
<h6>This is a Heading 6</h6>
```

### Paragraphs and Text Formatting

```html
<p>This is a paragraph of text.</p>
<p>This is another paragraph with <strong>bold text</strong> and <em>italic text</em>.</p>
<p>You can also use <mark>highlighted text</mark> or <code>code formatting</code>.</p>
```

### Links

Links are created using the anchor (`<a>`) tag:

```html
<a href="https://www.example.com">Visit Example.com</a>
<a href="about.html">About Us</a>
<a href="#section1">Jump to Section 1</a>
```

### Images

Images are embedded using the `<img>` tag:

```html
<img src="image.jpg" alt="Description of the image">
<img src="https://example.com/logo.png" alt="Company Logo" width="200" height="100">
```

### Lists

HTML supports ordered, unordered, and definition lists:

```html
<!-- Unordered list -->
<ul>
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
</ul>

<!-- Ordered list -->
<ol>
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ol>

<!-- Definition list -->
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language, the standard for creating web pages.</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets, used for styling web pages.</dd>
</dl>
```

### Semantic Elements

Modern HTML includes semantic elements that help describe the content's meaning:

```html
<header>
    <nav>
        <a href="/">Home</a>
        <a href="/about">About</a>
        <a href="/contact">Contact</a>
    </nav>
</header>

<main>
    <article>
        <h2>Article Title</h2>
        <p>Article content...</p>
    </article>
    
    <aside>
        <h3>Related Information</h3>
        <p>Sidebar content...</p>
    </aside>
</main>

<footer>
    <p>&copy; 2023 My Website</p>
</footer>
```

## HTML Accessibility Best Practices

Creating accessible web content ensures that your websites can be used by people with disabilities. Here are some key HTML accessibility practices:

### Semantic HTML for Accessibility

Using semantic HTML properly improves accessibility significantly:

```html
<!-- BAD: Non-semantic markup -->
<div class="header">Site Title</div>
<div class="nav">
  <div class="nav-item"><a href="/">Home</a></div>
  <div class="nav-item"><a href="/about">About</a></div>
</div>
<div class="main-content">
  <div class="article">
    <div class="title">Article Title</div>
    <div class="text">Article content...</div>
  </div>
</div>
<div class="footer">Copyright 2023</div>

<!-- GOOD: Semantic markup -->
<header>
  <h1>Site Title</h1>
</header>
<nav>
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
  </ul>
</nav>
<main>
  <article>
    <h2>Article Title</h2>
    <p>Article content...</p>
  </article>
</main>
<footer>Copyright 2023</footer>
```

### ARIA Attributes

ARIA (Accessible Rich Internet Applications) attributes enhance accessibility when HTML semantics are insufficient:

```html
<!-- Menu button that controls a dropdown -->
<button aria-expanded="false" aria-controls="dropdown-menu">
  Menu
</button>
<ul id="dropdown-menu" aria-hidden="true">
  <li><a href="/products">Products</a></li>
  <li><a href="/services">Services</a></li>
</ul>

<!-- Progress indicator -->
<div role="progressbar" aria-valuenow="75" aria-valuemin="0" aria-valuemax="100">
  75% Complete
</div>
```

### Accessible Forms

Forms should be properly labeled and organized for accessibility:

```html
<form>
  <div>
    <!-- Label explicitly connected to input -->
    <label for="name-input">Name:</label>
    <input id="name-input" type="text" name="name" required aria-required="true">
  </div>
  
  <div>
    <!-- Fieldset groups related inputs -->
    <fieldset>
      <legend>Subscription Type</legend>
      
      <input id="sub-basic" type="radio" name="subscription" value="basic">
      <label for="sub-basic">Basic</label>
      
      <input id="sub-premium" type="radio" name="subscription" value="premium">
      <label for="sub-premium">Premium</label>
    </fieldset>
  </div>
  
  <!-- Error message with proper connection to field -->
  <div role="alert" aria-live="assertive" id="email-error" class="error-message"></div>
  
  <button type="submit">Subscribe</button>
</form>
```

### Keyboard Navigation Support

Ensure interactive elements work with keyboard navigation:

```html
<!-- Ensure clickable elements are focusable -->
<div tabindex="0" role="button" onclick="togglePanel()">Toggle Panel</div>

<!-- Skip navigation link for keyboard users -->
<a href="#main-content" class="skip-link">Skip to main content</a>
<!-- Content further down the page -->
<main id="main-content">
  <!-- Main content -->
</main>
```

### Alternative Text for Images

Always provide alternative text for images:

```html
<!-- Informative image -->
<img src="chart-sales-2023.png" alt="Sales chart showing 20% growth in Q4 2023">

<!-- Decorative image -->
<img src="decorative-divider.png" alt="">

<!-- Complex image with longer description -->
<figure>
  <img src="complex-diagram.png" alt="Simplified view of database architecture" 
       aria-describedby="diagram-description">
  <figcaption id="diagram-description">
    This diagram illustrates our three-tier database architecture with load balancing,
    primary and replica databases, and connection pooling.
  </figcaption>
</figure>
```

HTML forms the foundation of every web page. Mastering these basics will help you create well-structured, accessible web content that can then be styled with CSS and made interactive with JavaScript. 