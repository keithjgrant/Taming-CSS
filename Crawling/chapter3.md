# Taming CSS
# Chapter 3: Cascade, Specificity, & Inheritance

By now you know how to apply some styling to the page.  You can set colors, style links, and start to give your site or app a design of its own.  As your stylesheet grows, you will start to encounter instances where your style declarations conflict with one another.  Consider the following example:

```html
<h1>My Incredible Website</h1>
<p id="nav">
  <a href="/">Home</a>
  <a href="/widgets">Widgets</a>
  <a href="/about">About</a>
  <a href="/special" class="featured">Special!</a>
</p>
```

We want this to be the start of our page.  It has a heading and our main navigation, consisting of a list of links.  In this list, we have a featured link, which we want to make orange to it stands out.  So let's put together some CSS:

```css
body {
  font-family: sans-serif;
}

h1 {
  color: darkslategray;
}

a:link {
  color: blue;
}

#nav a {
  color: white;
  background: cornflowerblue;
  padding: 5px;
  border-radius: 5px;
  text-decoration: none;
}

.featured {
  background: orange;
}
```

Load it up, and we get this:

<img src="figure3-1.png"/>

Hmm.  That didn't work out quite like we pictured.  Our default link color is blue, and we were able to change that in the Nav to have white text on a blue background.  However, our featured link didn't turn orange.  Why could we override one style but not another?

If you are paying close enough attention, you may have also noticed another odd thing happened: the browser put a gap between our heading and the nav.  We didn't specify any margins, so why is that there?  What if we want those to be close together?

## The Cascade

The answer to these mysteries lies in understanding **the cascade**.  The cascade is the C in CSS: Cascading Style Sheets, so clearly it must be important.

The cascade refers to the set of rules the browser uses to resolve any ambiguity in the stylesheets.  There are several things that can affect how these are resolved: origin, specificity, importance, and source order.  Let's look at these one at a time.

### Stylesheet Origin

Believe it or not, the stylesheets you add to your webpage are not the only ones the browser applies.  Browsers also add their own set of default styles, known as the *user agent* stylesheet.  These vary from browser to browser, but generally they do the same common things: headings (`<h1>` through `<h6>`) are given a top and bottom margin, `<p>` is given a bottom margin, `<ul>` is given a left padding, and default font sizes are set for all of these.

In our page, this is the source of the mysterious gap below our heading.  For each element on the page, the browser first looks in your stylesheets (known as *author* stylesheets) and uses the attributes specified there.  Then it looks for any in the user agent stylesheet, and applies any properties specified there which have not already been specified in the author stylesheet.  Thankfully, the user agent styles set things we typically want, so they don't do anything entirely unexpected.  When you don't like what they do to a certain property, you simply have to set your own value in your stylesheet.

There is a third type of stylesheet, called the *user* stylesheet.  These are styles specified by the user to be applied to the page.  These are extremely uncommon, however, and some modern browsers have actually removed this functionality entirely.  You typically do not need to concern yourself with these, but for the sake of completeness, I will explain how the browser applies them.

These are applied between the author styles and the user agent styles, with one exception: any declarations that have `!important` added at the end of them in the user stylesheet are taken over styles in the author stylesheet.  This is true even if the author stylesheet also specifies an `!important` value for that property!  So the overall order of preference is this:

* User Agent declarations
* User declarations
* Author declarations
* Important Author declarations
* Important User declarations

This order of importance is applied for every attribute of every element on the page independantly.  So if the User specifies a color and an important background for a given element, while you specify a color and a background for that same element, the browser will choose your color but the user's background.  Don't concern yourself too much with this, though.  You have no way of knowing what styles the user may specify, and even if you did, there is nothing you can do about it if they specify their styles with `!important`.

The `!important` declaration is an interesting quirk of CSS, which we will come back to again shortly.

### Specificity

A very important topic to understand is **specificity**.  You can go a long way without an understanding of stylesheet origin, but if you don't understand specificity, it will bite you.  Sadly, specificity is often a missed concept.  When giving job interviews, I always ask about specificity.

If the stylesheet origin does not resolve conflicting declarations (and it usually won't), then then the browser tries to resolve based on specificity.  When two attributes apply to the same element, the one with the most specific selector is applied.  For instance, `.bar .bar`, with two class names, has a higher specificity than `.bar`, which has only one.

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

### Importance

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

### Source Order

