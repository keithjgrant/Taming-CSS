# Taming CSS: Selectors & Specificity
# Chapter 3: Selectors

## Basic Selectors

Selectors are perhaps the most important part of CSS.  Countless articles and books have been written examining selectors and the best way to construct and organize them.  We will examine this in detail in *Taming CSS: Organization*, but for now, let's look at the different ways to construct selectors.

The three most simple selectors are the **tag selector** (also called a type selector), the **class selector**, and the **id selector**.  As we saw in chapter two, a tag selector looks like the name of the tag it is targeting: `p` to target all `<p>`, `h1` to target all `<h1>`.

The class selector begins with a `.`: `.navmenu` targets `<ul class="navmenu">`, or any other elements with that class.  The tag name is irrelevant: `.foo` targets `<div class="foo">` as well as `<li class="foo bar">`.  Note that targeted elements may also have additional classes.

ID selectors begin with a `#`.  `#sidebar` targets the element with that id, `<div id="sidebar">` for instance.  In HTML, ids must be unique, meaning only one element on the page may have a given id and each element may only have one id; if you need to put an identifier on multiple elements, use a class instead.

### Combining Selectors

Selectors may be combined to make them more specific.  `p.foo` targets `<p class="foo">` but not `<p>` or `<div class="foo">`.  `#navmenu.is-open` targets the `#navmenu`, but only if it has the class `is-open` applied.  `.bar.baz` targets elements that have both the `bar` and `baz` classes.

A space between two selectors is a **descendant combinator**:

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

You are not limited to one combinator.  String many together to do things like target items in nested lists with `ul li ul li` or anchors in navigation list items with `ul.nav li a`.

There are some other types of combinators, too:

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

### Universal Selector

You can select all elements with the universal selector `*`.  At one point, it was a common practice to use this to do a "universal reset".

```css
/* Don't do this */
* {
  margin: 0;
  padding: 0;
}
```

This reset all the browser's default margin and padding on all elements to make development across multiple browsers more uniform.  This is not a recommended practice, however, as it is more heavy-handed than you probably want.  Lists generally need a left padding, paragraphs and headers generally need a top and/or bottom margin, etc.  We will take a look at better ways to do browser resets in chapter five.

The universal selector can also be combined.  For example, `.foo *` targets all elements that descend from `.foo`; `.bar > *` selects all direct descendants of `.bar`.

You may hear that `*` should be avoided for performance reasons.  In truth, this is a holdover from IE6.  These days, it is generally fine to use in all modern browsers.

## Specificity

A very important topic to understand is **specificity**.  Of everything we'll cover in this book, specificity is probably the most often missed as people learn the basics.  When giving job interviews, I always ask about specificity.

Specificity is how the browser chooses between conflicting declarations.  When multiple different declarations need to be resolved, the one with the most specific selector is applied.  For instance, `.bar .bar`, with two class names, has a higher specificity than `.bar`, which has only one.

The exact rules of specificity are as follows:  The selector with the most ids wins (i.e., it is more specific).  If that results in a tie, the selector with the most classes wins.  If that results in a tie, the selector with the most tag names wins.  If all three are equal, whichever one appears later in the stylesheet wins.

A common way to write this is to count each of them up and separate the sums by commas.  So `#foo .bar p` has a specificity of 1,1,1.  `ul li` has a specificity of 0,0,2.  A specificity of 1,0,0 wins over a specificity of 0,2,2, and even over 0,0,10 (though I don't recommend ever writing selectors as long as one with 10 tags).

Consider the following CSS:

```css
#sidebar ul {
  color: #000;
}

ul.main-nav {
  background: #000;
  color: #999;
}
```

It is a simple concept, but if you don't understand specificity, you can drive youself mad trying to figure out why your navigation menu has black text on a black background instead of the grey on black you specified.  Note that only the declarations in conflict are an issue.  Other declarations in the ruleset with the less-specific selector are still applied.

There are two ways to get around specificity rules.  First, you can declare a style inline.  Inline styles are more specific than those applied via a selector.  Second, you can add **!important** to the end of the declaration:

```css
#sidebar ul {
  color: #000;
}

ul.main-nav {
  background: #000;
  color: #999 !important;
}
```

Now, when both of these selectors target the same element, the color `#999` would be used.  `!important` even overrides an inline style.

Be careful with `!important`, however.  When two conflicting declarations both have an `!important`, then the regular specificity rules apply.  If you rely on it too much, you'll find you need to use it more and more to continue overriding declarations where you've already used it.

Some people will go so far as to say you should never use `!important`.  If you are just starting out in CSS, this is very good advice, but don't cling to it forever.  I believe it has a place, specifically in utility classes.  I've also had to use it occasionally to override inline styles added by a third-party JavaScript libary.

This brings up an important point about specificity: if you are creating a package for distribution, I strongly urge you not to apply styles inline via JavaScript.  If you do, you are forcing developers using your package to either accept your styles exactly, or to use `!important` for every property they want to change.  Instead, include a stylesheet in your package.  If your component needs to make style changes dynamically, it is almost always preferable to use JavaScript to add and remove classes to the elements.  Then users can simply use your stylesheet, and they have the option to edit it however they like without battling specificity.

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

## Pseudo-Classes

### Links

When starting a new project, one of the first thing you'll generally want to style are links.  These require a special type of selector called a **pseudo-classes**.  These are used to target elements when they are in a special state.

```css
a:link {
  color: blue;
  text-decoration: underline;
}
a:visited {
  color: purple;
}
a:hover {
  color: darkblue;
}
a:active {
  color: red;
}
```

`a:link` refers to an `<a>` that has an href attribute.  `a:visited` refers to a link that the user has already been to before (i.e. its url is in the browser's history).  `a:hover` specifies styles to apply to the link when the user hovers their mouse cursor over it.  `a:active` styles the link after the user activates it, whether by clicking it with their mouse, tabbing to it with their keyboard and pressing enter, or tapping it on a touchscreen device.

Note the order of these selectors.  Because they all have the same specificity, the cascade causes a later declaration to override an earlier one.  If a visited link is active, we want the active color to appear, so we put that selector later, etc.  For years, I knew the order of these mattered, but never stopped to think about why.  There's no deep voodoo to it; it's just the cascade doing what it does.  If you don't want to stop and reason through the cascade every time, though, a helpful acronym to remember this order is "LoVe HAte".

The `:active` state is often overlooked.  We usually develop on fast computers with fast Internet connections.  When we click a link and the next page loads in an instant, we don't even notice that quick flicker of red text.  But on a slow connection--say, on a smartphone with a 3G connection--that red text means a lot.  It tells the user, for the two or three seconds after they tap a link but before anything else on the screen changes, that the page is loading, and they do not need to tap again.  Be sure to do something visually with the active state, even if it is very subtle.

The `a:link` is a bit of an odd relic from early HTML.  Originally, there were two uses for the `<a>` tag.  The first was a standard link, using the `href` attribute.  The second was a named key point in your document such as `<a name="section-two">`.  Then you could link directly to that point on the page using `<a href="#section-two">`.  This is where the name "anchor" comes from, as it was where one webpage was "anchored" to another via a hyperlink.

The second use is no longer practiced much, and is in fact deprecated in HTML5.  You can obtain the same behavior by using an id instead, and you can put it on any type of element you want, not just an `<a>` (`<span id="section-two">`).

In modern web applications, it is common to have anchors that activate functionality via JavaScript, and don't in fact link directly to another page.   This creates a bit of a problem, as we still want them to look and behave like a normal link.  To get this behavior, we often see `href="#"` or `href="javascript:void()"`, because there must be an `href` for the browser to treat it like a link.  Then, at least with `href="#"`, we have to be sure to call `event.preventDefault()` in the JavaScript handler to stop the browser from following the link.

Another, less common, option is to omit the `href` attribute and style `a` instead of `a:link`.  The link behavior we lose when we do this the underline and the cursor behavior when mousing over the link.  Thankfully, we can easily replicate both of these with CSS:

```css
a {
  text-decoration: underline;
  cursor: pointer;
}
```

I'm not saying you should always take this approach, but it is worth considering, especially in a web app where you find yourself using a lot of `<a>` tags.  (That said, always add an `href` when there is a url that makes sense; that way, a ctrl-click will still open the url in a new window.)

`:hover` can also be used to add hover styles to any element, not just links:

<!--- better example? -->
```css
.foo:hover {
  background: orange;
}
```

There are more advanced uses for this, which we will look at later; it can be used to do things like open dropdown menus or show a full-size image while hovering over a thumbnail.  Just be aware that the user cannot "hover" when using a touchscreen device, thoughsome devices will toggle the hover functionality when the user taps.

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

## Pseudo-Elements

While pseudo-classes are used to select elements in a certain state, **pseudo-elements** are used to target certain parts of the page.

<!---
::first-letter
::first-line
::before
::after
::selection
-->

## That's a Lot of Selectors
