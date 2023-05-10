up:: [[Obsidian Advanced Slides]]
tags:: #on/Obsidian #source/documentation #note/reference 


## Advanced Slides Grid


### Explicit placement

```
<grid drag="width height" drop="x y">
```

Defaults to percentage based. Can append `px` if desired.

x and y values can be negative. Negative values reference the right and bottom edge respectively.

drop accepts named values:
- center
- top, bottom
- left, right
- topleft, topright
- bottomleft, bottomright

- Arrange elements in columbs or rows:  `flow="col | row"`.
- Background: `bg="green"`
- Border: `<grid  drag="width height" drop="x y" border="width style color">
- Animation: `animate="type speed"`
- Opacity: `opacity="level"`
- Filter: `filter="effect()"`
- Rotation: `rotate="deg"`
- Padding: `pad="top right bottom left"`
- Alignment (Horizontal): `align="type"`
- Justify Content (space between and around children): `justify-content="type"`
- Fragments: `frag="1"`

Animation type values:
- fadeIn, fadeOut
- slideRightIn, slideLeftIn, sideRightOut, sideLeftOut
- slideUpin, slideDownIn, slideUpOut, slideDownOut
- scaleUp, scaleUpOut, scaleDown, scaleDownOut

Filter values:
- blur
- bright
- contrast
- grayscale
- hue
- invert
- saturate
- sepia
`
Type values for align:
- left
- right
- center (default)
- justify / block
- top
- bottom
- topleft
- topright
- bottomleft
- bottomright
- stretch

Justify content type values:
- start
- center
- space-between
- space-around
- space-evenly (default
- end
