---
up: [[ObsPKMEssentials]]
tags: on/Obsidian, on/PKM, note/product, effort/ObsPKMClass, note/reference
aliases: 
---
#### Dataview Queries


Queries are statements which take the following form:

Fields can be ^cb6d89
- File names
- Folders
- Tags
- Links
- Other metadata you have defined

Fields can be:
- combined with `AND` and  `OR`
- aliased with `AS`
- multiple fields are separated with a comma

|                              |                     | Possible Values                                                                                       |
| ---------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------- |
| What are you looking for     | First part of query | List, Table, Task, Calendar                                                                           |
| Where are you looking        | FROM clause         | Any field or fields                                                                         | 
| Conditions you want to match | WHERE               | Use =, <=, >= to apply conditions to fields. Without an operator it checks for existence of the field |
| Order you want               | SORT                | asc, desc                                                                                             |
| Maximum number of results    | LIMIT               | Any number                                                                                            |

#### Examples

To list all files tagged #on/Obsidian 

> [!example]- "list from #on/Obsidian "
> ```dataview
> list from #on/Obsidian 
> ```

The same, but exclude daily notes (notes with #note/daily tags)

> [!example]- "list from #on/Obsidian and -#note/daily"
> ```dataview
> list from #on/Obsidian and -#note/daily
> ```

You can show one other field in a LIST

> [!example]- "list tags from #effort/ObsPKMClass  and -#note/daily"
> ```dataview
> list tags from #effort/ObsPKMClass  and -#note/daily
> ```

But then you might as well use a TABLE, which looks nicer

> [!example]- "table tags from #effort/ObsPKMClass  and -#note/daily"
> ```dataview
> table tags from #effort/ObsPKMClass  and -#note/daily
> ```

Now we can add more fields. Let's show the date each file was created.

> [!example]- "table tags, file.ctime from #effort/ObsPKMClass  and -#note/daily"
> ```dataview
> table tags, file.ctime from #effort/ObsPKMClass  and -#note/daily
> ```

To make it nicer, let's change the column headings


> [!example]- "table tags as Tags, file.ctime as Created from #effort/ObsPKMClass  and -#note/daily"
> ```dataview
> table tags as Tags, file.ctime as Created from #effort/ObsPKMClass  and -#note/daily
> ```

Lets sort by creation date.

> [!example]- "table tags, file.ctime from #effort/ObsPKMClass  and -#note/daily sort file.ctime"
> ```dataview
> table tags, file.ctime from #effort/ObsPKMClass  and -#note/daily sort file.ctime
> ```

