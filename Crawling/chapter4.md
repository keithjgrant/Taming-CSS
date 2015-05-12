# Taming CSS
# Chapter 4: Cascade, Specificity, & Inheritance

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

We want this to be the start of our page.  It has a heading and our main navigation, consisting of a list of links.  In this list, we have a featured link, which we want to make orange so it stands out.  So let's put together some CSS:

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

<img src="images/figure3-1.png"/>

Hmm.  That didn't work out quite like we pictured.  It turns out, three different selectors target out featured link.  The default link color is blue.  We were able to change that in the nav to have white text on a blue background.  However, our featured link didn't turn orange.  Why could we override one style but not another?

If you are paying close enough attention, you may have also noticed another odd thing happened: the browser put a gap between our heading and the nav.  We didn't specify any margins, so why is that there?  What if we want those to be close together?

## The Cascade

The answer to these mysteries lies in understanding **the cascade**.  The cascade is the C in CSS: Cascading Style Sheets, so clearly it must be important.

The cascade refers to the set of rules the browser uses to resolve any ambiguity in the stylesheets.  When two different declarations for the same property apply to the same element, this conflict has to be resolved predictably.  There are several things that can affect how these are resolved: origin, specificity, importance, and source order.  Let's look at these one at a time.

### Stylesheet Origin

Believe it or not, the stylesheets you add to your webpage are not the only ones the browser applies.  Browsers also add their own set of default styles, known as the *user agent* stylesheet.  These vary from browser to browser, but generally they do the same common things: headings (`<h1>` through `<h6>`) and `<p>` are given a top and bottom margin, lists (`<ol>` and `<ul>`) are given a left padding, and default font sizes are set.

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

If conflicting declarations are found from the same origin, the browser then tries to resolve the conflict by looking at their **specificity**.  Specificity is essential to understand.  You can go a long way without an understanding of stylesheet origin, because 99% of the styles on your website will be coming from the same origin.  But if you don't understand specificity, it will bite you.  Sadly, it is often a missed concept.  When giving job interviews, I always ask about specificity.

Specificity is determined by the selectors of the conflicting declarations.  For instance, `.bar .bar`, with two class names, has a higher specificity than `.bar`, which has only one.  The exact rules of specificity are as follows:  If a style is inline, it wins.  Otherwise, the selector with the most ids wins (i.e., it is more specific).  If that results in a tie, the selector with the most classes wins.  If that results in a tie, the selector with the most tag names wins.

A common way to write this is to count each of them up and separate the sums by commas.  So `#foo .bar p` has a specificity of 1,1,1.  `ul li` has a specificity of 0,0,2.  A specificity of 1,0,0 wins over a specificity of 0,2,2, and even over 0,0,10 (though I don't recommend ever writing selectors as long as one with 10 tags).

Let's look back at part of our CSS from earlier:

```css
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

It is a simple concept, but if you don't understand specificity, you can drive youself mad trying to figure out why your white color overrode the blue, but the orange background did not override the cornflowerblue one.  `#nav a` is the most specific selector here.  Only a selector of specificity 1,0,1 or higher can override it.  Note that only the declarations in conflict are an issue.  Other declarations in the ruleset with the less-specific selector are still applied.

### Source Order

The third and final step to resolving the cascade is source order.  If the origin is the same, and the specificity is the same, then the declaration that appears later in the stylesheet--or appears in a stylesheet included later on the page--wins.

### Working With the Cascade

Now that we understand how the cascade behaves, let's look back to our example and see if we can solve our problems with our nav menu.

We know the user agent stylesheet is adding some margins to our `<h1>` and `<p>`, we simply have to specify the values we want to override them.  Let's bring them a bit closer together:

```css
h1 {
  color: darkslategray;
  margin-bottom: 10px;
}

.nav {
    margin-top: 10px;
}
```

As for the orange "featured" button, we have some options to consider.  The quickest fix is to add an `!important` to the declaration we want to favor:

```css
/* fix 1 */
.featured {
  background: orange !important;
}
```

This is sure easy, but it is also a naive fix.  It may do the trick now, but it can cause problems down the road.  If we start adding `!important` to our declarations, what happens when we need to trump something that is already set to important?  When we give both conflicting declarations an `!important`, then the regular specificity rules still apply.  This will ultimately leave us back where we started.

Let's find a better way.  Instead of trying to get around the rules of selector specificity, let's try to make them work for us.  What if we raised the specificity of our selector?

```css
/* fix 2 */
#nav .featured {
  background: orange;
}
```

This works.  Now, our selector has one id and one class, giving it a specificity of 1,1,0, which is higher than `#nav a`, which has a specificity of 1,0,1.

We can still make this better, though.  Instead of *raising* the specificity of the second selector, let's see if we can *lower* the specificity of the first.  Does "nav" really need to be an id?  Perhaps, if we're using that id in our JavaScript to find that element on the page, but that doesn't mean we have to use it in our CSS.  Let's give it a class as well: `<p id="nav" class="nav">`.  Then we can change our CSS a bit:

```css
/* fix 3 */
.nav a {
  color: white;
  background: cornflowerblue;
  padding: 5px;
  border-radius: 5px;
  text-decoration: none;
}

.nav .featured {
  background: orange;
}
```

Now we've brought the specificity of the first selector down, though we did have to raise the specificity of our second a bit.  We might be able to find ways to go even further.  Let's consider one other option:

```css
/* fix 4 */
.nav a {
  color: white;
  background: cornflowerblue;
  padding: 5px;
  border-radius: 5px;
  text-decoration: none;
}

a.featured {
  background: orange;
}
```

In this solution, the specificities are equal, which means source order determines which declaration is applied to our link.  It addresses our problem, but potentially also introduces a new one: What happens now if we want to use the "featured" class on another link outside of our nav?  Doing so would create an element that is targeted only by the second selector, not the first.  It would have an orange background, but not the text color, padding, or border-radius.  It would also have the default underline that appears on links.  We have to decide whether we want this button to work outside of the `.nav`, and if we do, we have to make sure those declarations apply to it as well.

For now, I don't think that's what I want to do, so I'm going to stick with fix 3.  In your website, you will ideally be able to make some educated guesses about your needs elsewhere on the site.  Then maybe fix 4 would be what you want.  Very often in CSS, the best answer is, "it depends".

Now that we're happy with our fix, let's take a look at the result:

<img src="images/figure3-2.png"/>

As you can see from these examples, specificity tends to become a sort of arms race, especially on large projects.  It is generally best to keep it low when you can, so when you need to override something, your options are open.

Two common rules of thumb you may hear are, first, to never use ids in your selectors, and second, never use `!important`.  While you are just starting out in CSS, these can be good advice, but don't cling to them forever.  I believe both have a place.  Specifically, `!important` can be useful in utility classes.  I've also had to use it occasionally to override inline styles added by a third-party JavaScript libary.

This brings up an important point about specificity: if you are creating a package for distribution, I strongly urge you not to apply styles inline via JavaScript.  If you do, you are forcing developers using your package to either accept your styles exactly, or to use `!important` for every property they want to change.  Instead, include a stylesheet in your package.  If your component needs to make style changes dynamically, it is almost always preferable to use JavaScript to add and remove classes to the elements.  Then users can simply use your stylesheet, and they have the option to edit it however they like without battling specificity.

A series of practical methodologies have emerged in the last few years to help with managing selector specificity.  We will look at them in detail later on, but now that you understand how the cascade behaves, we can press on.

## Inheritance

There is one last way that an element can receive styles: through **inheritance**.  If, after the cascade is resolved, an element has no assigned value for a given attribute, it may inherit one from an ancestor element.  If you look at our example above, you will notice that both the heading and the nav buttons are displayed with a sans-serif font, but no declarations explicitly assign this to them.  Instead, the `font-family` is set on the `<body>`, and it is inherited from there.

Not all attributes are inherited, however.  Only certain ones are.  In general, these are the attributes you will *want* to be inherited.  This is primarily attributes pertaining to text: `color`, `font`, `font-family`, `font-size`, `font-weight`, `font-variant`, `font-style`, `line-height`, `letter-spacing`, `text-align`, `text-indent`, `text-transform`, `white-space`, `word-spacing`.

A few others such as the list attributes `list-style`, `list-style-type`, `list-style-position`, and `list-style-image`.  The table border attributes `border-collapse` and `border-spacing` inherit, as well; note that these are the attributes that control table border behavior, not the more commonly used attributes for specifying borders for non-table elements.  We wouldn't want a `<div>` passing its border down to every descendant element!  This is not quite a comprehensive list, but very nearly.

Again, you will find these to come naturally.  If you inherently expect something to inherit, it probably will.

```css
.parent {
  font-weight: bold;
}
```
```html
<div class="parent">
  <div class="child">This font will be bold, because the value is inherited from the parent</div>
</div>
```

Remember that the cascade takes precedence before inheritance:

```css
.child {
  font-weight: normal;
}
```

This ruleset here targets the child directly, so the child font will now have a normal font weight.

Sometimes, you stil want inheritance to take place when the cascade is preventing it.  To do this, these attributes also come with a special value, `inherit`.  You can assign this value to an element to override any specified value, and it will inherit normally:

```css
.parent .child {
  font-weight: inherit;
}
```

The child font will now be inherited, and thus bold.

## Summing Up

That might seem like a lot.  Let's review.  An element can be targeted by any number of selectors, and they may all specify values for the same attribute.  When this is the case, only declarations from the highest-priority origin are considered.  If this does not resolve the conflict, the one with the highest-specificity selector is applied.  If there this does not resolve it, the one appearing last in source order is used.  And in cases where an attribute is not set on an element, but it is a an attribute that can be inherited, then the value is taken from the nearest ancestor element where it is set.
