## Task Management Basic
### Tasks Plugin

## Multi-table layout documentation



# Usage:

You create a multi-column region by defining the start, settings, column-end, and end tags. EG:

  

Text displayed above.

```start-multi-column  
ID: ExampleRegion1  
number of columns: 2  
largest column: left  
```

Text displayed in column 1.

--- end-column ---

Text displayed in column 2.

--- end-multi-column

Text displayed below.

**Rendered as:**  
![Example_1](https://raw.githubusercontent.com/ckRobinson/multi-column-markdown/master/images/Example_1.png?raw=true)

---

  

# Syntax Reference

### **Region Start Tag:**

Each multi-column region must start with either:
> ```start-multi-column  
> ID: A_unique_region_ID  
> ```

or


```
> ```multi-column-start  
> ID: A_unique_region_ID_1  
> ```
```

or

```
> ::::: {.columns id=A_unique_region_ID_2}  
> _(See more about Pandoc's fenced divs syntax below.)_
```

After defining the start tag you must declare an ID for the region. The ID is used to differentiate between different regions if there are multiple in the same document.

Each ID must be unique within the same document or unexpected render issues may occur. An ID may be used across multiple documents so that, for example, you can use the ID "dailynote" in the template used for your Periodic Notes.

By using the "Insert Multi-Column Region" command (more below) the start ID will be pre-set as a randomly generated 4 character string.

You can also use the "Fix Missing IDs" command which will search the currently open document and append random IDs to all regions that are missing one.

  

### **Valid Syntax Tags:**

  

Each tag type can be defined with the following options:

#### **Start Multi-Column Region:**

> ```start-multi-column  
> ID: A_unique_region_ID  
> _Any Additional Setting flags (see below)_  
> ```

> ```multi-column-start  
> ID: A_unique_region_ID_2  
> _Any Additional Setting flags (see below)_  
> ```

> ::::: {.columns id=A_unique_region_ID_2 _Any Additional Setting flags (see below)_}

  

#### **End a Column:**

--- column-end ---  
--- end-column ---  
--- column-break ---  
--- break-column ---\

> ::: columnbreak  
> :::  
> _(New line after columnbreak required.)_

  

#### **End Multi-Column Region:**

--- end-multi-column  
--- multi-column-end\

> :::  
> _(This end region syntax is only valid when using the Pandoc fenced divs syntax to start a region.)_

  

### **Pandoc Fenced Divs Support**

You can also use Pandoc's fenced divs syntax to define column regions. (For more detail on this syntax see [here](https://pandoc.org/MANUAL.html#divs-and-spans) and [here](https://github.com/dialoa/columns/blob/master/README.md).)

To create a multicolumn region use:
```
> ::: columns
> 
> <Column Content>
> 
> :::
```

To define multiple Pandoc regions on the same document, and to define region settings you must use the attributes syntax:

> ::::: {.columns property=value id=ID_ExampleID}
> 
> <Column Content>
> 
> :::::

Not providing an ID will cause regions to not render.

All other settings can be defined within the attributes using the same setting flag names defined below.

##### **What is supported with this syntax:**

- Basic fenced divs column definition: '::: columns' or '::: {.columns}'
- Specifying the number of columns with english words up to ten: '::: twocolumns', '::: {.three-columns}', etc.
- Specifying the number of columns through attributes: '::: {.columns col-count=3}'
- Specifying column gap through attributes: '::: {.columns columngap=3em}'
- Specifying column breaks through column break div: :::: columnbreak\n::::

##### **What is not supported:**

- Recusive Column Regions. Recusive regions are not supported in Core MCM so will not render the same as an exported Pandoc PDF.
- Spanning element. Elements that break up a column region to span across the view are not supported. You must manually end the region and start a new one.
- Specifying 'column rule', as there is currently no way to define this with other syntax.
- Justified or ragged column mode.
- "Fluid Columns" by default. The fluid columns default of Pandoc's syntax is equivalent to MCM's Auto Layout. However auto layout has significant perforamce overhead in Live preview and due to this Pandoc syntax will not automatically flag regions to auto layout. You can however manually flag them by adding the setting to the attributes: ::: {.three-columns fluid-columns=true} or ::: {.three-columns auto-layout=true}

  

## **Region Settings:**

#### **ID:**

- **Setting Flags**:
    - ID:
- **Valid Selections**:
    - Any string of characters.

_Example:_

> ```start-multi-column  
> ID: Random_ID_String  
> ```

- The ID is used to differentiate between different regions if there are multiple in the same document.
- Each ID must be unique within the same document or unexpected render issues may occur. An ID may be used across multiple documents so that, for example, you can use the ID "dailynote" in the template used for your Periodic Notes.
- Can be ommitted if there will only ever be a single column region in the document.

  

#### **Number of Columns:**

- **Setting Flags**:
    - Number of Columns:
    - Num of Cols:
    - Col Count:
- **Valid Selections**:
    - Any digit.

_Example:_

> ```start-multi-column  
> Number of Columns: 2  
> ```

  

#### **Column Size:**

By default all of the columns will be set to equal size if this option is omitted.  
  
_Can define on a per column basis with array syntax: EG: [25%, 75%]_

- **Setting Flags**:
    - Column Size:
    - Col Size:
    - Column Width:
    - Col Width:
    - Largest Column:
- **Valid Selections**:
    - Single Column layout:
        - Small
        - Medium
        - Large
        - Full
    - For either 2 or 3 column layouts
        - Standard
        - Left
        - First
        - Right
        - Second
    - Only for 3 column layouts
        - Center
        - Third
        - Middle
    - Allows _most_ CSS unit lengths (px, pt, %, etc).

_Example:_

> ```start-multi-column  
> Column Size: standard  
> ( **OR** )  
> Column Size: [25%, 75%]  
> ```

  

#### **Border:**

By default the border is enabled but can be removed with:  
  
_Can define on a per column basis with array syntax: EG: [off, on, off]_

- **Setting Flags**:
    - Border:
- **Valid Selections**:
    - disabled
    - off
    - false

_Example:_

> ```start-multi-column  
> Border: disabled  
> ( **OR** )  
> Border: [off, on]  
> ```

  

#### **Shadow:**

On by default, can be removed with:

- **Setting Flags**:
    - Shadow:
- **Valid Selections**:
    - disabled
    - off
    - false

_Example:_

> ```start-multi-column  
> Shadow: off  
> ```

  

#### **Column Position or Column Location:**

Only used with the single column option.

- **Setting Flags**:
    - Column Position:
    - Col Position:
    - Column Location:
    - Col Location:
- **Valid Selections**:
    - Left
    - Right
    - Center
    - Middle

_Example:_

> ```start-multi-column  
> Number of Columns: 1  
> Column Position: Left  
> ```

  

#### **Column Spacing:**

Used to set the distance between all of the columns.  
  
_Can define on a per column basis with array syntax: EG: [5px, 10px]_

- **Setting Flags**:
    - Column Spacing:
- **Valid Selections**:
    - Allows _most_ CSS unit lengths (px, pt, %, etc).
    - A number alone without a defined unit type defaults to pt unit.

_Example:_

> ```start-multi-column  
> Column Spacing: 5px  
> ( **OR** )  
> Column Spacing: [5px, 10px]  
> ```

  

#### **Content Overflow:**

Should the columns cut off anything that goes outside the column bounds or should it be scrollable.  
  
_Can define on a per column basis with array syntax: EG: [Scroll, Hidden]_

- **Setting Flags**:
    - Overflow:
    - Content Overflow:
- **Valid Selections**:
    - Scroll (Default)
    - Hidden

_Example:_

> ```start-multi-column  
> Overflow: Hidden  
> ( **OR** )  
> Overflow: [Scroll, Hidden]  
> ```

  

#### **Alignment:**

Choose the alignment of the content in the columns.  
  
_Can define on a per column basis with array syntax: EG: [Left, Center]_

- **Setting Flags**:
    - Alignment:
    - Content Alignment:
    - Content align:
    - Text Align:
- **Valid Selections**:
    - Left (Default)
    - Center
    - Right

_Example:_

> ```start-multi-column  
> Alignment: Center  
> ( **OR** )  
> Alignment: [Left, Center]  
> ```

  

#### **Auto Layout**

- **Setting Flags**:
    - Auto Layout:
    - Fluid Columns:
    - Fluid Cols:
- **Valid Selections**:
    - true
    - on

_Example:_

> ```start-multi-column  
> Auto Layout: on  
> ```

##   

Auto layout regions do not use defined column breaks. Instead these type of multi-column regions will attempt to balance all content equally between the columns. Headings are also attempted to be preserved so that a heading block will not end a column but will instead be moved to the top of the next column with it's corresponding content.

To use this feature set "Auto Layout: true" within the region settings.

  

---

## Full Document Multi-Column Reflow

---

Documents can be set to fully reflow into multiple columns while in Reading mode.

#### **Syntax**

To enable document reflow use Obsdian's frontmatter to provide the metadata for the file with the following syntax:

EG:

```
---
Multi-Column Markdown:
  - Number of columns: 3
  - Alignment: [Left, Center, Left]
  - Border: off
---

First line of document.
```

All settings must be a list underneath the Multi-Column Markdown tag. If obsidian does not parse a valid syntax it will not render. You can use the "Setup Multi-Column Reflow" command to ensure proper syntax.

**Features:**

- Reflow automatically detects your document view size and sets the column heights to match, reducing the number of times you need to scroll through the document.
    - Auto column height is overridable by defining the col-height in frontmatter settings using standard MCM syntax.
    - Changes to the view size currently require a document reload to update layout.
- User definable column breaks using default Multi-Column Markdown column break syntax.

**Additional Notes:**

- Just as with core MCM, the default Obsidian theme, all basic markdown syntax and rendered elements should be fully supported. However cross compatibility with other plugins, embeds, and themes are not guarenteed.
- All manually set multi-column regions are overridden by the document reflow.

**Known Issues:**

- Changes to the document may require a file reload to properly update.
- Export to PDF is currently not supported.
- Long paragraphs of text will not be split across columns, as they are rendered as a single chunks of content by Obsidian.

---

## Plugin Cross Compatibility.

---

Not all plugins will be cross compatable within a multi-column region. Depending on the implementation or use case, some plugins will either entierly not render, render incorrectly, or render but be uninteractable. For the most part, due to how Obsidian plugins work, there is little that can be done at this stage to guarentee cross compatibility. And this is even more the case when using Live Preview. You can check the [Cross Compatibility](app://obsidian.md/documentation/CrossCompatibility.md) sheet for plugins known to work within columns. Anything not on that list has not been tested.

---

## Obsidian Theming

---

Just as with cross compatibilty above, multi-column regions may be effected by the Obsidian Theme you are running. There is very little non-layout dependent CSS within MCM but some themes may add or remove elements neccessary to properly render the columns. If regions do not render properly in a specific theme, feel free to open an issue and make sure to include what Obsidian theme you are running when describing the problem.

  

# **Full Mutli-Column Examples:**

[Here](app://obsidian.md/documentation/FullExamples.md)

  
  

# **Plugin Commands:**

You can access the command pallet with ctrl/command - P.

#### **Insert Multi-Column Region**

Will create a two column region where the cursor is located with a randomly generated ID and a default settings block created. Anything currently selected will be moved outside the end of the inserted region as to not overwrite any data.

  

#### **Fix Missing IDs For Multi-Column Regions**

Will search the current document for any region start tags that are missing IDs and randomly generate new IDs for each region.

  

#### **Toggle Mobile Rendering - Multi-Column Markdown**

Enables or disables column rendering on mobile devices only.

  

#### **Setup Multi-Column Reflow**

Adds the default multi-column reflow tags and settings to the document frontmatter. Will not overwrite if already defined.


---
up:: [[ObsPKMEssentials]]
tags:: #on/Obsidian #on/PKM  #note/product #effort/ObsPKMClass 
prev:: 
next:: 