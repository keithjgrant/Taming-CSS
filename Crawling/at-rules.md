<!--- where to put this? -->

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
