# Taming CSS
# Chapter 1: Getting Started

## Declarations and Selectors

CSS syntax is very straightforward.  Let's look at an example:

```css
color: black;
```

This is a **declaration**.  It is made up of two parts: a **property** ("color") and a **value** ("black").  A colon goes between the property and the value, and a semi-colon ends the declaration.  Declarations are used to specify all sorts of styles.  This declaration, for example, sets the text color to black.

There are hundreds of various properties.  They can specify everything from adding drop shadows to setting fonts to determining the position of an element on the page.  Don't let that overwhelm you, though, as you can get very far with just a small number of them, most of which are rather self-explanatory.  We will cover many of the most common ones in the next few chapters.

A declaration by itself isn't very useful.  We've set the text color to black, but what text color are we talking about?  To make it valid, need to specify which part of the document this style is intended for:

```css
body {
  color: black;
}
```

Now we've added a **selector**, "body", and we've enclosed the declaration in braces to associate it with the selector.  This tells the browser to apply our declaration to the `<body>` tag.  Because all visible content is inside the `<body>`, this effectively gives the entire page black text.  We can also add more declarations under the same selector.  Let's specify a white background:

```css
body {
  color: black;
  background: white;
}
```

Simple enough, right?  A selector and any number of declarations: together, these are called a **ruleset**.  Rulesets can be very long, including any number of declarations (though, typically not more than five or ten), and they can be very short, even empty.  `body {}` is a valid ruleset.  Although it has no effect, it can be useful as a placeholder during development when you know you will need to use it soon.

The vast majority of your CSS will be rulesets.  There are only a few other valid structures in CSS, which we will get to later on.

### Linking CSS and HTML

Now that we know how to write a little bit of CSS, let's learn how to apply it to some HTML so we can see it in action.  There are three ways to do this:

First, **inline** styles may be used to apply style declarations directly to an HTML element using the `style` attribute.  For example: `<p style="color: red;">`.  Since this is done explicitly on an element, no selector is needed to target an element.  Typically, inline styles are not preferred, because they do not provide for code reuse.  They are also highly specific, meaning they are difficult to override.

Second, a `<style>` tag can be used to specify styles for the current document.  It should be placed inside of the `<head>`:

```html
<head>
  <style type="text/css">
    body {
      color: black;
      background white;
    }
  </style>
</head>
```

Third, a `<link>` tag can be used to link to an external CSS stylesheet.  This is generally the preferred way to use CSS.  It allows you to keep your CSS in a separate file from your HTML, and it allows you to reuse the same CSS on multiple different pages.  The `<link>` should be inside the document's `<head>`.

Let's make a starter webpage and link it to a stylesheet.  Create a `demo.html` file, and copy the following into it:

```html
<!doctype html>
<head>
  <link href="style.css" rel="stylesheet"/>
</head>
<body>
</body>
```

In the same directory, create a file named `style.css`.  Open `demo.html` in your browser.  Now, you can add CSS to your stylesheet and HTML to your webpage, and refresh your browser window to see the changes.

As you work through this book, add to these files to see the examples in action.  Play around with different values, selectors, and markup to see how it changes things.  You will learn a lot more by seeing things in action, and by seeing immediate feedback as your changes affect things.

## Basic Selectors

So far, we've seen only one selector, `body`.  This is an example of a **type selector** (sometimes called a tag selector).  This selector specifies the HTML tag name it targets.  You can target any HTML element: the selector `p` targets all `<p>` on the page, `h1` targets all `<h1>`, etc.

We know there is only one `<body>` in a valid HTML document, so `body` only targets that one element, but if we were to select another tag, such as the `<p>` tag, it would target all such elements on the page.

Another type of selector is the **class selector**.  Class selectors begin with a `.` followed by the name of the class they target.  For instance, `.navmenu` targets `<ul class="navmenu">`, or any other elements with that class.  The tag name is irrelevant: `.foo` targets `<div class="foo">` as well as `<li class="foo bar">`.  Note that targeted elements may also have additional classes.

**ID selectors** begin with a `#`.  `#sidebar` targets the element with that id, `<div id="sidebar">` for instance.  In HTML, ids must be unique, meaning only one element on the page may have a given id, so this selector targets no more than one element.  If you need to put an identifier on multiple elements, use a class instead.  Also note that an element may only have one id.

Let's try these out in your demo page.  Add this to your stylesheet:

```css
p {
  color: blue;
}

.tinygreen {
  color: green;
  font-size: 10px;
}

#bigred {
  color: red;
  font-size: 20px;
}
```

And add this inside the `<body>` tag of your html:

```html
<p>Lorem ipsum dolor <span class="tinygreen">sit amet</span>.</p>
<p id="bigred">Consectetur adipiscing elit.</p>
```

Save and open up the page in your browser, and you should see the result:

<img src="figure1-1.png"/>

Sometimes you want to apply the same set of styles to multiple elements that cannot be targeted with the same selector.

```css
input {
  color: gray;
  border: blue 1px solid;
}
textarea {
  color: gray;
  border: blue 1px solid;
}
```

This is silly; we're repeating ourself.  Thankfully, there's a better way:

```css
input,
textarea {
  color: gray;
  border: blue 1px solid;
}
```

Any number of selectors can be used in the same ruleset, each separated by a comma.  This saves you from duplicating a lot of code.

### Universal Selector

You can select all elements with the **universal selector** `*`.  At one point, it was a common practice to use this to do a "universal reset".

```css
/* Don't do this */
* {
  margin: 0;
  padding: 0;
}
```

This resets all the browser's default margin and padding on all elements to make development across multiple browsers more uniform.  This is not a recommended practice, however, as it is more heavy-handed than you probably want.  Lists generally need a left padding, paragraphs and headers generally need a top and/or bottom margin, etc.  We will take a look at better ways to do browser resets later on.

You may hear that `*` should be avoided for performance reasons.  In truth, this is a holdover from Internet Explorer 6.  These days, it is generally fine to use in all modern browsers.

## Combining Selectors

You can do a lot with these basic types of selectors, but sometimes you need to be more specific.  Selectors may be combined together to target more precisely.

If you connect two or more basic selectors together, you define a selector that targets elements meeting *all* of the specified criteria:

 * `p.foo` targets `<p class="foo">` but not `<p>` or `<div class="foo">`.
 * `#navmenu.open` targets the `#navmenu`, but only if it also has the class `open` applied.
 * `.widget.active` targets elements that have both the `widget` and `active` classes.

Adding a space between two selectors creates a **descendant selector**:

 ```css
 p .highlight {
   color: red;
 }
 ```

This targets all elements with the `highlight` class that are *descendants* of a `<p>`.  For example, if we applied the CSS above to some HTML:

```html
<p>This paragraph has <span class="highlight">three red words</span></p>
<span class="highlight">But this text is not red.</span>
<p class="highlight">Neither is this.</p>
```

You can string together several selectors like this to do things such as target items in nested lists with `ul li ul li` or anchors in navigation list items with `ul.nav li a`.

## Comments

Use `/*` and `*/` to open and close comments, respectively.  Anything between the open and close is ignored by the browser.  This is useful to leave notes clarifying why some code is important or to temporarily disable a rule for testing.

```css
h1 {
  /* This is a comment.  It does not affect behavior of the CSS. */
  font-size: 18px;
}
```

## A Note on Formatting

The common convention is to indent each line inside braces.  This is for readability; white space does not matter to the browser.  You can put an entire ruleset (or even the entire css file) in one line.  However, I strongly recommend you place each declaration on its own line.  This way, they are easier to parse visually as you maintain the code.  And because modern version control systems work on a line-by-line basis, they will be able to track changes to each declaration separately.

The semi-colon after the final declaration in a block is optional, but I recommend you add it.  Then you won't accidentally introduce invalid syntax should you come back later and add another declaration on the line below it.  It also means you won't have two lines changed in version control history when you only made a meaningful change to one.

I also prefer to split multiple selectors each onto their own line, so it is obvious to the reader.  Otherwise, it can be easy to mistake two distinct selectors (such as `li, .foo`) for one compound one (such as `li .foo` or `li .foo`).
