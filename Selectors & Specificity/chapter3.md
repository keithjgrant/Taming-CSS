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

Now, if both of these selectors targeted the same element, the color `#999` would be used.  `!important` even overrides an inline style.  However, if both declarations have an `!important`, then the regular specificity rules apply.

Be careful with `!important`, however, or you'll find you need to use it more and more to continue overriding declarations where you've already used it.

## Pseudo-classes & Pseudo-elements

## Advanced Selectors

