# HTML Cheatsheet

## Basic Syntax

HTML is made up of **elements**.

```html
<a href="https://www.wikipedia.org">
  Go to Wikipedia
</a>
```

An **element** is constructed from the following:
#### Opening Tag
`<a>` in this example.

#### Closing Tag
`</a>` in this example.

#### Content
`Go to Wikipedia` in this example.

#### Attributes

One or more attributes can be added inside the opening tag after the tag name. `href=""` is an an attribute in the example.

Most attributes only make sense on specific elements.

For example:
- `href=""` is for `<a>` elements, to specify where to take the user when they click the link.
- `src=""` is for `<img>` elements, to specify the url of the image to load.
- `for=""` is for `<label>` elements, to specify which input element it is paired with.

Some attributes can be put on almost any element, like `class=""` or `id=""`.
​Any single `id` should only be used **once** per document. A class can and should be used repeatedly.


### Nesting Elements

Elements can contain other elements.
When that happens, the one inside is known as a **child element** and the one outside is the **parent element**.

If there are multiple levels of nesting, then all the elements inside an outer element are its _descendants_. Similarly, the "outer-most" element is referred to as an inner elements _ancestor_.

## Required Elements

There are required elements that every HTML page must have to be considered **valid**, according to [W3](https://www.w3.org/Consortium/) HTML specifications.

### Doctype Declaration

```html
<!DOCTYPE html>
```

This must be the very first line in the file. It declares what format the data on the page is. Nowadays this is always HTML.

### `<html>` element

```html
<!DOCTYPE html>
<html>
</html>
```

The `<html>` element contains every other HTML element on the page. It contains **exactly** two other elements: `<head>` and `<body>`

### `<head>` element

```html
<!DOCTYPE html>
<html>
  <head>
  </head>
</html>
```

This element contains information and instructions for the browser on how to process the document; things like what title to put in the browser tab, what style sheets to load, what language to use (we will always use a character set called UTF-8, which allows for all languages as well as things like emoji), etc.

### `<body>` element

```html
<!DOCTYPE html>
<html>
  <head></head>
  <body></body>
</html>
```

This element contains all other elements and content that a user can see or interact with.


