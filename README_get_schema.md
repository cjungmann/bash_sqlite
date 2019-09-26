# SQLite With BASH : get_schema

[top](README.md)

This is the first SQLite BASH script I wrote in this project.
I wanted to look at all the cookies to see what patterns I
could discern.  In particular, I wanted to see how websites
save authorization on a machine so a user doesn't have to
signin each time a protected website is visited.

## Usage Suggestion

The initial use of this utility was to peruse my Chrome
browser cookies.  I performed the following steps:

- Found the path the the SQLite database in which the cookies are found:\
  `~/.config/chromium/Default/Cookies`\
  on my computer (it might be different on different systems).

- Saved the path to a environment variable to save retyping:\
  `CCookiePath='~/.config/chromium/Default/Cookies'`

- Confirmed tables residing in the database:\
  `sqlite3 "${CCookiePath}" ".tables"`

- Ran this script to see the column names:\
  `./get_schema ~/.config/chromium/Default/Cookies Cookies`

- Looked at the table fields, then constructed a query to show
  the table contents.  This is the query I used:\
  `sqlite3 "${CCookiePath}" "SELECT host_key, name, value, path FROM Cookies"`

## Script Notes

- Parsing the CREATE TABLE statement

  - extract the parameters list from the statement using the
    **cut** command, then using BASH *Parameter Expansion*
    (use command `man bash | less +'/\{parameter:offset'`)
    to trim extra characters to the left and right.
  
  - split parameters list into an array using IFS=','.

  - walk through array to reconstruct lines in which commas
    where included (based on lines that begin with a space).

- Fragile for complicated tables.

  - Based on the [create table syntax diagram](https://www.sqlite.org/syntaxdiagrams.html#create-table-stmt),
    I have not handled conditions that follow the field list,
    specifically *WITHOUT ROWID* or *AS Sql statement*.


