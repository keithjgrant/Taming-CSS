

Before we start building any fancy layouts with floats, we need to understand what they are, and exactly how they behave.  Countless developers have seen that floats are a common tool for laying out webpages, and started using them, only to find themselves helpless down the road when things don't behave like they expect.


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
