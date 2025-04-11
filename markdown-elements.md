# Markdown Elements Guide

## Markdown Elements in GitBook

This guide shows all the Markdown elements you can use in GitBook.com, with examples and code to implement them.

### Basic Text Formatting

#### Bold

**This text is bold**

```
**This text is bold**
```

#### Italic

_This text is italic_

```
_This text is italic_
```

#### Strikethrough

~~This text is strikethrough~~

```
~~This text is strikethrough~~
```

#### Inline Code

`inline code`

```
`inline code`
```

### Headers

## Header 1

### Header 2

#### Header 3

**Header 4**

**Header 5**

**Header 6**

```
# Header 1
## Header 2
### Header 3
#### Header 4
##### Header 5
###### Header 6
```

### Links

#### External Link

[Link to Google](https://www.google.com)

```
[Link to Google](https://www.google.com)
```

#### Relative Link

[Link to README](README.md)

```
[Link to README](README.md)
```

#### Email Link

[Send email](mailto:example@email.com)

```
[Send email](mailto:example@email.com)
```

#### Anchor Link (Internal Page Link)

[Go to Tables section](markdown-elements.md#tables)

```
[Go to Tables section](#tables)
```

First, create a heading (which automatically creates an anchor), then link to it with `#heading-name` (lowercase with hyphens instead of spaces).

#### Page Link Block

Page link block helps create relationships between pages in your space:

{% content-ref url="README.md" %}
[README.md](README.md)
{% endcontent-ref %}

```

<div data-gb-custom-block data-tag="content-ref" data-url='README.md'>

[README.md](README.md)

</div>

```

### Lists

#### Unordered List

* Item 1
* Item 2
  * Subitem 2.1
  * Subitem 2.2
* Item 3

```
* Item 1
* Item 2
  * Subitem 2.1
  * Subitem 2.2
* Item 3
```

You can also use `-` instead of `*`:

* Item A
* Item B

```
- Item A
- Item B
```

#### Ordered List

1. First item
2. Second item
   1. Subitem 2.1
   2. Subitem 2.2
3. Third item

```
1. First item
2. Second item
   1. Subitem 2.1
   2. Subitem 2.2
3. Third item
```

#### Task List

* [ ] Pending task
* [x] Completed task

```
- [ ] Pending task
- [x] Completed task
```

### Quotes

> This is a quote
>
> It can include multiple paragraphs

```
> This is a quote
> 
> It can include multiple paragraphs
```

### Code Blocks

Code block without syntax highlighting:

```
function example() {
  return "Hello world";
}
```

````
```
function example() {
  return "Hello world";
}
```
````

Code block with JavaScript syntax highlighting:

```javascript
function example() {
  return "Hello world";
}
```

````
```javascript
function example() {
  return "Hello world";
}
```
````

### Extended Code Examples

#### GitHub Workflow Example

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '16'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Run tests
      run: npm test
      
    - name: Build project
      run: npm run build
```

#### HTML Example

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Website</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <nav>
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </nav>
    </header>
    
    <main>
        <section id="home">
            <h1>Welcome to My Website</h1>
            <p>This is a sample HTML page.</p>
        </section>
    </main>
    
    <footer>
        <p>&copy; 2023 My Website. All rights reserved.</p>
    </footer>
    
    <script src="script.js"></script>
</body>
</html>
```

#### CSS Example

```css
/* Modern CSS with variables and responsive design */
:root {
    --primary-color: #3498db;
    --secondary-color: #2ecc71;
    --text-color: #333;
    --background-color: #f8f9fa;
    --spacing-unit: 1rem;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: var(--text-color);
    background-color: var(--background-color);
    line-height: 1.6;
    margin: 0;
    padding: 0;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: var(--spacing-unit);
}

/* Responsive navigation */
nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spacing-unit);
    background-color: var(--primary-color);
    color: white;
}

/* Media query for mobile devices */
@media (max-width: 768px) {
    nav {
        flex-direction: column;
    }
    
    .nav-links {
        margin-top: var(--spacing-unit);
    }
}
```

#### JavaScript Example

```javascript
// Modern JavaScript with ES6+ features
class TaskManager {
    constructor(username) {
        this.username = username;
        this.tasks = [];
    }
    
    addTask(title, priority = 'medium') {
        const newTask = {
            id: Date.now(),
            title,
            priority,
            completed: false,
            createdAt: new Date()
        };
        
        this.tasks.push(newTask);
        return newTask;
    }
    
    completeTask(taskId) {
        const task = this.tasks.find(task => task.id === taskId);
        if (task) {
            task.completed = true;
            return true;
        }
        return false;
    }
    
    getPendingTasks() {
        return this.tasks.filter(task => !task.completed);
    }
    
    getTasksByPriority(priority) {
        return this.tasks.filter(task => task.priority === priority);
    }
}

// Using async/await with fetch API
async function fetchUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error fetching user data:', error);
        return null;
    }
}
```

#### TypeScript Example

```typescript
// TypeScript interface and class implementation
interface User {
    id: number;
    name: string;
    email: string;
    role: 'admin' | 'user' | 'guest';
    createdAt: Date;
}

interface AuthService {
    login(email: string, password: string): Promise<User | null>;
    logout(): void;
    getCurrentUser(): User | null;
}

class UserAuthService implements AuthService {
    private currentUser: User | null = null;
    
    constructor(private apiBaseUrl: string) {}
    
    async login(email: string, password: string): Promise<User | null> {
        try {
            const response = await fetch(`${this.apiBaseUrl}/login`, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({ email, password })
            });
            
            if (!response.ok) {
                throw new Error('Authentication failed');
            }
            
            const userData: User = await response.json();
            this.currentUser = userData;
            return userData;
        } catch (error) {
            console.error('Login error:', error);
            return null;
        }
    }
    
    logout(): void {
        this.currentUser = null;
        // Additional logout logic (clear tokens, etc.)
    }
    
    getCurrentUser(): User | null {
        return this.currentUser;
    }
}

// Generic function example
function createDataStore<T>(initialData: T[]): {
    getAll: () => T[];
    getById: (id: number) => T | undefined;
    add: (item: T) => void;
} {
    const data: T[] = [...initialData];
    
    return {
        getAll: () => [...data],
        getById: (id: number) => data.find((item: any) => item.id === id),
        add: (item: T) => data.push(item)
    };
}
```

#### Angular Example

```typescript
// Angular component example
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { UserService } from '../../services/user.service';
import { User } from '../../models/user.model';

@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.scss']
})
export class UserProfileComponent implements OnInit {
  @Input() userId: number;
  @Output() userUpdated = new EventEmitter<User>();
  
  profileForm: FormGroup;
  user: User | null = null;
  isLoading = false;
  errorMessage = '';
  
  constructor(
    private fb: FormBuilder,
    private userService: UserService
  ) {
    this.profileForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      bio: ['', Validators.maxLength(500)]
    });
  }
  
  ngOnInit(): void {
    this.loadUserProfile();
  }
  
  async loadUserProfile(): Promise<void> {
    this.isLoading = true;
    
    try {
      this.user = await this.userService.getUserById(this.userId);
      if (this.user) {
        this.profileForm.patchValue({
          name: this.user.name,
          email: this.user.email,
          bio: this.user.bio
        });
      }
    } catch (error) {
      this.errorMessage = 'Failed to load user profile';
    } finally {
      this.isLoading = false;
    }
  }
  
  onSubmit(): void {
    if (this.profileForm.valid && this.user) {
      const updatedUser = {
        ...this.user,
        ...this.profileForm.value
      };
      
      this.userService.updateUser(updatedUser)
        .subscribe(
          (result) => {
            this.userUpdated.emit(result);
          },
          (error) => {
            this.errorMessage = 'Failed to update profile';
          }
        );
    }
  }
}
```

#### NestJS Example

```typescript
// NestJS controller and service example
import { Controller, Get, Post, Body, Param, UseGuards, HttpException, HttpStatus } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { ProductsService } from './products.service';
import { CreateProductDto } from './dto/create-product.dto';
import { Product } from './interfaces/product.interface';
import { RolesGuard } from '../auth/guards/roles.guard';
import { Roles } from '../auth/decorators/roles.decorator';

@Controller('products')
export class ProductsController {
  constructor(private readonly productsService: ProductsService) {}

  @Get()
  async findAll(): Promise<Product[]> {
    return this.productsService.findAll();
  }

  @Get(':id')
  async findOne(@Param('id') id: string): Promise<Product> {
    const product = await this.productsService.findOne(id);
    
    if (!product) {
      throw new HttpException('Product not found', HttpStatus.NOT_FOUND);
    }
    
    return product;
  }

  @Post()
  @UseGuards(AuthGuard('jwt'), RolesGuard)
  @Roles('admin')
  async create(@Body() createProductDto: CreateProductDto): Promise<Product> {
    return this.productsService.create(createProductDto);
  }
}

// Service implementation
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { Product } from './interfaces/product.interface';
import { CreateProductDto } from './dto/create-product.dto';

@Injectable()
export class ProductsService {
  constructor(
    @InjectModel('Product') private readonly productModel: Model<Product>
  ) {}

  async findAll(): Promise<Product[]> {
    return this.productModel.find().exec();
  }

  async findOne(id: string): Promise<Product> {
    return this.productModel.findById(id).exec();
  }

  async create(createProductDto: CreateProductDto): Promise<Product> {
    const newProduct = new this.productModel(createProductDto);
    return newProduct.save();
  }
}
```

#### Docker Example

```dockerfile
# Multi-stage build example for a Node.js application
FROM node:16-alpine AS builder

WORKDIR /app

# Copy package files and install dependencies
COPY package*.json ./
RUN npm ci

# Copy source code and build the application
COPY . .
RUN npm run build

# Production stage
FROM node:16-alpine AS production

WORKDIR /app

# Copy only necessary files from builder stage
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./

# Set environment variables
ENV NODE_ENV=production
ENV PORT=3000

# Expose port and define healthcheck
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s --retries=3 \
  CMD node -e "fetch('http://localhost:3000/health').then(r => process.exit(r.ok ? 0 : 1))" || exit 1

# Use non-root user for security
USER node

# Start the application
CMD ["node", "dist/main.js"]
```

### Images

![Alt text](https://dummyimage.com/150x150/3498db/ffffff\&text=Image)

```
![Alt text](https://dummyimage.com/150x150/3498db/ffffff&text=Image)
```

#### Images in Different Formats

**JPG Image**

![JPG Example](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Image_created_with_a_mobile_phone.png/800px-Image_created_with_a_mobile_phone.png)

```
![JPG Example](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b6/Image_created_with_a_mobile_phone.png/800px-Image_created_with_a_mobile_phone.png)
```

**PNG Image (with transparency)**

![PNG Example](https://www.pngall.com/wp-content/uploads/5/Python-PNG-File.png)

```
![PNG Example](https://www.pngall.com/wp-content/uploads/5/Python-PNG-File.png)
```

**SVG Image**

![SVG Example](https://upload.wikimedia.org/wikipedia/commons/thumb/6/61/HTML5_logo_and_wordmark.svg/512px-HTML5_logo_and_wordmark.svg.png)

```
![SVG Example](https://upload.wikimedia.org/wikipedia/commons/thumb/6/61/HTML5_logo_and_wordmark.svg/512px-HTML5_logo_and_wordmark.svg.png)
```

**GIF Image (animated)**

![GIF Example](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExcmUwZWFwcGd3YW0yMzg0aGZiNnBmaWRnZHFidXliNXY5N2RhYmNrdiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/du3J3cXyzhj75IOgvA/giphy.gif)

```
![GIF Example](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExcmUwZWFwcGd3YW0yMzg0aGZiNnBmaWRnZHFidXliNXY5N2RhYmNrdiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/du3J3cXyzhj75IOgvA/giphy.gif)
```

**WebP Image**

![WebP Example](https://www.gstatic.com/webp/gallery/4.webp)

```
![WebP Example](https://www.gstatic.com/webp/gallery/4.webp)
```

#### Adjusting Image Size

Images can be resized using HTML:

![Resized Image](https://dummyimage.com/150x150/3498db/ffffff\&text=Image)

```
<img src="https://dummyimage.com/150x150/3498db/ffffff&text=Image" width="300" height="200" alt="Resized Image">
```

#### Images with Links

[![Image with Link](https://dummyimage.com/150x150/3498db/ffffff\&text=Image)](https://www.gitbook.com)

```
[![Image with Link](https://dummyimage.com/150x150/3498db/ffffff&text=Image)](https://www.gitbook.com)
```

### Tables

| Header 1 | Header 2 | Header 3 |
| -------- | -------- | -------- |
| Cell 1,1 | Cell 1,2 | Cell 1,3 |
| Cell 2,1 | Cell 2,2 | Cell 2,3 |

```
| Header 1 | Header 2 | Header 3 |
| -------- | -------- | -------- |
| Cell 1,1 | Cell 1,2 | Cell 1,3 |
| Cell 2,1 | Cell 2,2 | Cell 2,3 |
```

### Horizontal Line

***

```
---
```

### Hints (Notes)

{% hint style="info" %}
This is an information note.
{% endhint %}

```

<div data-gb-custom-block data-tag="hint" data-style='info'>

This is an information note.

</div>

```

{% hint style="warning" %}
This is a warning.
{% endhint %}

```

<div data-gb-custom-block data-tag="hint" data-style='warning'>

This is a warning.

</div>

```

{% hint style="danger" %}
This is a danger alert.
{% endhint %}

```

<div data-gb-custom-block data-tag="hint" data-style='danger'>

This is a danger alert.

</div>

```

{% hint style="success" %}
This is a success note.
{% endhint %}

```

<div data-gb-custom-block data-tag="hint" data-style='success'>

This is a success note.

</div>

```

{% hint style="info" %}

## This is a H2 heading

This is a line

This is an inline <img src=".gitbook/assets/notification.png" alt="" data-size="line"> image

- This is a second <mark style="color:orange;background-color:purple;">line using an unordered list and color</mark>
{% endhint %}


### Tabs

{% tabs %}
{% tab title="First Tab" %}
Content of the first tab
{% endtab %}

{% tab title="Second Tab" %}
Content of the second tab
{% endtab %}

{% tab title="Third Tab" %}
Content of the third tab
{% endtab %}
{% endtabs %}

```

<div data-gb-custom-block data-tag="tabs">

<div data-gb-custom-block data-tag="tab" data-title='First Tab'>

Content of the first tab

</div>

<div data-gb-custom-block data-tag="tab" data-title='Second Tab'>

Content of the second tab

</div>

<div data-gb-custom-block data-tag="tab" data-title='Third Tab'>

Content of the third tab

</div>

</div>

```

{% tabs %}
{% tab title="CSS" %}
```css
/* Modern CSS with variables and responsive design */
:root {
    --primary-color: #3498db;
    --secondary-color: #2ecc71;
    --text-color: #333;
    --background-color: #f8f9fa;
    --spacing-unit: 1rem;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: var(--text-color);
    background-color: var(--background-color);
    line-height: 1.6;
    margin: 0;
    padding: 0;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: var(--spacing-unit);
}

/* Responsive navigation */
nav {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: var(--spacing-unit);
    background-color: var(--primary-color);
    color: white;
}

/* Media query for mobile devices */
@media (max-width: 768px) {
    nav {
        flex-direction: column;
    }
    
    .nav-links {
        margin-top: var(--spacing-unit);
    }
}
```
{% endtab %}

{% tab title="JS" %}
```javascript
// Modern JavaScript with ES6+ features
class TaskManager {
    constructor(username) {
        this.username = username;
        this.tasks = [];
    }
    
    addTask(title, priority = 'medium') {
        const newTask = {
            id: Date.now(),
            title,
            priority,
            completed: false,
            createdAt: new Date()
        };
        
        this.tasks.push(newTask);
        return newTask;
    }
    
    completeTask(taskId) {
        const task = this.tasks.find(task => task.id === taskId);
        if (task) {
            task.completed = true;
            return true;
        }
        return false;
    }
    
    getPendingTasks() {
        return this.tasks.filter(task => !task.completed);
    }
    
    getTasksByPriority(priority) {
        return this.tasks.filter(task => task.priority === priority);
    }
}

// Using async/await with fetch API
async function fetchUserData(userId) {
    try {
        const response = await fetch(`https://api.example.com/users/${userId}`);
        
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error fetching user data:', error);
        return null;
    }
}
```
{% endtab %}

{% tab title="TS" %}
```typescript
// Angular component example
import { Component, OnInit, Input, Output, EventEmitter } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { UserService } from '../../services/user.service';
import { User } from '../../models/user.model';

@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.scss']
})
export class UserProfileComponent implements OnInit {
  @Input() userId: number;
  @Output() userUpdated = new EventEmitter<User>();
  
  profileForm: FormGroup;
  user: User | null = null;
  isLoading = false;
  errorMessage = '';
  
  constructor(
    private fb: FormBuilder,
    private userService: UserService
  ) {
    this.profileForm = this.fb.group({
      name: ['', [Validators.required, Validators.minLength(3)]],
      email: ['', [Validators.required, Validators.email]],
      bio: ['', Validators.maxLength(500)]
    });
  }
  
  ngOnInit(): void {
    this.loadUserProfile();
  }
  
  async loadUserProfile(): Promise<void> {
    this.isLoading = true;
    
    try {
      this.user = await this.userService.getUserById(this.userId);
      if (this.user) {
        this.profileForm.patchValue({
          name: this.user.name,
          email: this.user.email,
          bio: this.user.bio
        });
      }
    } catch (error) {
      this.errorMessage = 'Failed to load user profile';
    } finally {
      this.isLoading = false;
    }
  }
  
  onSubmit(): void {
    if (this.profileForm.valid && this.user) {
      const updatedUser = {
        ...this.user,
        ...this.profileForm.value
      };
      
      this.userService.updateUser(updatedUser)
        .subscribe(
          (result) => {
            this.userUpdated.emit(result);
          },
          (error) => {
            this.errorMessage = 'Failed to update profile';
          }
        );
    }
  }
}
```
{% endtab %}
{% endtabs %}

### Content Expandable

```

<div data-gb-custom-block data-tag="expand" data-title='Click to expand'>

This content is hidden until the title is clicked.

</div>

```

Alternative syntax:

<details>

<summary>Click to expand (alternative method)</summary>

This is another way to create expandable content using standard Markdown syntax.

</details> 