# Taming CSS
# Chapter 3: Styling Essentials





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
