---
title: Markdown
category: Markup
layout:
languages: [markdown]
---

### References

This Markdown cheat sheet provides a quick overview of all the Markdown syntax
elements. It can’t cover every edge case, so if you need more information
about any of these elements, refer to the reference guides for
[basic syntax](https://www.markdownguide.org/basic-syntax)
and
[extended syntax](https://www.markdownguide.org/extended-syntax).

### Headers

```markdown
# h1
## h2
### h3
#### h4
##### h5
###### h6
```

```markdown
Header 1
========
```

```markdown
Header 2
--------
```

### Emphasis

```markdown
*italic*
_italic_
```

```markdown
**bold**
__bold__
```

```markdown
***bold italic***
___bold italic___
```

```markdown
~~strikethrough~~
```

```markdown
`code`
```

### Lists

```markdown
* Item 1
* Item 2
```

```markdown
- Item 1
- Item 2
```

```markdown
- [ ] Checkbox off
- [x] Checkbox on
```

```markdown
1. Item 1
2. Item 2
```

### Definition List

term
: definition

### Strikethrough

~~The world is flat.~~

### Links

```markdown
[title](https://archlinux.org)
```

```markdown
[title][archlinux]
[archlinux]: https://archlinux.org
```

```markdown
<https://archlinux.org>
```

### Images

```markdown
![Image alt text](/path/to/img.jpg)
![Image alt text](/path/to/img.jpg "title")
![Image alt text][img]
```

```markdown
[img]: http://foo.com/img.jpg
```

### Code

```
    4 space indent
    makes a code block
```

```
code fences
```

```js
codeFences.withLanguage()
```

### Blockquotes

```markdown
> This is
> a blockquote
>
> > Nested
> > Blockquote
```

### Horizontal line

```markdown
----
```

```markdown
****
```

### Tables

```markdown
| Column 1 Heading | Column 2 Heading |
| ---------------- | ---------------- |
| Some content     | Other content    |
```

```markdown
Column 1 Heading | Column 2 Heading
--- | ---
Some content | Other content
```

### Footnote

Here's a sentence with a footnote. [^1]

[^1]: This is the footnote.

### Heading ID

### My Great Heading {#custom-id}

---

THE END
