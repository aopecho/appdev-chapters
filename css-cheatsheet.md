# CSS Cheatsheet

## Basic Syntax

CSS is made up of **style rules**.

A **style rule** is constructed from a <u>selector</u> and a <u>declaration block</u>. The general structure looks like:

```css
h1 {
  color: maroon;
  font-weight: 700;
}
```

The **selector** identifies which elements you want to style. 

A **declaration block** is surrounded by curly braces (`{}`) and is made up of one or more _declarations_, which are separated by semicolons (`;`).

A **declaration** is comprised of a **property** and a **value** separated by a colon (`:`).

## Selectors

CSS selectors are used to indetify all of the HTML elements that we want our style rule to apply to.

There are bunch of different selectors; for now we're going to dramatically simplify and only think about two: **tag-type** and **class-level**.

## Tag-type

Tag-type selectors target _all_ elements of a particular type, e.g. all `h1` or all `ul`:

```css
h1 {
  color: maroon;
  font-weight: 700;
}

ul {
  list-style-type: none;
  padding-left: none;
}
```

## Class-level

The class selector selects HTML elements with a specific `class` attribute.

To define a class-level selector, write a period (`.`) character, followed by a class name that you come up with, e.g. `.company-name` or `.unstyled-list`.

```css
.company-name {
  color: maroon;
  font-weight: 700;
}

.unstyled-list {
  list-style-type: none;
  padding-left: 0;
}
```

These style rules will be applied to HTML elements with the associated `class=""` attribute.

```html
<h1 class="company-name">Our Awesome Company</h1>

<ul class="unstyled-list">
  <li>Our Work</li>
  <li>About Us</li>
  <li>Give Support</li>
</ul>
```

### Multiple Classes

One HTML element can have multiple values in the `class=""` attribute. Each class is separated by a space. So if possible, it's a good idea to keep your class style rules modular:

```css
.medium-border {
  border-bottom: 5px grey solid;
}

.big-padding {
  padding: 100px;
}
```

Then, you can combine them by applying multiple classes to the _same_ HTML element:

```html
<div class="medium-border big-padding">
  <h1 class="company-name">Our Awesome Company</h1>

  <ul class="unstyled-list">
    <li>Our Work</li>
    <li>About Us</li>
    <li>Give Support</li>
  </ul>
</div>
```

You can still use them individually elsewhere, too, which helps maintain consistency:

```css
<div class="medium-border">
  « Homepage
</div>

<p class="big-padding">
  Our mission is to...
</p>
```

## Where to define style rules

Technically you could write CSS right inside an HTML element using the `style=""` attribute:

```html
<p style="padding: 100px;">Our mission is to...</p>
```

However, it's much more re-usable to define classes for you style rules so you can re-use them on different HTML elements.

Similarly, it's much more re-usable to define your classes in an external file and connect to it in your HTML page.

Steps to connect an external CSS stylesheet:
- Create a file ending in `.css` and add your style rules to it.
- In the desired HTML file, add a `<link>` element to connect it.
- The `<link>` element should have a `rel=""` attribute with a value of `"stylesheet"`.
- The `<link>` element should have an `href=""` attribute with a value that is the location of the CSS file you created.

```html
<link rel="stylesheet" href="/my_styles.css">
```

Now for any page in your website you can link to the external css file instead of re-defined the style rules in every file.

