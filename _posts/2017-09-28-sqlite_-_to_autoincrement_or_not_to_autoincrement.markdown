---
layout: post
title:  "SQLite - To AUTOINCREMENT or Not to AUTOINCREMENT"
date:   2017-09-28 15:05:25 +0000
---



In a recent lab review we wondered if we should use AUTOINCREMENT when building our SQLite tables. So I decided to do some research and find the answer. 

Along the way I of course learned more than just a simple answer, but the simple answer – for those who want one is: don’t use it, you likely don’t need it.

```
sqlite> CREATE TABLE streets (
   ...> street_id INTEGER PRIMARY KEY AUTOINCREMENT, ***<= You don't need AUTOINCREMENT***
   ...> street_name TEXT);

```

SQLite automatically increments the id for you as you add rows to your table. 

It turns out that SQLite implements the new table with a field called ROWID and that is where your PRIMARY KEY goes. Further SQLite automatically starts ROWID with the number 1 and increments each subsequent row by 1 unless you explicitly set the ROWID number.

Inserting 2 streets into the table:
```
sqlite> INSERT INTO streets (street_name) VALUES ('Main Street'), ('Elm Street');
```


Select using rowid:

```
sqlite> SELECT rowid, street_name FROM streets;

1|Main Street  ***<- ROWID set to 1***
2|Elm Street  ***<- ROWID incremented by 1***
```

Select using street_id:

```
sqlite> SELECT street_id, street_name FROM streets;

1|Main Street  ***<-ROWID = streetid***
2|Elm Street
```

OK, now we know:

* ROWID automatically gets assigned a number (beginning at 1)

* it automatically increments as rows are inserted into the table
 
* you can SELECT it using your PRIMARY KEY or ROWID. 

So why would we ever need to use AUTOINCREMENT?

You shouldn’t need it! Why? 

An SQLite table can hold 9223372036854775807 rows. If you reach this limit SQLite looks for an unused ROWID (maybe one from a row that was deleted) and assigns that number. 

If you use AUTOINCREMENT and reach the limit you get an error instead. AUTOINCREMENT also uses more computing resources.

So you should only use it if you need to keep SQLite from using a previously deleted ROWID when it reaches the maximum number of rows. That is going to be rare.

