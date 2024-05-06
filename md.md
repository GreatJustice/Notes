# Markdown

- [Headings](#headings)
- [Links](#links)
- [Lists](#lists)
- [End line](#end-line)
- [Code](#code)
- [Emphasis](#emphasis)
- [Tables](#tables)

## Headings
```
# Heading1
## Heading2
### Heading3
```
[Top](#markdown)

## Links

### Inline
```
[Link Text](url)
```
[Google Inline](https://www.google.com)

### Referenced
Create numbered links to reference later
```
[link number]: url "Title"
[link text][link number]
```
[1]: www.google.com
[Google Referenced][1]

### Link to section
Link to sections of the markdown document
```
[link text](#section-name-all-lowercase-omit-special-chars)
```
[Link to section](#link-to-section)

[Top](#markdown)

## Lists

### Unordered
```
- item1 
- item2
```
- item 1
- item 2

### Unordered (alternate)
```
* item1
* item2
```
* item1
* item2

### Numbered
```
1. Item1
2. Item2
```
1. Item1
2. Item2

### Mixed
1. Item1
   - Sub1
2. Item2

[Top](#markdown)


## End line
```
End line with\
a backslash
```
Line 1\
Line 2

[Top](#markdown)

## Code

### Inline
```
Text `code` text
```
Say hello `cout << "Hello"` and goodbye `cout << "Goodbye"`

### Block

```
~~~
Code block
~~~
```

~~~
```
Code block
```
~~~

[Top](#markdown)

## Emphasis

```
*Italics*
**Bold**
```
Do **you** *really* want to go?

[Top](#markdown)

## Tables

```
| Month    | Savings |
| -------- | ------- |
| January  | $250    |
| February | $80     |
| March    | $420    |
```
| Month    | Savings |
| -------- | ------- |
| January  | $250    |
| February | $80     |
| March    | $420    |

[Top](#markdown)


