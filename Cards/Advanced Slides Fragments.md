up:: [[Obsidian Advanced Slides]]
tags:: #on/Obsidian #source/documentation #note/reference 


## Advanced Slides Fragments



### Fragments (transition effects)

```
Fade in <!-- element class="fragment" -->

Fade out <!-- element class="fragment fade-out" -->

Highlight red <!-- element class="fragment highlight-red" -->

Fade in, then out <!-- element class="fragment fade-in-then-out" -->

Slide up while fading in <!-- element class="fragment fade-up" -->

---

- Permanent item
- Appear Fourth <!-- element class="fragment" data-fragment-index="4" -->
- Appear Third <!-- element class="fragment" data-fragment-index="3" -->
- Appear Second <!-- element class="fragment" data-fragment-index="2" -->
- Appear First <!-- element class="fragment" data-fragment-index="1" -->

```

### Transitions

| Name | Effect |
| ---- | ------ |
| fade-out|Start visible, fade out|
|fade-up|Slide up while fading in|
|fade-down|Slide down while fading in|
|fade-left|Slide left while fading in|
|fade-right|Slide right while fading in|
|fade-in-then-out|Fades in, then out on the next step|
|current-visible|Fades in, then out on the next step|
|fade-in-then-semi-out|Fades in, then to 50% on the next step|
|grow|Scale up|
|semi-fade-out|Fade out to 50%|
|shrink|Scale down|
|strike|Strike through|
|highlight-red|Turn text red|
|highlight-green|Turn text green|
|highlight-blue|Turn text blue|
|highlight-current-red|Turn text red, then back to original on next step|
|highlight-current-green|Turn text green, then back to original on next step|
|highlight-current-blue|Turn text blue, then back to original on next step|

### Fragmented lists

```
# Unordered list

- First
- Second
- Third

---

# Fragmented unordered list

- Permanent
+ First
+ Second
+ Third

---

# Ordered list

1. First
2. Second
3. Third

---

# Fragmented ordered list

1. Permanent
2) Second
3) Third
4) Fourth

```
