up:: [[ObsPKMEssentials]]
tags:: #on/Obsidian, #on/PKM, #note/product, #effort/ObsPKMClass, #note/reference

##### Daily Notes

Daily notes provide a central location to

- Organize your daily work and other activities
- Reflect on your day and your work 
- Set goals and tasks
- Document your work and make log entries 

##### Obsidian setup

1. First create the folder "Calendar/Daily Notes.
2. In Obsidian install _and enable_ the Calendar plugin
3. In the Settings for "Daily Note" add
 	- Calendar/Daily Notes for New file location
 	- Extras/Templates/Daily Note for Template file location


##### Daily note template

1. Let's start with some frontmatter. Create a file Extras/Templates/Daily Note and add
```
---
date: {{date}}
tags: #log/journal #note/daily 
---
```

2. Then create a basic layout with headers
```
## {{title}}

### Today's Goals

### Today's Progress

### Reflections

### Logs

### Tasks

---
## Yesterday's Note

[[2023-06-15]]

```

2. Name the file "Daily Note" and place it in Extras/Templates.
3. Open the right sidebar and click the calendar icon.
4. Click on a day to create a note for that day.
5. The note will be created and the template automatically applied.
6. Use Ctrl-R to replace the JavaScript with yesterday's note.

---
prev:: 
next:: 