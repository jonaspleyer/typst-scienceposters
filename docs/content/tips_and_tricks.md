+++
title = "Tips and Tricks"
+++
# Tips and Tricks
<!-- TODO add screenshots of before and after -->
## Modify Spacing between Boxes
Unfortunately for technical reasons, setting the spacing between boxes generated by `typst-posters` has to be done manually with 3 distinct methods.
Suppose, we want to set the new spacing to `0.5em` from the [default value](https://typst.app/docs/reference/layout/block/#parameters-spacing) of `1.2em`.
So let's capture this in a variable.
```typst
#let box_spacing = 0.5em
```
First, we need to specify the spacing between [blocks](https://typst.app/docs/reference/layout/block/).
```typst
#set block(spacing: box_spacing)
```
Next, we also want to change the spacing between columns.
Thus we modify the `gutter` argument of the [`columns` function](https://typst.app/docs/reference/layout/columns/).
```typst
#set columns(gutter: box_spacing)
```
Last but not least, we need to tell `typst-posters` to use the new value when calculating how large boxes need to be in order to stretch them to the nearest endpoint.
```typst
#update_layout(
    spacing: box_spacing,
)
```

<!-- TODO add screenshots of before and after -->
## Deeply modify Heading and Body Boxes
In order to deeply modify heading and body boxes, we are able to change the function itself which draws the box.
By default, these boxes are simply rectangles which are drawn with the specified options.
We define a new function that takes the heading as a mandatory argument (or the body when changing the body funtion) and a range of arguments not more closely specified.
```typst
#let my_custom_heading(heading, ..args) = {
    ...
}
```
This function will now obtain all information that the default `rect` function would be given as well.
We can choose to ignore the optional `..args` or reuse them if desired.
Let's begin by simply making a smaller centered box.
```typst
#let my_custom_heading(heading, ..args) = {
    align(center, rect(heading, ..args, width: 100% - 2em))
}
```

Afterwards, we need to supply the new behaviour to `typst-posters` by setting the corresponding option in the [layout](/documentation/layout).
```typst
#update_layout(
    heading_box_function: my_custom_heading,
)
```