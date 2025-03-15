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

HTML forms the foundation of every web page. Mastering these basics will help you create well-structured, accessible web content that can then be styled with CSS and made interactive with JavaScript. 