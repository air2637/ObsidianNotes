# Summary

This chapter covers how database works on 2 primary operations - reads, write data. 

Why as an application developer,  needs to care how DB handles storage and retrieval internally? It is because, we need to decide which DB is most fits for our use cases by knowing how it does under the hood. 

We will talk about relational databases, and also NoSQL databases. We will examine two families of storage engines:
- log-structured storage engines
- page-oriented storage engines

# Log file based simple DB design

We created a blank file called 'database', and introduce 2 sets of commands - to read, and to write. 
 - The read command keeps appending the key-value pair to a new line in the 'database' file
 - The write command read the lines according to the key provided and retrieve the last occurrence

```bash
#!/bin/bash

db_set() {
	echo "$1,$2" >> database
}

db_get() {
	grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}

# example of write
db_set 123456 '{"name":"London","attractions":["Big Ben","London Eye"]}'

# example of read
db_get 123456
```

This is the simplest but effective DB we have built on our own!
But there is some problems:
- The file keeps growing overtime, and make read operation slower overtime (The complexity is O(n) )!
- Miss of a good mechanism to handle concurrent operations
- Reclaiming disk space
- Handling errors
- Partially written records

# Hash indexes

Introduce an additional structure to aid the read operation - indexing!

> Every key is mapped to a byte offset in the data file - the location at which the value can be found.

Project the key of the key-value value to an offset on disk file. So when the key is queried, we only need 1 read on the disk to retrieve the data located as per the offset. 

It comes with a cost - each write operation needs to compute the hash before storing.

## Compaction

**Throw away duplicate keys in the log, and keep only the most recent update for each key. Merging segments means combining multiple data files into smaller ones, after the removal of duplicate keys. The merge and compaction of frozen segments can be done in a background thread. So we can still continue to serve read and write requests as normal, using the old segment files.**

## How hashing helps

Each segment now has its own in-memory hash table, mapping keys to file offsets. 

For a search of key, it first check the most recent segment's hash map (as it is the latest compaction outcome); if the key is not present we check the second-most-recent segment, and so on.

### File format

CSV is not the ideal format for a log. Binary form is faster and simpler, as it first encodes the length of a string in bytes, followed by the raw string without need for escaping.

### Deleting records

It append a deletion record to the log. During segment merging, the deletion flag (AKS a *tombstone*) tells the merging process to discard any previous values for the deleted key.

### Crash recovery

Restarting DB and recover the in-memory hash tables is painful, especially when the DB is really large. So a way to handle it is, **storing a snapshot of each segment's hashmap on disk, which can be loaded into memory more quickly** (but what if the DB system crashed before it can save the last snapshot? -> it leads to next feature : *partially written records*)

### Partially written records

In case DB crashed halfway through appending a record to the log segment. we can **include checksums, allowing such corrupted parts of the log to be detected and ignored**.

### Concurrency control

Writes are appended to the log in a **strictly sequential order**. Writing is only single thread, while the reading can be concurrently by multiple threads. **Data file segments are append-only and otherwise immutable.**

### Cost of hash table indexing

1. You have to ensure the memory space is sufficient to fit the hash table. 
2. Range queries are not efficient, given the nature of hash table storing the keys not in sequential order.

# SSTables and LSM-trees

## what is SSTable?

We make a simple change to the way we write segment files - when writing the key-value pairs into a segment file, we required they have to be written in a sorted order by the keys.  This becomes *Sorted String Table, or SSTable for short*.

## why we do this sorting on key for each segment file?

- It makes the search of key being naturally applicable of binary search in each segment file.
- It simplifies the merging segments operation: read the segment files side by side, write the lowest key and its associated value records from each file first, and repeat this till both sides are fully copied into the merged file. (*mergesort algorithm actually*)
- *You still need an in-memory index to tell you the offsets for some of the keys, but it can be sparse (meaning not necessarily has to be each key), one key for every few kilobytes of segment file is sufficient*. e.g. we want to get record with key 'b', and the in-memory index only knows, 'a' and 'c', well, we just need to find the offset directed between 'b' and 'c', as we know they are for sure written in order. This helps greatly in eliminating the constraint that imposed by ordering hashing approach.

## Constructing and maintaining SSTables

*How do you get your data to be sorted by key in the first place, as our incoming writes can occur in any order?*

Maintaining a B-tree memory space, witch which, you can insert keys in any order and read them back in sorted order!

A typical workflow:
1. When a write comes in, add it to an in-memory B-tree. AKA *memtable*.
2. When the memtable size reaches the threshold, dump it in sorted order onto SSTable file on disk
3. Read a record with key, will get tried firstly in memtable, then in most recent SSTable file, then the next older SSTable file, etc



# B-tree backed DB

This is the most common approach in constructing a DB. 

> each page can be identified using an address. (Just like the pointers we used in storing variables in memory)

> One page is designated as the root of the B-tree, contains several keys and references to child page. Each child is responsible for a continuous range of keys, and the keys between the references indicate where the boundaries between those range lies.

> Eventually, we get down to a page containing individual keys (a leaf page), which either contains the value or not.

> The number of references to lead to the deepest value, is called branching factor.

> If you want to update the value for an existing key in a B-tree, you search for the leaf page containing that key, change the value in that page, and write the page back to disk.

> If you want to add a new key, you need to find the page whose range **encompasses** the new key and add it to that page. In cases there is not enough free space in the leaf page to accommodate the new key, it is split into two half-full pages e.g. the leaf page contains some keys residing in range 1 ... 500, and now it needs to insert 265, it will be split into: 1 ... 265 , 284 ... 500. **This ensures the tree remans balanced**.

## Save a crashed B-tree DB

Write operation can sometimes be crashed. Especially B-tree DB does overwrites an existing page when updating an existing key value, or more complicated that *splits a page because an insertion caused it to be overfull, you need to write two pages that were split, and also overwrite their parent page to update the references to the two child page.*

**Additional data structure on disk - Write-ahead log (WAL, or call it redo log) , that is append-only file to which every B-tree modification must be written before it can be applied to the pages of the tree itself.** During restoring DB after its crash, this WAL is used to restore DB operations. 

A *latch* (lightweight lock) is used to protect the tree's DB in handling concurrency.


