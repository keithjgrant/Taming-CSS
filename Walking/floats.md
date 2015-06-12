

Before we start building any fancy layouts with floats, we need to understand what they are, and exactly how they behave.  Countless developers have seen that floats are a common tool for laying out webpages, and started using them, only to find themselves helpless down the road when things don't behave like they expect.
---
Notice that once the text reaches the bottom of the floated element, the document flow extends once again to the edge of its container.  Sometimes we design with shorter content and do not see this behavior until we add more content or work with a smaller browser window.  We need to be aware of it, especially when floating to the left, or we can wind up with oddly-placed words that can be distruptive to the reader:

<img src="images/figure6-1.png"/>

This is probably not the behavior we want, especially if we are trying to make our element its own column with the main content in another column of its own.  We can fix this by making each column
---
Even though the flow of the text is altered, this is not actually changing the shape of the container.  Let's put a floated element inside a container and use some background colors to observe exactly what is happening.

```html
<div class="container">
  <div class="floated">floated element</div>
  Contents of the container.
</div>
```
```css
.container {
  background-color
}

<!---
stacking floats horizontally
splitting floats in both direction
extending into another block-level
clearing
-->
