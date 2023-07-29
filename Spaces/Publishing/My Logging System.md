---
cssclass: "qs"
---
%%
up:: [[Obsidian]]
tags:: #note/product #effort/article
Status:: In Progress
%%
# Logging in Obsidian

## Motivation
I like to keep track of the work I have done in certain types of activities. Up until recently I have done this rather simply by leveraging my daily note with a special setup for my exercise logs. I recently started using Todoist for task management and that has required (inspired 😉) me to re-think how I keep my logs. I would like to do all of this in Obsidian with basic plugins except the actual task management. The Obsidian Todoist plugin is very good but still lacks some basic functionality.

More specifically I want to be able to:
- Add log information to a note
- Create arbitrary log entries 
- Display the day's logs on the Daily Note 
- Create views based on, eg., type of entry or topic

Here I will describe my solution. In the process I found an application for the little-used core _Unique Note Creator_ plugin. In addition I will use the *Dataview* and *Templater* plugins. I will not go into detail in relation to these plugins but you should be able to easily modify the examples I give.

## Implementation

I will use *tags* to indicate a log entry. I'll create template "sinippets" which can be inserted in existing documents to make a log entry related to that note. I will also create a template for a new "Log Note" with a guaranteed unique name for log entries not linked to a specific note. 

I will add _Dataview_ queries to my daily note to show the day's activities. Finally I will create a note which gathers all log entries for a specific log topic.

### Tags

In order to easily work with _Dataview_ I'll use tags to indicate log entries. In my case I'll use the following tags:
```
#log/study 
#log/project 
#log/infrastructure 
#log/exercise 
#log/piano
#log/domestic
```


### On-the-fly Log Entries 

Before setting up the generic log entry template we need to enable the "Unique Note" core plugin. Once enabled we need to define a directory and a template for the plug-in to use. In my file structure the appropriate directory is called `Extras` so I created a directory called `Extras/Logs`. I decided to keep the template itself here as well instead of with my other templates so I also created a note called `Extras/Logs/Log Entry`. 

In Obsidian's settings, toggle on the _Unique note creator_ plugin under _Core Plugins_.

![[Screenshot_20230729_121842b.jpeg]]

![[Screenshot_20230729_122215.jpeg]]

This plugin enables a new command, `Create new unique note`. I assigned this to a hotkey because it's __The Right Thing To Do__ 😉.

![[Screenshot_20230729_131915.jpeg]]

I created the file `Extras/Logs/Log Entry` which looks like this:
```
# Log Entry

tags:: #log 
Date:: 2023-07-29
Topic:: 
Notes:: 
Related:: 
References:: 
```

As you can see I have provided the generic tag #log as a default. This will be changed to the specific log tag when making the entry. Also note that I'm using *Templater* to provide the date automatically.

I plan to use these fields as follows:
- `Topic` will be a link to one or more Maps of Content. (Thanks to Obsidian these need not already exist to be useful.)
- `Related` may contain links to other notes or MOCs
- `References` are for external resources. These could be web links or book/article references
- `Notes` is for comments, descriptive information, and other text to put in the log

Now any time I want to make a log entry I can simply to `Ctrl-Alt-Sftd-L` and I have a new, uniquely named note in which I can make my log entry. The most common use-case for this type of note is to log certain tasks when I've completed them. (I use Todoist with the Obsidian plugin for task management.)

### Add Log Entries to Existing Notes

Sometimes I want to make a log entry in a note I am working with. For example, I'm learning how to do things in NixOS which I recently installed. They have great documentation, and I'll typically create a note from the page (with the _Surfing_ plug-in.) After working through the page and making new _atomic notes_ (from the information by `Extract current selection` and writing the information in my own words, but with the referred to text). After working through the page I might want to make an entry in my _Study Log_, and if it leads to some change on my system I might want an entry in my *Infrastructure Log*.

I need 2 templates for this:

```
%%
#log/infrastructure
Topic:: 
Note:: 
Date:: 2023-07-29
Related::  
%%
```

```
%%
#log/study
Topic:: 
Note:: 
Date:: 2023-07-29
Related::  
%%
```

Note that the use of `%%` prevents the fields from showing in *Read View*. Now to log relevant information from within the note I'm working on I just need to `Ctrl-T` to insert a template and I can again fill in the relevant information.

NB. Unfortunately a given note can only contain one log entry or the *Dataview* queries break since there would be multiple fields with the same name. I do not anticipate this as a problem for what I'm logging. Worst case would be just to generate a *Log Entry* note.

### The Daily Note

One use of these logs is to see my daily activity. To do this I have created a section in my daily note for logs.

![[Screenshot_20230729_135721.jpeg]]
![[Screenshot_20230729_135804.jpeg]]


### Sample Topic Report

Here is an example of a view of all my log entries related to NixOS

![[Screenshot_20230729_150225.jpeg]]
And this is the query:

```
table without id
date as Date, notes as Note, related as Related, file.etags as Tags, file.name as Reference
from #log & -"Extras/Templates"
where topic = [[NixOS]]
sort date desc
```


## Closing thoughts

It is so interesting to me how often working with Obsidian has led me to change the way I think about things and how I do things. Going through the process of developing this system has led me to reflect on my work patterns and where I can make them more efficient. 

> Brian Carey, Albuquerque, 2023