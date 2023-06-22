
%%
Library:: 000
up:: [[Obsidian]]
tags:: #on/Obsidian #note/reference #source/documentation 
%%

## Date Time Formatting Tokens

Obsidian uses the tokens from moment.js.

#### Year, month, and day tokens

_Tokens are case-sensitive._

|Input|Example|Description|
|---|---|---|
|`YYYY`|`2014`|4 or 2 digit year. Note: Only 4 digit can be parsed on `strict` mode|
|`YY`|`14`|2 digit year|
|`Y`|`-25`|Year with any number of digits and sign|
|`Q`|`1..4`|Quarter of year. Sets month to first month in quarter.|
|`M MM`|`1..12`|Month number|
|`MMM MMMM`|`Jan..December`|Month name in locale set by `moment.locale()`|
|`D DD`|`1..31`|Day of month|
|`Do`|`1st..31st`|Day of month with ordinal|
|`DDD DDDD`|`1..365`|Day of year|
|`X`|`1410715640.579`|Unix timestamp|
|`x`|`1410715640579`|Unix ms timestamp|

#### Week year, week, and weekday tokens

For these, the lowercase tokens use the locale aware week start days, and the uppercase tokens use the [ISO week date](https://en.wikipedia.org/wiki/ISO_week_date) start days.

_Tokens are case-sensitive._

|Input|Example|Description|
|---|---|---|
|`gggg`|`2014`|Locale 4 digit week year|
|`gg`|`14`|Locale 2 digit week year|
|`w ww`|`1..53`|Locale week of year|
|`e`|`0..6`|Locale day of week|
|`ddd dddd`|`Mon...Sunday`|Day name in locale set by `moment.locale()`|
|`GGGG`|`2014`|ISO 4 digit week year|
|`GG`|`14`|ISO 2 digit week year|
|`W WW`|`1..53`|ISO week of year|
|`E`|`1..7`|ISO day of week|

#### Locale aware formats

Locale aware date and time formats are also available using `LT LTS L LL LLL LLLL`. They were added in version **2.2.1**, except `LTS` which was added **2.8.4**.

_Tokens are case-sensitive._

|Input|Example|Description|
|---|---|---|
|`L`|`09/04/1986`|Date (in local format)|
|`LL`|`September 4 1986`|Month name, day of month, year|
|`LLL`|`September 4 1986 8:30 PM`|Month name, day of month, year, time|
|`LLLL`|`Thursday, September 4 1986 8:30 PM`|Day of week, month name, day of month, year, time|
|`LT`|`8:30 PM`|Time (without seconds)|
|`LTS`|`8:30:00 PM`|Time (with seconds)|

#### Hour, minute, second, millisecond, and offset tokens

_Tokens are case-sensitive._

|Input|Example|Description|
|---|---|---|
|`H HH`|`0..23`|Hours (24 hour time)|
|`h hh`|`1..12`|Hours (12 hour time used with `a A`.)|
|`k kk`|`1..24`|Hours (24 hour time from 1 to 24)|
|`a A`|`am pm`|Post or ante meridiem (Note the one character `a p` are also considered valid)|
|`m mm`|`0..59`|Minutes|
|`s ss`|`0..59`|Seconds|
|`S SS SSS ... SSSSSSSSS`|`0..999999999`|Fractional seconds|
|`Z ZZ`|`+12:00`|Offset from UTC as `+-HH:mm`, `+-HHmm`, or `Z`|


Unless you specify a time zone offset, parsing a string will create a date in the current time zone.

```js
moment("2010-10-20 4:30",       "YYYY-MM-DD HH:mm");   // parsed as 4:30 local time
moment("2010-10-20 4:30 +0000", "YYYY-MM-DD HH:mm Z"); // parsed as 4:30 UTC
```

#### Era Year related tokens

_Tokens are case-sensitive._

|Input|Examples|Description|
|---|---|---|
|y .. yyyy|`5 +5 -500`|Years|
|yo|`5th 1st`|Ordinal Years|
|N|`AD`|Abbr Era name|
|NN|`AD`|Abbr Era name|
|NNN|`AD`|Abbr Era name|
|NNNN|`Anno Domini`|Full Era name|
|NNNNN|`AD`|Narrow Era name|

