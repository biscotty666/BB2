%%
up:: [[Obsidian]]
tags:: #note/product #effort/article
Status:: In Progress
%%
# Logging in Obsidian

## Motivation
I like to keep track of the work I have done in certain types of activities. Up until recently I have done this rather simply by leveraging my daily note with a special setup for my exercise logs. I recently started using Todoist for task management and that has required me to rethink how I keep my logs. I would like to do all of this in Obsidian except the actual task management because Todoist is so nice to work with. The Todoist plugin is very good but still lacks some basic functionality.

More specifically I want to be able to:
- Add log information to a note
- Create arbitrary log entries 
- Display the day's logs on the Daily Note 
- Create views based on, eg., type of entry or topic

Here I will describe my solution. In the process I found an application for the little-used core "Unique Note" plugin.

## Implementation

### Tags

In order to easily work with dataview I'll use tags to indicate log entries. In my case I'll use the following tags:
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

In Obsidian's settings, 

The file looks like this:
```
# Log Entry

tags:: #log 
Date:: 2023-07-29
Topic:: 
Notes:: 
Related:: 
References:: 
```

`Topic` and `Related` contain a link or links to other notes (perhaps non-existing). `References` are for external resources. `Notes` is used for comments and other text to put in the log.



### Add Log Entries to Existing Notes

### The Daily Note

### Sample Topic Report

## Closing thoughts

It is so interesting to me how often working with Obsidian has led me to change the way I think about things and how I do things. Going through the process of developing this system has led me to reflect on my work patterns and where I can make them more efficient.

Brian Carey, Albuquerque, 2023
