# Forms, Query Strings, and Params

We use forms to collect information from users. The information the user enters is sent to our app for processing.

## Bare bones HTML form

```html
<form>
</form>
```

### form attributes

A form must have an `action` attribute. The value of this attribute is the route/URL that the user should visit when the form is submitted.

```html
<form action="/process_form">
</form>
```

A form must also have a `method` attribute

Forms generally capture user input using `<input>` elements. Each `<input>` must be a child element of the form.

```html
<form>
  <input>
</form>
```

A `<button>` element is required to submit the form.

```html
<form>
  <input>
  <button>Submit</button>
</form>
```


### input attributes

At a minimun, each `<input>` needs a `name` attribute and a `value` attribute.

The `name` can be anything, but it should be unique within the form.

```html
<!-- GOOD -->
<form>
  <input name="email">
  <input name="password">
  <button>Submit</button>
</form>
```

```html
<!-- BAD -->
<form>
  <input name="email">
  <input name="email">
  <button>Submit</button>
</form>
```

The `value` can be determined in different ways, but is most commonly it is whatever the user has been typed into the field.

By default the `value` attribute is empty, but you can set an initial value— which can be handle for edit forms.

```html
<form>
  <input name="email" value="test@example.com">
  <button>Submit</button>
</form>
```

which renders in the browser like this:
<div>
  <input name="email" value="test@example.com">
</div>

## Form submission

How does our app know what the user typed in the form?

### Query string

The query string is an optional part of a URL that begins with a `?`. The query string contains information a user has entered.

<span style="font-size: 1.2rem;font-family: monospace;">http://www.example.com/<span style="color: red; text-decoration: underline;">?fruit=apple&color=green</span></span>

A query string functions similar to a Ruby Hash— it's a list of key-value pairs. In the example, `fruit` and `color` are keys, and `apple` and `green` are values. 

Forms _automatcially_ create a query string when they are submitted. The keys in the resulting query string come from the `name` attribute of the `<input>` and the `value` is whatever the user typed into that field. The URL before the query string comes from the `action` attribute of the `<form>`

```html
<form action="http://www.example.com/">
  <input name="fruit" value="apple">
  <input name="color" value="green">
  <button>Submit</button>
</form>
```
  
<span style="font-size: 1.2rem;font-family: monospace;">http://www.example.com/<span style="color: red; text-decoration: underline;">?fruit=apple&color=green</span></span>




When a form is submitted, the name-value pairs from all the fields inside the `<form>` element are included in an HTTP. The request is made to a URL defined in the form’s `action` attribute, and the type of request (`GET` or `POST`) is defined in the form’s `method` attribute. This means that all the user-provided data is sent to the server all at once when the form is submitted, and the server can do whatever it wants with that data.

## Demo

```html
<form action="/process" method="get">
  <input name="fruit">
  <button>Submit</button>
</form>
```

<form action="/process" method="get" id="demo">
  <input name="fruit">
  <button>Submit</button>
</form>

<div id="result"></div>

<script>
  var form = document.getElementById('demo')
  var input = document.getElementsByTagName('input')[0]
  form.addEventListener('submit', function(event) {
    event.preventDefault();
    var action = this.attributes[0].value;
    var name = input.name;
    var value = input.value;
    var query_string = `?${name}=${value}`;
    var full_url = `${action}${query_string}`;
    var rails_params = `{ "${name}" => "${value}" }`;
    var content = `This form submitted to: <code>${full_url}</code><br>The query string is <code>${query_string}</code>.<br>The <code>params</code> Hash looks like this <code>${rails_params}</code>.`
    console.log(full_url);
    console.log(rails_params);
    updateResult(content);
  });

  function updateResult(content) {
    var result = document.getElementById("result");
    result.innerHTML = content;
  }
</script>
---
`<input>` are one of the few HTML elements that don't require a closing tag.


## How forms work

## params
## Valid forms

## Visiting a URL
An HTML form is used to collect user input. The user input is most often sent to a server for processing.

When you load a page, you are making an HTTP request (a `GET` request, usually). This request is sent by your browser to the server, and the server responds with (usually) the web page you are looking for. This interaction is one of the most fundamental concepts of the internet. And it is how HTML forms are designed to work.

Each input element inside a `<form>` element has a `name` attribute, and also a `value`. The `value` is determined in different ways. For text-based inputs, the value is whatever has been typed into the field. The user can adjust the `value`, but not the `name`. This creates a set of name-value pairs in which the values are determined by user input.

When a form is submitted, the name-value pairs from all the fields inside the `<form>` element are included in an HTTP. The request is made to a URL defined in the form’s `action` attribute, and the type of request (`GET` or `POST`) is defined in the form’s `method` attribute. This means that all the user-provided data is sent to the server all at once when the form is submitted, and the server can do whatever it wants with that data.

When the server receives the form submission, it is like any other HTTP request. It does whatever it needs to do with the included data and issues a response back to the browser. Remember how a page load is a response? Same thing here. In a typical form submission, the response is a new page which the browser loads.

The vast majority of online forms work this way.

## Why We Use Forms
to get input from users
## How forms 



