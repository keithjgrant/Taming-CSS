# Taming CSS
# Chapter 5: Document Flow

CSS has two primary responsibilities on a web page: styling and layout.  Styling pertains to the look and feel of the individual elements on the screen.  We have looked at a number of things like font size, backgrounds, and colors which are all about styling.  In this chapter, we will begin to turn our attention to layout.  Layout pertains to the position of elements on the page and in relation to one another.

Think about the last webpage you visited.  Perhaps it had a navigational bar across the top of the screen, with links or even dropdown menus arranged horizontally.  Or maybe it had a menu down the side, with links stacked one atop another.  On some sites, the main navigation remains fixed on the screen as you scroll the page, on others it scrolls normally.  The page likely had a footer at the bottom, below both the main content and any sidebars; on some pages, the sidebar may have been longer than the content, but on others the content may have been longer than the sidebar.  Either way, the footer is below them both.  There are also less obvious elements in the design: modal dialog boxes, expanding search boxes, promotional sections that appear side by side or in a grid.  All of these things are part of the layout.

Layout is one of your first major design concerns when designing a new site.  It can also become very complex, depending on what you wish to do.  Most questions I get about CSS concern troubles with layout.  There are countless ways to position and size elements on the page, and dozens of ways they can interact with each other.  Needless to say, it is essential to understand the parts of CSS that have to do with layout, and your knowledge of them needs to be thorough enough that you can understand and predict how they will interact with one another when mixed and matched in different ways.

Some of the main tools we use to layout pages are positioning, floats, and sizing.  But before we get to those, we need to understand how the page behaves by default.  We can not effectively change things from their default state if we don't understand what the default state is and how it behaves.  Even if we understand what we are changing an element to, we need to also understand how the elements around it will behave.  This default state is called the **normal document flow**.

The browser displays content in the order it appears in the HTML markup, starting at the top of the page, and works downward as far as it needs to until all content is displayed.  Text will fill the full width of the screen, line wrapping as necessary when it reaches the end.  It looks like this:

<img src="images/figure5-1.png"/>

HTML does something interesting with white space: it collapses it.  White space is not strictly followed, but it is not ignored completely either.  Any amount of consecutive white space&mdash;tabs, line breaks, spaces&mdash;is rendered as a single space.  The HTML from the image above could be written in one long line, or it could be spread out over any number of lines, each indented any number of ways.  We tend to take this for granted, but it comes into play from time to time when we may not expect it.

Now observe what happens when we insert an `<img>` in the middle of the text:

<img src="images/figure5-2.png"/>

The browser makes a few changes here to accomodate the image.  First, a it carves out a gap horizontally between the text where the image appears; the text prior to the image stops ("...lectus. "), the image is rendered, and then the following text resumes (" Ut sem...").  Again, if there is any white space between the text and the image, a space is collapsed and retained as a single space character.

The second change the browser makes is a shift in the vertical spacing of the text, but only for the line where the image appears.  We can observe a similar behavior if we wrap part of the text in a `<span>` that sets a larger font size:

<img src="images/figure5-3.png"/>

This also shifted the vertical spacing of the text, and again, only for the affected line.  But if you notice, it did so a little differently.  With the image, it only added space above the line of text.  In this case, it added space above, but also a little bit below to accomodate the descenders (i.e. the tail of the "q").

The behavior is caused by the `vertical-align` property.  The default vertical alignment is `baseline`.  The baseline is the bottom edge of the text, not including descenders.  If you were to draw a straight line along the base of the small text, it would also go along the base of the large text.  If we gave our span a `vertical-align: bottom`, it would align according to the end of the descenders:

<img src="images/figure5-4.png"/>

We can also align by `top` or `middle`.  However we align the span, the browser will make room above or below the rest of the affected line of text to accomodate the larger font.  We also see similar behavior if the span had a different `line-height`.

In our original example with the image, there were no descenders, so it aligned the bottom of the image on the baseline and only had to make room above the text.  A vertical alignment of `bottom` would have the same result, since the bottom and the baseline of an image are the same.  A vertical alignment of `top` would align the top of the image with the top of the text, making room below the text to accomodate the extra space.

## Inline vs Block-level elements

This behavior belongs to **inline** elements.  Inline elements flow inline with the text of the page, almost as if they themselves are text (and usually, they are).  By default, the following elements are inline:

 * span
 * a
 * b, i, strong, em
 * img
 * br
 * sub, sup
 * label
 * code
 * cite, abbr, acronym

On the other hand, most other elements are **block-level** elements.  Let's see what happens when we put a block-level element in the middle of our text:

```html
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Duis
eget <div class="block-example">aliquet lectum.</div>  Ut
sem sem, varius elementum massa nec, blandit ultricies magna.
Vestibulum eu rutrum massa. Mauris at pretium mauris. Nam
dolor neque, tristique eget nisi sit amet, bibendum commodo
magna.
```

<img src="images/figure5-5.png"/>

A block-level element appears on its own line, with a newline started both before and after.  It also fills the width of its container.  We can change the size of a block-level element using `height` and `width`, but it will still remain on its own line:

```css
.block-example {
  height: 3em;
  width: 50%;
}
```

<img src="images/figure5-6.png"/>

Because a block-level element is always on its own line, setting the `vertical-align` property on one has no effect.  Likewise, setting `height` or `width` on an inline element has no effect.

As we start nesting elements in the page, a block-level element can be used to constrain the bounds of its descendant elements:

<img src="images/figure5-7.png"/>

In this example, I have given the `<div>` a width of 50%, constraining document flow within.  I could also give the element a padding, further constraining the width of the inner content.

## Floats

The normal document flow is limiting.  Sometimes we want our text on one side of the screen and a block-level element beside it.  Sometimes we want more than one block-level element beside one another.  One way we can accomplish this is with **floats**.

Before we start building any fancy layouts with floats, we need to understand what they are, and exactly how they behave.  Countless developers have seen that floats are the most common tool for laying out webpages, and started using them, only to find themselves helpless down the road when things don't behave like they expect.

<img src="images/figure5-8.png"/>

We can float an element in either direction: `float: left` or `float: right`.  This takes the element out of the document flow and shifts it to the edge of the current line of text.  If the element is inline, it is changed to be block-level.  Now in its new location, a space is carved out of the normal document flow where the element is, so content will not overlap with the floated element.

If we float multiple elements, they will stack up alongside one another.  Here are two elements floated left:

<img src="images/figure5-9.png"/>

If we were to put all of our content into two containers, we could float both containers and set their sizes appropriately to create a two-column layout for our page.  Go ahead and try this yourself.  We will look at an example of this in depth in the next chapter.

## The Display Property

Sometimes, we want to force an element to behave as inline or block when it normally behaves as the opposite.  We can do this with the `display` property:

```css
.inline {
  display: inline;
}
.block {
  display: block;
}
```

<!--
other display settings:
none
inline-block
table, table-row, table-cell
-->
