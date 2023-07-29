%%
up:: [[Linux]]
tags:: #moc 
%%
# NixOS

**NixOS** is a *declarative* Linux system. 

Most operating systems are *imperative*, which means they are constructed by installing software using commands. The user and/or administrator interacts with all elements of the system.

A somewhat different approach uses *immutable* operating systems, an approach taken by [[Silverblue]] and [[Kinoite]]. In these cases the user does not interact with the base system but installs into containers all software not already included on the system.

A *declarative* operating system is also immutable, but in addition it is *reproducible*.  A configuration file describes all elements of the system such as users, software, services, etc. NixOS then builds the system based on the config file and swaps in the new system without updates.

I installed it on my [[Raspberry Pi]] and am really impressed so far.

## Logs

```dataview
table without id
date as Date, notes as Note, related as Related, file.etags as Tags, file.name as Reference
from #log & -"Extras/Templates"
where topic = [[NixOS]]
sort date desc
```
