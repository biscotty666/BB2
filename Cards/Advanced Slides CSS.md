up:: [[Obsidian Advanced Slides]]
tags:: #on/Obsidian #source/documentation #note/reference 


## Advanced Slides CSS

### Apply css with

```
text with border <!-- element class="with-border" -->

text with background <!-- element style="background:blue" -->

text with attribute <!-- element data-toggle="modal" -->

```


### Or in style tags

```
<style>
	.with-border{
		border: 1px solid red;
	}
</style>

styled text <!-- element class="with-border" -->

```

### Slide annotations

```
<!-- .slide: style="background-color: coral;" -->

# Header with coral background color

Paragraph has coral background color, too!

---

<!-- .slide: style="background-color: green;" -->

- All Bullet points
- have green
- background color

```
