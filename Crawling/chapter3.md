# Taming CSS
# Chapter 3: Data Types & Units

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

## Absolute vs. Relative Units

We have now seen the `px` unit a lot of these examples.  This is short for "pixel", and is an example of an absolute unit.

Specifying `px` ("pixels") does pretty much what it sounds like.  It tells the browser to make something display at a precise size.  Devices with high resolution screens, such as a Retina Display, will scale these up, so a CSS "pixel" may actually translate to more than one pixel on the screen, but as far as we are concerned, they are an unchanging value.

Up until now, I have used `px` with font sizing for clarity, but this is generally not recommended.  Some browsers allow users set a default font size ("Small", "Large", "Larger" etc).  If you use `px` sizing, these settings will not work.  Since this functionality is vital to some users, particularly those who are vision impaired, it is worth learning to specify fonts with relative units.

"Em"s are the most common relative unit.  Ems are a measure used in typography, referring to the height of the letters (originally, a capital M, which is where it gets its name).  So in CSS, one em (`1em`) means the height of the current element's font-size.  This mean that it's exact value varies depending on the font size of the element we are applying it to.

```css
.em-example {
  font-size: 16px;
  padding: 1em;
}
```

These will set both the font size and the padding equal to 16 pixels.  This is more interesting when we also set the font size using ems:

```css
.em-example-2 {
  font-size: 1em;
  padding: 1em;
}
```

Now both the font size and the padding are set to the same value... but we don't know exactly how big that value is.  The font-size is inherited from the parent element.  So, if the parent element's font is 16 pixels, then 1em equals 16 pixels.  2em equals 32px.  1.2em equals 19.2px.  0.8em equals 12.8px.

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

Much better.  But it should be clear now that ems can get away from us if we're not careful.  You are best off using ems

<!--- WORKING HERE -->

<!---
One other important unit is percent.  If a value is a horizontal value (such as `padding-left`) this means a certain perctage of the parent container's width.  If the value is a vertical value, it means a percentage of the parent container's height.  Finally, if you use percent to set a font size, it behaves much like ems; `100%` means equal to the parent container's font size.
-->

Pixels and Ems are two of the most common units in CSS, but there are many more.  See Appendix B for a comprehensive list.
