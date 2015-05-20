# Taming CSS
# Chapter 4: Data Types & Units

We've seen a number of various types of declaration values now: keywords like "top" and "left", colors like "slategray", and pixel values like "14px" fonts and "1px" borders.  These are all classified into various **data types**.  In this chapter, we will taking a closer look at these data types.  You should be familiar with them, especially some gotchas involved with a few of them that I will point out.

## Lengths

<!---
keywords
strings
colors
numbers
lengths (px, em, etc)
percentages
uris
-->

A **length** in CSS is used to denote a distance measurement.  It is a number followed by a length unit, such as `5px`.

We have now seen the `px` unit a lot of these examples.  This is short for "pixel".  Specifying `px` does pretty much what it sounds like: it tells the browser to make something display at a precise size.  On a stardard resolution display, this means one pixel on screen.  Devices with high resolution screens, such as a Retina Display, will scale these up, so a CSS "pixel" may actually translate to more than one pixel on the screen, but as far as we are concerned, they are an unchanging value.  The browser tries to define the pixel as 1/96th of an inch.  This is also true when printing a webpage.  Printers typically print at a much higher resolution, usually around 300 dpi, so one CSS "pixel" will generally equate to several dots of ink.  Because of this, pixels are known as an **absolute** length unit.

Other absolute units are:

 * `mm`: one millimeter
 * `cm`: one centimeter, equal to 10mm
 * `in`: one inch, equal to 2.54cm and 96px
 * `pt`: one "point", which is a typographical term for 1/72 of an inch
 * `pc`: one "pica", another typographical term meaning 12 points

These units are generally less commonly used than `px`, but they can be useful at times.  They are all defined in terms of one another, so they are completely interchangable, as long as you feel like working out the math (96px = 25.4mm = 2.54cm = 1in = 72pt = 6pc).

Up until now, I have used `px` with `font-size` for simplicity, but this is generally not recommended to use absolute units for font size.  Browsers allow users set a default font size ("Small", "Large", "Larger" etc).  If you define absolute font sizes, you will be overriding this setting.  This is distinct than the "zoom" feature most browsers provide, which will resize absolutely sized fonts, as well as everything else on the page.  Since a default font size is vital to some users, particularly those who are vision impaired, it is worth learning to specify fonts with **relative units**.

### Em

"Em"s are the most common relative unit.  Ems are a measure used in typography, referring to the size of the letters, originally the width of a capital M (hence the name "em").  So in CSS, one em (written `1em`) means the font size of *the current element*.  Its exact value varies depending on the element we are applying it to.

```css
.em-example {
  font-size: 16px;
  padding: 1em;
}
```

These will set both the font size and the padding equal to 16 pixels.  Setting the padding to `2em` would make it equal to 32 pixels.  If another selector targets the element and sets a different font size, it will change the meaning of em, and thus the padding.

Using ems can be a very convenient when setting properties such as `padding`, `height`, `width`, or `border-radius`, because these will scale evenly with the element if it inherits different font sizes or the user changes their font settings.

So if ems are defined by the font size, let see what happens when we use them to set the `font-size`:

```css
.em-example-2 {
  font-size: 1.2em;
  padding: 1.2em;
}
```

Now we've set the padding equal to 1.2 times the font size, and the font size equal to 1.2 times... itself?  Obviously, this doesn't make any sense.  When it comes to ems, `font-size` behaves a little differently: instead of ems being relative to the current element's font size, they are relative to the *inherited* font size.  So, if the parent element's font is 16 pixels, then a font-size of `1.2em` equals 19.2 pixels.  Then, we can calculate the current padding as 1.2 times that, giving us 23.04px.  Remember, for any property other than `font-size`, ems refer to the font-size of the current element, after the font size is calculated.

This gets interesting is when elements using ems are nested multiple levels deep:

```css
body {
  font-size: 16px;
}
ul {
  font-size: .8em;
}
```

<img src="images/figure2-7.png"/>

Our text is shrinking!  What happened?  Remember, our `ul` selector targets all `<ul>` on the page, so it sets each list to a font 0.8 times that of its parent.  This means that our first list has a font size of 12.8px, but the next one down is 10.24px (12.8px * 0.8), and the third level is 8.192px, and so on.  Similarly, if we specified a size larger than 1em, our text would be continually growing instead.  Here's how we fix this:

```css
ul {
  font-size: .8em;
}
ul ul {
  font-size: 1em;
}
```

This second selector targets all unordered lists within an unordered list: all of them except the top level.  Nested lists now have a font size equal to their parents:

<img src="images/figure2-8.png"/>

Much better.  Though this feels a little hacky.  It should be clear now that ems can get away from us if we're not careful.  Thankfully, there is a better options, rems.

"Rem" is short for "root em".  Instead of being relative to the current element, rems are relative to the root element, the `<html>` tag.

<!-- comment on html { font-size: 62.5%; } -->
<!--- WORKING HERE -->

<!---
One other important unit is percent.  If a value is a horizontal value (such as `padding-left`) this means a certain perctage of the parent container's width.  If the value is a vertical value, it means a percentage of the parent container's height.  Finally, if you use percent to set a font size, it behaves much like ems; `100%` means equal to the parent container's font size.
-->


## Colors

So far, we have been using named colors like "blue", "slategray", and "orange".  There are about 150 named colors like these that are valid, but it's still fairly limiting.  Thankfully, there is a way we can specify any color we want:

```css
background-color: #3366aa;
```

This is a **hex color**, also known as hex notation.  "Hex" is short for "hexadecimal", which is a base-16 number system.

Unlike our common decimal number system, which is base-10 and uses the ten digits 0 through 9, hexidecimal uses sixteen digits.  We represent these with 0 through 9 as well as A through F.  "A" represents the decimal value "10".  "B" represents "11", et. cetera up through "F" which represents "15".  Capitalization is ignored.

If you were to add one to `F`, you get `10`.  Instead of a tens column, as with decimal numbers, we have a sixteens column.  `11` means 1 sixteen plus 1.  `2A` means 2 sixteens (decimal 32) plus A (decimal 10).  Don't worry too much about the conversion, though.  Suffice it to say, in hex, letters have higher values than numbers; they are kind of like the face cards in a deck of cards.

You can easily get by with a general grasp of the concept.  Then you know that `B9` is larger than `9B`, and `A1` is much larger than `1A`.  Most often, you will be obtaining these values from a image editor or color picker, not writing them by hand off the top of your head.

So how does this get us a color?  A CSS hex color is actually three distinct hexidecimal numbers together, representing values for red, green, and blue.  In `#336699`, `33` is the amount of red, `66` is the amount of green, and `99` is the amount of blue.  Since blue is the largest value, so this color is primarily blue.  Because both digits in each value are equal, this number can be abbreviated as `#369`.

Colors you will common use include `#ffffff` (or `#fff`), which is pure white, and `#000000` (or `#000`), which is pure black.  (We use additive color, which means the higher the value, the more light we are adding).

