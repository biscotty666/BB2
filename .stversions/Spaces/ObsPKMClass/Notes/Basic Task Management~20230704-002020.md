---
cssclass: "qs"
---
## Basic Task Management

## Dates

Dates should be entered in the format `YYYY-MM-DD`. 

The tasks plugin uses icons to give meaning to dates which immediately follow the icon.

|Type|Icon|Queries|Note|
|---|---|---|---|
|Due Date|📅|`due`|Also matches `happens`|
|Scheduled Date|⏳|`scheduled`|Also matches `happens`|
|Start Date|🛫|`starts`|Also matches `happens`|
|Created Date|➕|`created`|Automatically added when using the modal|
|Done Date|✅|`done`|Automatically added when checked off|
|Recurring Date|🔁||Creates a new task when checked off|

#### Discrete dates

Absolute dates should be entered with the format "YYYY-MM-DD", eg. `2023-06-24`.

Relative dates are relative to today's date. Examples:

- `yesterday`
- `today`
- `tomorrow`
- `next monday`
- `last friday`
- `14 days ago`
- `in two weeks`
- `14 October` (the current year will be used)
- `May` (1st May in the current year will be used)

##### Examples

All tasks due today

```tasks
due before next Monday
filename includes Examples
```

#### Date ranges

Two absolute dates in succession define an inclusive range.

Relative date ranges are defined by  `this`, `next` or `last` followed by `week`, `month`, `quarter` or `year`.

Queries use `in`, `after`, `next` or `before`.

- Can use  `is recurring` or `is not recurring`


## Recurring tasks

- Start with the keyword `every`
- Add `when done` to make the date of the new recurring task based on the completion date and not the original due date

Examples:

🔁 every 3 days
🔁 every 10 days when done
🔁 every weekday (meaning every Mon - Fri)
🔁 every week on Sunday
🔁 every month on the last
🔁 every 6 months on the 2nd last Friday
🔁 every April and December on the 1st and 24th (meaning every April 1st and December 24th)

## Priorities

In order high to low

|Icon|Significance/Query|
|---|---|
|🔺|highest|
|⏫ |high|
|🔼|medium|
|no signifier|no priority|
|🔽|low|
|⏬️|lowest|

Example: `priority is high`. Can also use `above`, `below`, `not`.

## Statuses

We will only use the two basic statuses, checked or unchecked, indicated by `- [x]` and `- [ ]` and which correspond to `done` or `not done`

## Queries




### Description

`description includes`

### Multiple filters

Each new line describes an `AND` operator. All standard boolean operators are recognized, `NOT`, `AND`, `OR`, `AND NOT`, `OR NOT` and `XOR`. Related filters must be wrapped in parentheses.

## Tags

Existence: `has tags`, `no tags`
`tags include`, `tags do not include`

## File properties

`path includes`, `path does not include`
`folder includes`, `folder does not include`
`filename includes`, `filename does not include`

## Limit

`limit 10`
`limit groups 3` when using sorting, limit of tasks in each group

## Sorting and grouping

`sort by`, `sort by <something> reverse`
multiple (nested) sorting by listing different criteria on different lines

`group by`



---
## References



---
up:: [[ObsPKMEssentials]]
tags:: #on/Obsidian #on/PKM  #note/product #effort/ObsPKMClass 
prev:: 
next:: 
