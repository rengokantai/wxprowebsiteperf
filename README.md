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
The MEMORY engine is the only major storage engine to offer the choice of B-tree or hashing for indexes — MyISAM and InnoDB both use B-tree only.  
