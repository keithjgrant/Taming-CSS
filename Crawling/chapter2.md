# Taming CSS
# Chapter 2: Styling Essentials

Now that you know the syntax of CSS and know how to write selectors, let's get a little more familiar with some styling.  We have already seen a few common properties.  Let's take a closer look at them, as well as some other useful ones.

## Styling Text

Let's do a little more with the text on our page.  Place these styles in your stylesheet:

```css
body {
  color: slategray;
  font-family: "Helvetica", "Arial", sans-serif;
  font-size: 14px;
}
.incredible {
  font-weight: bold;
}
```

And in the body of your HTML:

```html
<p>My <span class="incredible">Incredible</span> Website</p>
```

In our browser, this gives us:

<img src="images/figure2-1.png"/>

The `color` property we are familiar with.  Here we've set it to the color "slategray".

`font-family` specifies what typeface we want use.  You'll notice we specified three different values, separated by commas.  These specify fallback values, which is unique to this particular property.  Different users will have different fonts installed on their systems, so when the specified font is not available, the browser can look for through the list until it finds one that it can use.

The final value, `sans-serif`, is a special generic font.  You should always include a generic font at the end of your list.  If none of the specified fonts are available, the browser can load the default font of the appropriate type.  `serif`, `sans-serif`, and `monospace` are typically want you will want to use for your fallback, though `cursive` and `fantasy` are also valid fallbacks.

For a long time, fonts in CSS were limited to a short list of ones that are common to most systems; often they were paired up so a font common to Apple computers and one common to Windows computers were listed together.  This would ensure the user would have at least one of them.  In our example, these are Helvetica and Arial.  Other common sans-serif pairs included Impact and Charcoal, Geneva and Tahoma, or Geneva and Verdana.  Common serif pairs were Times and Times New Roman, Book Antiqua and Palatino Linotype, or Georgia (which is common to both operating systems).  These were called "web safe" fonts.  You could name a more exotic font, like "Helvetica Neue", but it would only work for users who happened to have that font installed.

With CSS3, we can now use **web fonts**, which allow use a font that is hosted online.  Modern browsers can download this font and use it on the page.  This has opened up our options dramatically.  We will look at web fonts later on, but in the meantime, you can look for font services such as Google Fonts that provide a large library of fonts to select from and simple code snippets to use them.

The next declaration in our example sets the `font-size`.  We specified our units in pixels, but there is actually an alarming number of other options available to us.  We will look at some of the common ones a little later on.

The final property in our example is `font-weight`.  Common values for this are `normal` and `bold`.  You may also specify values from `100` through `900`, in increments of 100.  `400` is the equivalent of `normal` and `700` is the equivalent of `bold`.  The number values are particularly useful in conjunction with web fonts.  If, for example, you provide an "Ultra Light" or "Black" variant from the font family, you can specify these with `100` or `900`, respectively.  If the exact weight you specify is not available, the browser will use the closest value it can.

There are also a `font-style` and `text-decoration` properties.  `font-style` accepts the values `normal` and `italic`.  `text-decoration` accepts `underline`, `overline`, `line-through`, and `none`.

## Backgrounds

So far, we've used the `background` property to set background colors, but it can also be used to apply background images or color gradients.

```css
.star-bg {
  background: url(star.png) lightgray center no-repeat;
}
```

<img src="images/figure2-2.png"/>

This set a background image and a background color.  `center` positions the background image in the element and `no-repeat` means the image will not tile or repeat to fill the element.  We could also position with "top", "bottom", "left", "right", or any valid combination of them such as `bottom right` or `center left`.  Likewise, we can specify tiling with `repeat`.   `repeat-x` or `repeat-y` may be used to tile only horizontally or vertically, respectively.  You'll notice that the top and bottom of the image have been clipped off, because the element is not as tall as our background image; the background does not change the size of the element.  Here is an example with the image tiled:

```css
.star-bg-tiled {
  background: url(star.png) lightgray top left repeat;
}
```

<img src="images/figure2-3.png"/>

`background` is an example of a **shorthand property**.  A shorthand property allows you to set the values of several properties at the same time.  `background` allows you to set values for `background-color`, `background-image`, `background-position`, `background-repeat` with one declaration.  Expressed the long way, the above ruleset is equivalent to this one:

```css
.star-bg-tiled {
  background-color: lightgray;
  background-image: url(background.png);
  background-position: top left;
  background-repeat: repeat;
}
```

This is a bit verbose.  You can see why we will generally favor a shorthand property when we can.  Not all of them are required in the shorthand.  We could just use `background: url(background.png) gray;`, if we wanted to use the default values for position and repeat ("top left" and "repeat").

Often, if a series of property names are hyphenated and begin with the same word, there is a shorthand property to succinctly define them together.  `font` is another shorthand property, which defines `font-style`, `font-variant`, `font-weight`, `font-size`, `line-height`, and `font-family`.  `border`, `padding`, and `margin` are some other common shorthand properties.  The order you specify values often matters, so as you learn shorthand properties, it is important to make note of the order.

## Borders

You can put a border around an element using `border`.  We saw this briefly in chapter one with the declaration `border: blue 1px solid`.  This is a shorthand property, setting `border-color`, `border-width`, and `border-style`.  Here is an element with this border:

<img src="images/figure2-4.png"/>

Usually, you will want a `border-style` of `solid`, but other common values are `dotted`, `dashed`, and `none`.

Sometimes, you will want different border styles on different sides of an element.  You can specify one side at a time with `border-top`, `border-right`, `border-bottom`, and `border-left`.  These follow the same shorthand pattern of color, width, and style.

Alternatively, may use `border-color`, `border-width`, or `border-style` to specify values for each of the four sides independently:

```css
.crazy-border {
  border-color: blue red orange green;
  border-width: 2px 5px 4px 1px;
  border-style: solid dotted solid dashed;
}
```

<img src="images/figure2-5.png"/>

Note the order of these values: top, right, bottom, left.  You will see this order for other attributes as well.  It is clockwise order, or if it is easier for you to remember, think "TRouBLe", which includes the first letter of each direction in the correct order.

Properties whose values follow this pattern all support some shorter notations.  If the declaration ends before one of the four sides is given a value, that side takes its value from the opposite side.  Specify only three values, and the left and right side will both use the second one.  Specify only two values, and the top and bottom will use the first one.  If you specify only one value, it will apply to all four sides.  Thus the following declarations are all equivalent to one another:

```css
border-width: 1px 2px;
border-width: 1px 2px 1px;
border-width: 1px 2px 1px 2px;
```

And the this set are equivalent to one another as well:

```css
border-width: 1px;
border-width: 1px 1px;
border-width: 1px 1px 1px;
border-width: 1px 1px 1px 1px;
```

There are also border properties `border-top-width`, `border-left-style`, `border-bottom-color`, etc.  Needless to say, there are a lot of ways to mix and match these if you want to build a complicated border, but often the regular `border` attribute is enough.

One last border property that can be convenient is `border-radius`.  This is used to make the corners of our element round.  `border-radius: 3px;` gives an element slightly rounded corners, while a higher value such as `15px` rounds them off much more.  If the element has equal height and width, a border radius equals to half its height will change the element's shape into a circle.

## Padding

You will notice in our examples for both backgrounds and borders, the text appears right up against the edge of our element.  You can correct this with `padding`.  This adds space inside the element between its border and its contents.

```css
.blue-border-padded {
  border: blue 1px solid;
  padding: 5px;
}
```

<img src="images/figure2-6.png"/>

`padding` is also a shorthand property for top, right, bottom, and left values.  You can also use `padding-top`, `padding-right`, etc. to set them one at a time.

## Links

When starting a new project, one of the first thing you'll generally want to style are links.  These require some special selectors:

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

The order of these selectors is important.  If a visited link is active, we want the active color to appear, so we put that selector later, etc.  We will look more at why this behaves the way it does in chapter 4, but for now, a helpful acronym to remember this order is "LoVe HAte".

The `:active` state is often overlooked.  We usually develop on fast computers with fast Internet connections.  When we click a link and the next page loads in an instant, we don't even notice that quick flicker of red text.  But on a slow connection--say, on a smartphone with a 3G connection--that red text means a lot.  It tells the user, for the two or three seconds after they tap a link but before anything else on the screen changes, that the page is loading, and they do not need to tap again.  Be sure to change something visually with the active state, even if it is subtle.

The `a:link` is a bit of an odd relic from early HTML.  Originally, there were two uses for the `<a>` tag.  The first was a standard link, using the `href` attribute.  The second was a named key point in your document such as `<a name="section-two">`.  Then you could link directly to that point on the page using `<a href="#section-two">`.  This is where the name "anchor" comes from, as it was where one webpage was "anchored" to another via a hyperlink.

The second use is no longer practiced much, and is in fact deprecated in HTML5.  You can obtain the same behavior by using an id instead, and you can put it on any type of element you want, not just an `<a>` (`<span id="section-two">`).

In modern web applications, it is common to have anchors that activate functionality via JavaScript, and don't in fact link directly to another page.   This creates a bit of a problem, as we still want them to look and behave like a normal link.  To get this behavior, we often see `href="#"` or `href="javascript:void()"`, because there must be an `href` for the browser to treat it like a link.  Then (if we used `href="#"`), we have to be sure to call `event.preventDefault()` in the JavaScript handler to stop the browser from following the link.

Another, less common, option is to omit the `href` attribute and style `a` instead of `a:link`.  The link behavior we lose when we do this the underline and the cursor behavior when mousing over the link.  Thankfully, we can easily replicate both of these with CSS:

```css
a {
  text-decoration: underline;
  cursor: pointer;
}
```

I'm not saying you should always take this approach, but it is worth considering, especially in a web app where you find yourself using a lot of `<a>` tags.  (That said, I always add an `href` when there is a url that is relevant; that way, a ctrl-click will still open the url in a new window.)

### Real-world Example

Let's put all this to use.  Here are some styles for button:

```css
.button {
  color: white;
  background-color: steelblue;
  border: darkblue 1px solid;
  padding: 5px 15px;
  text-decoration: none;
}
.button:hover {
  background-color: darkblue;
}
```

```html
<a class="button" href="next.html">Click me</a>
```

These result in:

<img src="images/figure2-7.png"/>

Which, when we hover our cursor over it, becomes:

<img src="images/figure2-8.png"/>

Try some new things with these declarations.  Make the padding different on one side.  Change the border color and observe how it no longer blends in on the hover state.  Change the font or add a background image.

When you are starting to feel comfortable with these attributes, add another button to the page with different styles.  Add a paragraph before or after the button and see how they look together.  If you play around with them long enough, you might even start to come up with new questions about why behave a certain way or how to do something new.  That's good!  You're learning, and there's plenty more to learn still.
