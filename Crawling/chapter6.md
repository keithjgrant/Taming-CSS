# Taming CSS
# Chapter 6: The Box Model

Notice that once the text reaches the bottom of the floated element, the document flow extends once again to the edge of its container.  Sometimes we design with shorter content and do not see this behavior until we add more content or work with a smaller browser window.  We need to be aware of it, especially when floating to the left, or we can wind up with oddly-placed words that can be distruptive to the reader:

<img src="images/figure6-1.png"/>

This is probably not the behavior we want, especially if we are trying to make our element its own column with the main content in another column of its own.  We can fix this by making each column

<!--
width problem (w/ floats)
margin collapsing
overflow
negative margins
element size (based on children, etc.)
-->
