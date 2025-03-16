# Markdown Elements in GitBook

This guide shows all Markdown elements you can use in GitBook.com, with examples and code to implement them.

## Basic Text Formatting

### Bold

**This text is bold**

```
**This text is bold**
```

### Italic

_This text is italic_

```
_This text is italic_
```

### Strikethrough

~~This text is strikethrough~~

```
~~This text is strikethrough~~
```

### Inline Code

`inline code`

```
`inline code`
```

## Headers

# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6

```
# Heading 1
## Heading 2
### Heading 3
#### Heading 4
##### Heading 5
###### Heading 6
```

## Links

### External Link

[Link to Google](https://www.google.com)

```
[Link to Google](https://www.google.com)
```

### Relative Link

[Link to README](README.md)

```
[Link to README](README.md)
```

### Email Link

[Send email](mailto:example@email.com)

```
[Send email](mailto:example@email.com)
```

### Anchor Link (Internal Page Link)

[Go to Tables section](#tables)

```
[Go to Tables section](#tables)
```

First, create a heading (which automatically creates an anchor), then link to it with `#heading-name` (lowercase with hyphens instead of spaces).

### Page Link Block

Page link blocks help create relations between pages in your space:

{% content-ref url="README.md" %}
[README.md](README.md)
{% endcontent-ref %}

```
{% content-ref url="README.md" %}
[README.md](README.md)
{% endcontent-ref %}
```

## Lists

### Unordered List

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

- Item A
- Item B

```
- Item A
- Item B
```

### Ordered List

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

### Task List

- [ ] Pending task
- [x] Completed task

```
- [ ] Pending task
- [x] Completed task
```

## Quotes

> This is a quote
> 
> It can include multiple paragraphs

```
> This is a quote
> 
> It can include multiple paragraphs
```

## Code Blocks

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

## Extended Code Examples

### GitHub Workflow Example

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

### HTML Example

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

### CSS Example

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

### JavaScript Example

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

### TypeScript Example

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

### Angular Example

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

### NestJS Example

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

### Docker Example

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

## Images

![Alt text](https://via.placeholder.com/150)

```
![Alt text](https://via.placeholder.com/150)
```

## Tables

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

## Horizontal Line

---

```
---
```

## Hints (Notes)

{% hint style="info" %}
This is an information note.
{% endhint %}

```
{% hint style="info" %}
This is an information note.
{% endhint %}
```

{% hint style="warning" %}
This is a warning.
{% endhint %}

```
{% hint style="warning" %}
This is a warning.
{% endhint %}
```

{% hint style="danger" %}
This is a danger alert.
{% endhint %}

```
{% hint style="danger" %}
This is a danger alert.
{% endhint %}
```

{% hint style="success" %}
This is a success note.
{% endhint %}

```
{% hint style="success" %}
This is a success note.
{% endhint %}
```

## Tabs

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

## Expandable Content

{% expand title="Click to expand" %}
This content is hidden until the title is clicked.
{% endexpand %}

```
{% expand title="Click to expand" %}
This content is hidden until the title is clicked.
{% endexpand %}
```

Alternative syntax:

<details>

<summary>Click to expand (alternative method)</summary>

This is another way to create expandable content using standard Markdown syntax.

</details>

```
<details>

<summary>Click to expand (alternative method)</summary>

This is another way to create expandable content using standard Markdown syntax.

</details>
```

## Files

{% file src="README.md" caption="README File" %}

```
{% file src="README.md" caption="README File" %}
```

## Cards

{% card %}
This is a simple card.
{% endcard %}

```
{% card %}
This is a simple card.
{% endcard %}
```

## Embedded URLs

{% embed url="https://www.youtube.com/watch?v=dQw4w9WgXcQ" %}

```
{% embed url="https://www.youtube.com/watch?v=dQw4w9WgXcQ" %}
```

## Math Formulas

Inline: $E=mc^2$

```
$E=mc^2$
```

Block:

$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$

```
$$
\frac{n!}{k!(n-k)!} = \binom{n}{k}
$$
```

## Annotations (Footnotes)

Here's a footnote reference[^1].

[^1]: This is the footnote content.

```
Here's a footnote reference[^1].

[^1]: This is the footnote content.
```

## Stepper

{% stepper %}
{% step title="Step 1" %}
Content for step 1
{% endstep %}

{% step title="Step 2" %}
Content for step 2
{% endstep %}

{% step title="Step 3" %}
Content for step 3
{% endstep %}
{% endstepper %}

```
{% stepper %}
{% step title="Step 1" %}
Content for step 1
{% endstep %}

{% step title="Step 2" %}
Content for step 2
{% endstep %}

{% step title="Step 3" %}
Content for step 3
{% endstep %}
{% endstepper %}
```

## Emojis

You can insert emojis using the `:emoji_name:` syntax, for example: :smile: :heart: :rocket:

```
:smile: :heart: :rocket:
```

## Drawings

GitBook supports drawing diagrams, which can be added using the diagram block:

{% code title="Simple diagram example" %}
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
{% endcode %}

```
{% code title="Simple diagram example" %}
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
{% endcode %}
```

## Keyboard Keys

<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd>

```
<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd>
```

## Highlighting Text

==This text is highlighted==

```
==This text is highlighted==
```

## Definition Lists

<dl>
  <dt>Term</dt>
  <dd>Definition of the term</dd>
  <dt>Another term</dt>
  <dd>Definition of another term</dd>
</dl>

```
<dl>
  <dt>Term</dt>
  <dd>Definition of the term</dd>
  <dt>Another term</dt>
  <dd>Definition of another term</dd>
</dl>
```

## Superscript and Subscript

Superscript: X<sup>2</sup>
Subscript: H<sub>2</sub>O

```
Superscript: X<sup>2</sup>
Subscript: H<sub>2</sub>O
``` 