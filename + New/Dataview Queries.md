### Dataview Queries


__Note__:
- queries can be very simple or very complex
- query results are automatically updated 
- key words such as FROM and WHERE can be upper or lower case
- line breaks are optional
- fields can be:
	- combined with `AND` and  `OR`
	- aliased with `AS`
	- multiple fields can be separated with a comma
	- put a - before a field to negate inclusion (FROM clause)
- You can use JavaScript to write queries allowing for sophisticated manipulation with calculated fields

#### Query structure

##### Mandatory Part
This is the most basic query. For convenience of this I will go ahead and set a LIMIT for number of notes I want returned. The syntax and purpose is self-explanatory.

```
	```dataview
	TABLE (or list, task, calendar)
	LIMIT 3
	```
```
```dataview
TABLE
LIMIT 3
```

It will return every note in your vault (within LIMITs 😉).

You may want to see other information about your notes in the table. You can list those after TABLE.

```dataview
TABLE
file.tags, file.etags, tags
LIMIT 3
```
As you can see, you usually want file.etags, which is the same as just writing "tags".

In addition to fields you declare there is a lot of automatically generated (implicit) metadata for [[Dataview Implicit Fields#Pages|notes]], [[Dataview Implicit Fields#Lists and tasks|lists and tasks]].

You can change the table headers with "as"

```dataview
TABLE 
file.cday as "Created on"
LIMIT 3
```

##### FROM

FROM defines the "source", or where you want to look for information. (Don't confuse this with WHERE which we use to create conditions on the notes returned.)

The source can be
- tags
- filenames
- directories
- Any arbitrary [[Dataview Fields|fields]] you create

To list all files tagged #effort/ObsPKMClass

```
	```dataview
 	TABLE file.etags as Tags
 	FROM #effort/ObsPKMClass 
 	```
```
```dataview
TABLE file.etags as Tags
FROM #effort/ObsPKMClass 
LIMIT 3
```


For all files in a certain directory
```
	```dataview
	TABLE
	file.path as Path
	FROM "Spaces/ObsPKMClass"
	LIMIT 5
```
```dataview
TABLE
file.path as Path
FROM "Spaces/ObsPKMClass"
LIMIT 5
```

As you can see it looks in sub-directories as well. We could exclude a specific sub-directory like this:

```
	```dataview
	TABLE
	file.path as Path
	FROM "Spaces/ObsPKMClass" AND -"Spaces/ObsPKMClass/Chapters"
	LIMIT 5
```
```dataview
TABLE
file.path as Path
FROM "Spaces/ObsPKMClass" AND -"Spaces/ObsPKMClass/Chapters"
LIMIT 5
```

##### WHERE

Where allows you to apply conditions to return notes. 

To show all notes created in the last 4 days sorted by date, not including the daily notes:
```
	```dataview
	TABLE
	file.cday AS "Created on"
	FROM -#log/journal
	WHERE file.cday >= date(today) - dur(4 days)
	```
```
```dataview
TABLE
file.cday AS "Created on"
FROM -#log/journal
WHERE (file.cday >= date(today) - dur(4 days))
LIMIT 6
```

##### SORT

Takes a field name and a sort order, either `asc` or `desc`

To sort the above query by most recent
```
	```dataview
	TABLE
	file.cday AS "Created on"
	FROM -#log/journal
	WHERE file.cday >= date(today) - dur(4 days)
	SORT file.cday desc
	```
```
```dataview
TABLE
file.cday AS "Created on"
FROM -#log/journal
WHERE (file.cday >= date(today) - dur(4 days))
SORT file.cday desc
LIMIT 7
```

---
up:: [[Zettelkasten Implementation in Obsidian|Implementation]]
tags:: #note/product #effort/ObsPKMClass, #note/reference #on/Obsidian #on/PKM 
prev:: [[Dataview Fields]]
next:: 
