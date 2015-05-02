# Taming CSS
# Chapter 1: The Basics

## Rulesets

```css
body {
  color: black;
  background: white;
}
```

This is a ruleset.  It consists of a **selector** (`body`) and a declaration block (everything between the braces).  The declaration block contains any number of declarations, which are each in turn contain a **property** (`color` and `background`) and one or more **values** (`black` and `white`).  A colon goes between the property and the value, and a semi-colon goes after each declaration.

The selector determines which elements to target with the declaration block.  Styles defined in the block will be applied to element(s) targeted by the selector.  This selector above is a tag selector; it matches the name of the tag it is selecting, thus this one targets the `<body>` tag.  We know there is only one `<body>` in a valid HTML document, but if we were to select another tag, such as the `<p>` tag, it would target all such tags on the page.  This would like like so:

```css
p {
  color: blue;
}
```

The declarations specify styles to set on the selected elements.  In the example above, we are using the `color` property, which specifies the text color of everything inside the selected element, and we are giving it the value `blue`.  The text of all `<p>` tags will now appear blue.

There are hundreds of various properties you can set.  They can specify everything from adding drop shadows to specifying fonts to setting the position of an element on the page.  Don't let that overwhelm you, though, as you can get very far with just a small number of them, most of which are rather self-explanatory.  We will cover many of them as we go.

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

Any number of selectors can be used in the same rule set, each separated by a comma.  This saves you from duplicating a lot of code.

### Linking CSS to HTML

Before we look at more examples of rulesets, let's learn how to link the CSS to the HTML.  There are three ways to do this: inline, a `<style>` tag, and a `<link>` tag.

With **inline** styles, declarations are applied directly to an HTML element using the `style` attribute.  For example: `<p style="color: red;">`.  Since this is done explicitly on an element, no selector is needed to target an element.  Typically, inline styles are not preferred, because they do not provide for code reuse.  They are also highly specific, meaning they are difficult to override.

A `<style>` tag is used to specify styles for the current document.  It should be placed inside of the `<head>`:

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

A `<link>` tag is used to link to an external CSS stylesheet.  This is generally the preferred way to use CSS.  It allows you to keep your CSS in a separate file from your HTML, and it allows you to reuse the same CSS on multiple different pages.  As with `<style>`, `<link>` should be inside the document's `<head>`.

Let's create a starter webpage and link it to a stylesheet.  Create an `index.html` file, and copy the following into it:

```html
<!doctype html>
<head>
  <link href="style.css" rel="stylesheet"/>
</head>
<body>
</body>
```

In the same directory, create a file named `style.css`.  Open `index.html` in your browser.  Now, you can add CSS to your stylesheet and HTML to your webpage, and refresh your browser window to see the changes.

As you work through these books, add to these files to see the examples in action.  Play around with different values, selectors, and markup to see how it changes things.  You will probably learn a lot more by seeing things in action, and by seeing immediate feedback as your changes affect things.

### Another Example

Let's look at another example of a ruleset:

```css
.button {
  padding: 5px 15px;
  color: white;
  background: #336699;
  border-radius: 5px;
  text-decoration: none;
}
```

This uses a different kind of selector.  The `.` indicates that `button` is a class name rather than a tag name.  This selector targets all elements on the page that have the class `button`.  If we paired it with the HTML

```html
<a class="button" href="next.html">Next &raquo;</a>
```

we would get a button that looked like this:

<img src="figure1.png"/>

Let's look at each of the attributes we set.

```css
padding: 5px 15px;
```
`padding` puts space between the border of the element and its contents.  In this case, we specified `5px`, or five pixels, for the top and bottom padding, and `15px`, or 15 pixels, for the right and left padding.

This is actually a shorthand notation, which `padding` and many other attributes support.  The equivalent full notation would be `5px 15px 5px 15px`.  This sets the four sides in clockwise order: top, right, bottom, left.  If you find that order tough to remember, just think "TRouBLe", which includes the first let of each direction in the correct order.

If the shorthand declaration stops before a side is given a value, that side takes its value from the opposite side: the left side value will match the right side; the bottom side will match the top.  If only one value is specified, that value is applied to all four sides.

Thus the following declarations are equivalent to one another:

```css
padding: 5px 15px;
padding: 5px 15px 5px;
padding: 5px 15px 5px 15px;
```

And the this set are equivalent to one another as well:

```css
padding: 5px;
padding: 5px 5px;
padding: 5px 5px 5px;
padding: 5px 5px 5px 5px;
```

Let's move on to the next declaration:

```css
  color: white;
```

We are already familiar with the `color` attribute.  This sets the color of the text to white.

```css
  background: #336699;
```

We are also familiar with the `background` attribute, but this value is something new.  This is a **hex color**, also known as hex notation.  "Hex" is short for "hexadecimal", which is a base-16 number system.

Unlike our common decimal number system, which is base-10 and uses the ten digits 0 through 9, hexidecimal uses sixteen digits.  We represent these with 0 through 9 as well as A through F.  "A" represents the decimal value "10".  "B" represents "11", et. cetera up through "F" which represents "15".  Capitalization is ignored.

If you were to add one to `F`, you get `10`.  Instead of a tens column, as with decimal numbers, we have a sixteens column.  `11` means 1 sixteen plus 1.  `2A` means 2 sixteens (decimal 32) plus A (decimal 10).  Don't worry too much about the conversion, though.  Suffice it to say, in hex, letters have higher values than numbers; they are kind of like the face cards in a deck of cards.

You can easily get by with a general grasp of the concept.  Then you know that `B9` is larger than `9B`, and `A1` is much larger than `1A`.  Most often, you will be obtaining these values from a image editor or color picker, not writing them by hand off the top of your head.

So how does this get us a color?  A CSS hex color is actually three distinct hexidecimal numbers t
ogether, representing values for red, green, and blue.  In `#336699`, `33` is the amount of red, `66` is the amount of green, and `99` is the amount of blue.  Since blue is the largest value, so this color is primarily blue.  Because both digits in each value are equal, this number can be abbreviated as `#369`.

Colors you will common use include `#ffffff` (or `#fff`), which is pure white, and `#000000` (or `#000`), which is pure black.  (We use additive color, which means the higher the value, the more light we are adding).

Let's look at the next declaration from our button:

```css
  border-radius: 5px;
```

The `border-radius` attribute is used to round the element's corners.  When left undefined, the default value is `0`, which means normal squared corners.  The higher the value, the more gradual the curve, up to half of the element's size.

```css
  text-decoration: none;
```

The `text-decoration` attribute is used to do things like underline or strike through the text.  It supports values like `underline`, `overline`, and `line-through`, though in this case, we've set it to `none`.  We do this because browsers typically underline links by default, but we don't want the underline to appear in our button.  Our `none` value overrides the browser's default value.

So now let's look at our complete ruleset again:

```css
.button {
  padding: 5px 15px;
  color: white;
  background: #336699;
  border-radius: 5px;
  text-decoration: none;
}
```

Now you can understand how these declarations work together to produce our blue button:

<img src="figure1.png"/>

## At-Rules

Most of your CSS will be rulsets, but there is another structure called an **at-rule**.  Some at-rules are terminated with a semi-colon while others are followed by a block inside braces.

One type of at-rule is an **import statement**:

```css
@import url('base.css');
```

This tells the brower to load another stylesheet into the current one.The `@import` must be placed before any rulesets in the stylsheet.  Imports should almost always be avoided, as it forces stylesheets to be loaded consecutively rather than concurrently.  This slows down the page load time in the browser.

An important at-rule is a **media query**:

```css
@media print {
  .navmenu {
    display: none;
  }
}
```

The media query is followed by a block that contains rules.  These rules are applied only when the conditions of the media query are met.  In the example above, elements with the class `navmenu` are hidden when printing the page.  Media queries can also be used to apply styles conditionally based on the dimensions of the viewport window.

## Comments

Use `/*` and `*/` to open and close comments, respectively.  Anything between the the open and close is ignored by the browser.  This is useful to leave notes clarifying why some code is important or to temporarily disable a rule for testing.

```css
h1 {
  /* This is a comment.  It does not affect behavior of the CSS. */
  font-size: 18px;
}
```

## Formatting

The common convention is to indent each line inside braces.  White space does not matter.  You can put an entire ruleset, and even the entire css file in one line.

However, I strongly recommend you place each declaration on its own line.  This way, they are easier to parse visually as you maintain the code.  And because modern version control systems work on a line-by-line basis, they will be able to track changes to each declaration separately.  The semi-colon after the final declaration in a block is optional, but I recommend you add it.  Then you won't accidentally introduce invalid syntax should you come back later and add another declaration on the line below it.  It also means you won't have two lines changed in version control history when you only made a meaningful change to one.
