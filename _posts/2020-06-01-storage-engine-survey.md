---
layout: post
title: PostgreSQL Storage Engine Survey
---
## PostgreSQL and RocksDB Overview
System Properties Comparison PostgreSQL vs. RocksDB

|           Name          |                          PostgreSQL                         |                                    RocksDB                                     |
|-------------------------|---------------------------------------------------------------|----------------------------------------------------------------------------------|
| Description             | Widely used open source RDBMS                                 | Embeddable persistent key-value store optimized for fast storage (flash and RAM) |
| Primary database model  | Relational DBMS                                               | Key-value store                                                                  |
| Implementation language | C                                                             | C++                                                                              |
| Data schema             | yes                                                           | schema-free                                                                      |
| Typing                  | yes                                                           | no                                                                               |
| Secondary indexes        | yes                                                           | no                                                                               |
| SQL                     | yes                                                           | no                                                                               |
| Server-side scripts     | UDF                                                           | no                                                                               |
| Triggers                | yes                                                           | no                                                                               |
| Partitioning             | partitioning by range, list and (since PostgreSQL 11) by hash | none                                                                             |
| Replication methods     | Master-slave replication                                      | none                                                                             |
| Foreign keys            | yes                                                           | no                                                                               |
| Transaction concepts    | ACID                                                          | yes                                                                              |
| Concurrency             | yes                                                           | yes                                                                              |
| Durability              | yes                                                           | yes                                                                              |
| In-memory capabilities  | no                                                            | yes                                                                              |

## RocksDB 
### Pros
#### Performance
* Multithread compaction
* Multithread memtable inserts
* Reduced DB mutex holding
* Optimized level-based compaction style and universal compaction style
* Prefix bloom filter
* Memtable bloom filter
* Single bloom filter covering the whole SST file
* Write lock optimization
* Improved Iter::Prev() performance
* Fewer comparator calls during SkipList searches
* Allocate memtable memory using huge page

#### Features
* Column Families
* Transactions and WriteBatchWithIndex
* Backup and Checkpoints
* Merge Operators
* Compaction Filters
* RocksDB Java
* Manual Compactions Run in Parallel with Automatic Compactions
* Persistent Cache
* Bulk loading
* Forward Iterators/ Tailing iterator
* Single delete
* Delete files in range
* Pin iterator key/value

#### Alternative Data Structures And Formats
* Plain Table format for memory-only use cases
* Vector-based and hash-based memtable format
* Clock-based cache (coming soon)
* Pluggable information log
* Annotate transaction log write with blob (for replication)

#### Tunability
* Rate limiting
* Tunable Slowdown and Stop threshold
* Option to keep all files open
* Option to keep all index and bloom filter blocks in block cache
* Multiple WAL recovery modes
* Fadvise hints for readahead and to avoid caching in OS page cache
* Option to pin indexes and bloom filters of L0 files in memory
* More Compression Types: zlib, lz4, zstd
* Compression Dictionary
* Checksum Type: xxhash
* Different level size multiplier and compression type for each level.

#### Manageability
* Statistics
* Thread-local profiling
* More commands in command-line tools
* User-defined table properties
* Event listeners
* More DB Properties
* Dynamic option changes
* Get options from a string or map
* Persistent options to option files

### Cons
* Merging process were manageable hardly, leaded performance unstable and writing amplification
* Reading and writing needed to access Memtable mutually, R / W mixture mode using multiple threads concurrency had bottleneck
* WALs were shared among column families which required single-thread writing mode and limited high-speed devices 

## PostgreSQL 
### Pros
* Transaction management 
* SQL support
* Data importing speed
* Geographic data type, multi-dimension index

### Cons
* Code complexity and maintain effort
* Cannot be compiled as static library and need to compile on sepecific OS versions

### Postgres based storage engine draft 
* Defined communication specification between client and server based on pg_protocol, get rid of gRPC
* Postgres background process extension
* Data import and export
* Data encryption



## Reference
[System Properties Comparison PostgreSQL vs. RocksDB](https://db-engines.com/en/system/PostgreSQL%3BRocksDB) <br />
[In-Memory Microbenchmark](http://www.lmdb.tech/bench/inmem/) <br />
[MyRocks: A space- and write-optimized MySQL database](https://engineering.fb.com/core-data/myrocks-a-space-and-write-optimized-mysql-database/) <br />
[Benchmarking InnoDB and MyRocks Performance using Sysbench](https://minervadb.com/index.php/2018/06/01/benchmarking-innodb-and-myrocks-performance-using-sysbench/) <br />
[Pushdown computations in PolyBase](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/polybase-pushdown-computation?view=sql-server-ver15) <br />
[The Internals of PostgreSQL - Process and Memory Architecture](http://www.interdb.jp/pg/pgsql02.html) <br />
[RocksDB introduction:RocksDB use cases and features (Chinese)](https://www.jianshu.com/p/3302be5542c7) <br />
[RocksDB Pros and Cons in application scenarios (Chinese)](https://www.jianshu.com/p/73fa1d4e4273) <br />

