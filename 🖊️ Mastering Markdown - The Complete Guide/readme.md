## What is Markdown?
 
Markdown is a way to style text on the web. You control the display of the document; formatting words as bold or italic, adding images, and creating lists are just a few of the things we can do with Markdown.

## Table of Contents

* [Headings](#headings)

* [Line Breaks](#line-breaks)

* [Emphasis](#emphasis)

* [BlockQuotes](#blockquotes)

* [Lists](#lists)

* [Code Blocks](#code-blocks)

* [Footnotes](#footnotes)

* [Heading IDs](#heading-ids)

* [Definition Lists](#definition-lists)

* [Task Lists](#task-lists)

* [Images](#images)

* [Tables](#tables)


## Headings

```md
# Heading 1 
## Heading 2 
### Heading 3 
#### Heading 4 
##### Heading 5 
###### Heading 6
```

Output-
# Heading 1 
## Heading 2 
### Heading 3 
#### Heading 4 
##### Heading 5 
###### Heading 6

---

## Line Breaks

```
Line 1
<br>
Line 2
```
Output -

Line 1
<br>
Line 2


---

## Emphasis
### Bold
```md
I love **bold text**.
I love __bold text__.
Love**is**bold
```
Output
I love **bold text**.
I love __bold text__.
Love**is**bold

---

### Italic
```md
this is *italics text*
this is _italics text_
thisis*italics*text
```
Output-
this is *italics text*
this is _italics text_
thisis*italics*text

---

### Strikethrough
```md
~~The world is flat.~~ We now know that the world is round.
```
Output -

~~The world is flat.~~ We now know that the world is round.

---

## Blockquotes

### Single Line

```md
> This is a Single line Blockquote
```
Output-
> This is a Single line Blockquote

---

### Blockquotes with Multiple Paragraphs
```md
> This the first paragraph.
>
> And this is the second paragraph.
```
Output-
> This the first paragraph.
>
> And this is the second paragraph.

---

### Nested Blockquotes

```md
> This the first paragraph.
>
>> And this is the nested paragraph.
```
Output-
> This the first paragraph.
>
>> And this is the nested paragraph.

---

## Lists

### Ordered Lists

```md
1. First item
2. Second item
3. Third item
4. Fourth item

1. First item
1. Second item
1. Third item
1. Fourth item

1. First item
8. Second item
3. Third item
5. Fourth item
```
Output-
1. First item
2. Second item
3. Third item
4. Fourth item
<br>
1. First item
1. Second item
1. Third item
1. Fourth item
<br>
1. First item
8. Second item
3. Third item
5. Fourth item

---

### Nesting List Items

```md
1. First item
2. Second item
3. Third item
    1. Indented item
    2. Indented item
4. Fourth item
```

Output-
1. First item
2. Second item
3. Third item
    1. Indented item
    2. Indented item
4. Fourth item

### Unordered Lists


```md
- First item
- Second item
- Third item
- Fourth item

* First item
* Second item
* Third item
* Fourth item

+ First item
* Second item
- Third item
+ Fourth it
```
Output-
- First item
- Second item
- Third item
- Fourth item
<br>
* First item
* Second item
* Third item
* Fourth item
<br>
+ First item
* Second item
- Third item
+ Fourth item

---

## Code Blocks

 Enclose any code in (```) signs
```md
eg-
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}

```
---

## Footnotes

```md
Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.

```
Output-
Here's a simple footnote,[^1] and here's a longer one.[^bignote]

[^1]: This is the first footnote.

[^bignote]: Here's one with multiple paragraphs and code.

    Indent paragraphs to include them in the footnote.

    `{ my code }`

    Add as many paragraphs as you like.

## Heading IDs


```md
### My Great Heading {#custom-id}

```

### Linking to Heading IDs



```md
[Heading IDs]
(#heading-ids)

```
---

## Definition Lists

```md
First Term
: This is the definition of the first term.

Second Term
: This is one definition of the second term.
: This is another definition of the second term.

```
Output-

First Term
: This is the definition of the first term.

Second Term
: This is one definition of the second term.
: This is another definition of the second term.

---

## Task Lists

```md
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media
```
Output-
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

---

## Images

```md
![Markdown Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/1280px-Markdown-mark.svg.png)
```
Output-

![Markdown Logo](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/1280px-Markdown-mark.svg.png)


---

## Tables

```md
| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

```
Output-

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

---
