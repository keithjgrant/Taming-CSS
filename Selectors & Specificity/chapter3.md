# Taming CSS: Selectors & Specificity
# Chapter 3: Selectors

## Basic Selectors

Selectors are perhaps the most important part of CSS.  Countless articles and books have been written examining selectors and the best way to construct and organize them.  We will examine this in detail in *Taming CSS: Organization*, but for now, let's look at the different ways to construct selectors.

The three most basic selectors are the **tag selector** (also called a type selector), the **class selector**, and the **id selector**.  As we saw in chapter two, a tag selector looks like the name of the tag it is targeting: `p` to target all `<p>`, `h1` to target all `<h1>`.

The class selector begins with a `.`: `.navmenu` targets `<ul class="navmenu">`, or any other elements with that class.  The tag name is irrelevant: `.foo` targets `<div class="foo">` as well as `<li class="foo bar">`.  Note that targeted elements may also have additional classes.

ID selectors begin with a `#`.  `#sidebar` targets the element with that id, `<div id="sidebar">` for instance.  In HTML, ids must be unique, meaning only one element on the page may have a given id and an element may only have one id; if you need to put an identifier on multiple elements, use a class.

### Combining Selectors

Selectors may be combined to make them more specific.  `p.foo` targets `<p class="foo">` but not `<p>` or `<div class="foo">`.

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
```

You are not limited to one combinator.  You can target items in nested lists with `ul li ul li` or links in navigation list items with `ul.nav li a`.

<!---
child >
general sibling ~
adjacent sibling +
-->

### Universal Selector

You can select all elements with the universal selector `*`.  At one point, it was a common practice to use this to do a "universal reset".

```css
/* Don't do this */
* {
  margin: 0;
  padding: 0;
}
```

This reset all the browser's default margin and padding on all elements to make development across multiple browsers more uniform.  This is not a recommended practice, however, as it is more heavy-handed than you probably want.  Lists generally need a left padding, paragraphs and headers generally need a top and/or bottom margin, etc.  We will take a look at better ways to do browser resets in chapter six.

The universal selector can also be combined.  For example, `.foo *` targets all elements that descend from `.foo`.

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

Apart from ids and classes, you can also target elements based on their other attributes.

```css
input[disabled] {
  background: lightgray;
}
```

This will make the text of a disabled form input grey.

<!---
https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors
-->

## Styling Links

When starting a new project, one of the first thing you'll generally want to style are links.  These require a special type of selector called a **pseudo-selector**:

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

The `a:link` is a bit of an odd relic from early HTML.  Originally, there were two uses for the `<a>` tag.  The first was a standard link, using the `href` attribute.  The second was a named key point in your document such as `<a name="section-two">`.  Then you could link directly to that point on the page using `<a href="section-two">`.  This is where the name "anchor" comes from, as it was where one webpage was "anchored" to another via a hyperlink.

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
