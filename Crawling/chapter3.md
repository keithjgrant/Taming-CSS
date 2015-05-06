# Taming CSS
# Chapter 3: Cascade, Specificity, & Inheritance


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



## Colors

```css
  background: #336699;
```

We are also familiar with the `background` attribute, but this value is something new.  This is a **hex color**, also known as hex notation.  "Hex" is short for "hexadecimal", which is a base-16 number system.

Unlike our common decimal number system, which is base-10 and uses the ten digits 0 through 9, hexidecimal uses sixteen digits.  We represent these with 0 through 9 as well as A through F.  "A" represents the decimal value "10".  "B" represents "11", et. cetera up through "F" which represents "15".  Capitalization is ignored.

If you were to add one to `F`, you get `10`.  Instead of a tens column, as with decimal numbers, we have a sixteens column.  `11` means 1 sixteen plus 1.  `2A` means 2 sixteens (decimal 32) plus A (decimal 10).  Don't worry too much about the conversion, though.  Suffice it to say, in hex, letters have higher values than numbers; they are kind of like the face cards in a deck of cards.

You can easily get by with a general grasp of the concept.  Then you know that `B9` is larger than `9B`, and `A1` is much larger than `1A`.  Most often, you will be obtaining these values from a image editor or color picker, not writing them by hand off the top of your head.

So how does this get us a color?  A CSS hex color is actually three distinct hexidecimal numbers t
ogether, representing values for red, green, and blue.  In `#336699`, `33` is the amount of red, `66` is the amount of green, and `99` is the amount of blue.  Since blue is the largest value, so this color is primarily blue.  Because both digits in each value are equal, this number can be abbreviated as `#369`.

Colors you will common use include `#ffffff` (or `#fff`), which is pure white, and `#000000` (or `#000`), which is pure black.  (We use additive color, which means the higher the value, the more light we are adding).
