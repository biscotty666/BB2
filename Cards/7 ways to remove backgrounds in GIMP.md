## 7 ways to remove backgrounds in GIMP

1. For circular/rectangular objects, use the relevant select tools
	1. Make sure there is an alpha channel
	2. Invert selection and cut
	3. Select an area around the box, then layer/crop to selection
	4. Add a background image/layer
	5. Use the scale tool to resize and move tool to place object
		NB. Scaling is destructive so be sure to preserve the original 
		NB. I was having trouble with the move tool. Make sure that the proper Layer/Selection/Path is highlighted in the tool settings
2. __Fuzzy Select Tool__ when there is significant contrast between foreground and background
	1. NB. I couldn't get the Smudge tool to work. I needed to "Select None"
3. Path tool
4. For flat colors: __Select by color__
5. Foreground select
	1. Make an initial selection by clicking around areas to keep (hit enter after connecting)
	2. Paint to indicate foreground/background
6. Layer mask
	1. 

### Image

##### Full Size

> ```start-multi-column  
> ID: A_unique_region_ID  
> Number of Columns: 2
> ```


##### SVG

svg:: 

--- column-end ---  

##### PNG

png:: 

--- end-multi-column  

---
### Based on

[gimp move tool can't select object - Google Search](https://www.google.com/search?q=gimp+move+tool+can%27t+select+object)

---

up:: [[Gimp]]
tags:: #note/resource #image/logo #source/video #source/TheGimpTutorials #on/Graphics 
Status:: Watched
Priority:: 1