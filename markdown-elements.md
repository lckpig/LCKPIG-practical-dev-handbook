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