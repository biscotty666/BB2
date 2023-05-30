---
up: [[ObsPKMEssentials]]
tags: on/Obsidian, on/PKM, note/product, effort/ObsPKMClass, note/reference
aliases: 
---

##### Daily Notes

Daily notes provide a central location to

- Organize your daily work and other activities
- Reflect on your day and your work 
- Set goals and tasks
- Document your work and make log entries 

##### Obsidian setup

1. First we need to create 4 folders.
	- Extras/Templates/Daily Note
	- Calendar/Notes/Daily Note
2. In Obsidian install _and enable_ the Calendar plugin
3. In the Settings for Daily Note add the newly created directories where indicated

##### A basic daily note

1. Let's start with a simple note. Later we can add task management and logging functions. For fun I threw in a little JavaScript to embed the prior day's note. (Yes you can use JavaScript in notes!)
```
dates:: {{date}}
tags:: #log/journal #note/daily 
---
## {{title}}

### Today's Goals

### Today's Progress

### Reflections

---
## Yesterday's Note

<%*
const yesterday = tp.date.yesterday("YYYY-MM-DD")
tR += "[[" + [[yesterday]] + "]]"
%>

```

2. Name the file "Daily Note" and place it in Extras/Templates.
3. Open the right sidebar and click the calendar icon.
4. Click on a day to create a note for that day.
5. The note will be created and the template automatically applied.
6. Use Ctrl-R to replace the JavaScript with yesterday's note.

---
prev:: 
next:: 