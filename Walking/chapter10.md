# Taming CSS
# Chapter 10: More Selectors



## Combinators

We've already seen one type of combinator, the **descendant combinator**.  There are some other types of combinators, too:

A `>` is a **child combinator**.  This is used to target elements that are a *direct descendant* of another (`.parent > .child`).  While a descendant combinator selects a descendant element any number of elements deep, the child combinator selects only one level deep.  For example, given this CSS:

```css
.foo .bar { /* descendant selector */
  color: red;
}

.foo > .bar { /* child selector */
  color: green;
}
```

then

```html
<div class="foo">
  <p class="bar">This will be green</p>
  <div>
    <p class="bar">But this will be red</p>
  </div>
</div>
```

A `+` is an **adjacent sibling combinator**.  It is used to select an element that immediately follows another:

```css
.foo + span {
  color: red;
}
```
```html
<div>
  <span>Default text color</span>
  <span class="foo">Default text color</span>
  <span>Red text (adjacent sibling to foo)</span>
  <span>Default text color</span>
</div>
```

Finally, `~` is a **general sibling combinator**.  It is used to target *all* sibling elements that follow another:

```css
.foo + span {
  color: blue;
}
```
```html
<div>
  <span>Default text color (sibling, but before foo)</span>
  <span class="foo">Default text color</span>
  <span>Blue text (sibling of foo)</span>
  <span>Blue text (also a sibling)</span>
</div>
```

You may have noticed there are not combinators for selecting previous siblings or parent/ancestor elements (e.g. there is no selector for "a `<p>` that has a child with class `foo`").  There has been discussion concerning these for a long time, but they are difficult to implement in the browser without hindering performance.  They may happen some day, but I wouldn't count on them being supported any time soon.


## Attribute Selectors

Apart from just ids and classes, you can also target elements based on any of their other attributes using **attribute selectors**.

```css
input[disabled] {
  background: lightgray;
}
```

This will make the text of disabled form inputs grey (`<input type="text" disabled="true">`).  It does not matter what the value of the attribute is, as long as it is present.  If you want to select based on the value of an attribute, you can use one of the following formats:

`[attr="value"]`: Selects elements where the specified attribute equals the specified value.  For example, `input[type="text"]` targets text inputs.

`[attr~="value"]`: Selects elements where the specified attribute includes the specified value anywhere in a list of space separated values.  `div[class~="foo"]` is equivalent to `div.foo`.

`[attr^="value"]`: Selects elements with the specified attribute whose value *begins with* the specified value.  For example, `*[id^="chapter"]` would select `<h2 id="chapter-one">` as well as `<h3 id="chapter-two">`, etc.

`[attr$="value"]`: Selects elements with the specified attribute whose value *ends with* the specified value.  For example, `a[href$=".pdf"]` targets links to PDFs.

`[attr*="value"]`: Selects elements with the specified attribute whose value *includes* the specified value at least once.  For example `[class*="grid-"]` targets the classes `grid-1`, `grid-2`, etc.

`[attr|="value"]`: Selects elements with the specified attribute whose value begins with the specified value, immediately followed by a dash (`-`).  This is commonly used for matching the language attribute: for example `[lang|="en"]` targets `<span lang="en-US">` and `<span lang="en-UK">`.

The attribute selectors can be useful in conjunction with HTML5 `data-*` attributes.  Perhaps you have tag metadata on a series of articles (`<article data-tags="monday tuesday wednesday">`); you can use `[attr~="tuesday"]` to select all articles with the `tuesday` tag.

## Pseudo-classes

These are used to target elements when they are in a special state.

<!--- Already seen in ch2: links -->
`:hover` can also be used to add hover styles to any element, not just links:

<!--- better example? -->
```css
.foo:hover {
  background: orange;
}
```

There are more advanced uses for this, which we will look at later; it can be used to do things like open dropdown menus or show a full-size image while hovering over a thumbnail.  Just be aware that the user cannot "hover" when using a touchscreen device, thoughsome devices will toggle the hover functionality when the user taps.
`
### Forms Input State

There are several pseudo-classes for styling form elements in various states:

`:checked`: Target selected checkboxes.

`:focus`: Target the form element that has focus, either by the user clicking it or tabbing to it.

`:active`: Like links, this can also be for buttons, to style their appearance while being clicked or tapped.

`:enabled` and `:disabled`: Target enabled or disabled elements.  `:disabled` is effectively the same as using a `[disabled]` attribute selector.

`:valid` and `:invalid`: HTML5 introduced several form validation features, such as email inputs that must be a valid email address, or a required field that must not be empty.  Use these to target any input with valid or invalid content.

`:in-range` and `:out-of-range`: When inputs have `min` and/or `max` attributes, use this to target them when their current value is in or out of the specified range.


We'll look at styling forms more in depth later on, but here's an example to give you an idea how these pseudo-classes can be used:

```css
/* Style a default border around inputs */
input,
textarea {
  border: darkgray 1px solid;
}

/* Change the background to blue to highlight the current input */
input:focus,
textarea: focus {
  border-color: blue;
}

/* Turn invalid form values red so the user can notice and correct it */
input:invalid,
textarea:invalid {
  color: red;
  border-color: red;
}
```

### "Child" and "Nth" Selectors

There are a series of pseudo-classes for targeting elements based on their position relative to sibling elements and in other contexts:

`:first-child` and `:last-child`: Target elements that are the first or last child under their parent.

`:first-of-type` and `:last-of-type`: Target elements that are the first or last child of a their tag type under their parent.  For example, if there are three `<span>`s followed by an `<input>` as children of a container, the first `<span>` and the `<input>` could be targeted with a selector using `:first-of-type`.

`:nth-child(<formula>)` and `:nth-last-child(<formula>)`: Use a mathematical formula to target children based on their position under their parent.

`:nth-of-type(<formula>)` and `:nth-last-of-type(<formula>)`: Use a mathematical formula to target children based on their position among other elements of the same type under their parent.

`:only-child`: Target an element that is the only child of its parent.

`:empty`: Target an element that has no children elements.

`:not(<selector>)`: Target an element that does not meet the criteria of a given simple selector.  For example, `:not(div)` will target all elements but `<div>`s, and `p:not(.foo)` will target all `<p>` that do not have the class `foo`.



There are some more pseudo-classes not mentioned here, but these are all of the most commonly-used ones.
