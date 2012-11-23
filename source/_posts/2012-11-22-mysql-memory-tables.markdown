---
layout: post
title: "MySQL MEMORY Tables"
date: 2012-11-22 18:41
comments: true
categories:
- mysql
---
I've been doing a bit of learning about MySQL lately at working. I have a horrible memory and writing things down *after* I learn something has always helped me to remember it.

A memory table can be created in MySQL using the ENGINE table option:  
    CREATE TABLE t (i INT) ENGINE = MEMORY;

TYPE = HEAP is an older way of specifying the same thing and is still supported for backwards compatibility. Memory tables use hash indexes by default so they’re very fast, and useful for creating temporary tables. When the server shuts down, the tables lose all of their values since they’re stored in memory, but the table definitions aren’t lost since they’re stored in .frm files on disk. These tables will exist when the server restarts but will be empty.

- These tables can have up to 64 indexes per table, with 16 columns per index and a maximum key length of 3072 bytes.
- They cannot contain BLOB or TEXT columns.
- The maximum size of a MEMORY table is dependent on the `max_heap_table_size` system variable, which is by default 16MB. In order to create MEMORY tables of different sizes this variable’s value must be changed. It’s best to change the session value of this variable instead of the global value, unless you want the value to be changed for all clients.
- Memory is not reclaimed when individual rows are deleted from MEMORY tables. It’s only reclaimed when the entire table is deleted.