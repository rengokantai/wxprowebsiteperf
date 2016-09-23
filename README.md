## wxprowebsiteperf
###Chapter 8 Tuning MySQL
####LOOKING INSIDE
Storage engines are a separate logical entity and are modular in nature.  
Note: Many MySQL init scripts offer a reload option, which, in turn, runs mysqladmin reload.  
However, this reloads only the grants table. Be sure to perform a full stop and start after making changes to my.cnf.  

set temp:
```
SET key=value;
```
Global variables set in this way are reset upon a server restart.  
####UNDERSTANDING
#####MyISAM
(Until 5.5, default engine). MyISAM is faster than InnoDB.(particularly with SELECT queries)  
The main disadvantage of MyISAM from a performance point of view is that it uses table-level locking for write queries.  
(That is, if a DELETE, INSERT, or UPDATE query is being executed, the whole table is locked for the duration of the query. 
During this time, all other queries on the table (including SELECTs) must wait.)

slow-running SELECT statements can also affect performance because 
any write queries need to wait for these to finish before they can acquire a write lock.  


The MyISAM engine also enables tables to be packed using the myisampack tool.  
Packed tables are compressed on a row-by-row basis and are typically half the size of uncompressed MyISAM tables.  but such tables are read-only.
Another popular reason for using MyISAM is its support for full-text searching — a feature not offered by any of the other stock storage engines. 


#####InnoDB
InnoDB supports Transactions(MyISAM doesnot)  
slower lookups than MyISAM, but InnoDB has always offered particularly fast lookups on primary keys.  
InnoDB implements row-level locking.  

#####MEMORY
It stores the entire table in memory. 
The maximum size of MEMORY tables is limited by the max_heap_table_size variable, which defaults to 16 MB.  
The MEMORY engine is the only major storage engine to offer the choice of B-tree or hashing for indexes — MyISAM and InnoDB both use B-tree only.  
B-trees are usually a better choice for large indexes (which won’t fit fully into RAM). Hash tables are faster but only for smaller indexes (they don’t scale as well as B-tree)  


####TUNING MYSQL
[reference](https://dev.mysql.com/doc/refman/5.7/en/server-system-variables.html)
#####Table Cache
######table_open_cache
To avoid locking problems when multiple clients simultaneously attempt to access the same table, MySQL actually keeps a separate copy of the table open for each client accessing it.  
most desirable size for the table cache is proportional to the ```max_connections```
