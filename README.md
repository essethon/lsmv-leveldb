# LevelDB code analysis

This is repository includes code analysis documents for [google/leveldb](https://github.com/google/leveldb).

## Table of Contents and Schedule

### LevelDB Code Analysis

#### 0. Demo (~ end of May 2020)

[C++ demos](demo/) for making use of leveldb library.

#### 1. Doxygen

I generated [an HTML doc](https://rawcdn.githack.com/essethon/leveldb-doc/master/doxygen/html/index.html) using [doxygen](https://www.doxygen.nl) to find the structure of the code. For example, the main interface lies in [`Classes -> Class List -> leveldb -> DB`](https://rawcdn.githack.com/essethon/leveldb-doc/master/doxygen/html/classleveldb_1_1_d_b.html) and we can see the 2 child classes of `leveldb::DB`, which are `leveldb::DPImpl` and `leveldb::ModelDB`.

#### 2. [Data Structures](DS.md)

This part includes class definitions used by the following contents.

#### 3. [Write and Read Operations](Read-and-Write.md) (~ Week 1, June 2020)

This part of doc will include 
- API for write and read
- Overview of data structure involved, i.e. batch, log, memtable, sstable
- The process of write and read
- Snapshot mechanism

The main interfaces to the DB are declared in `include/leveldb/db.h`. A list of all members of `DB` class is [here](https://rawcdn.githack.com/essethon/leveldb-doc/master/doxygen/html/classleveldb_1_1_d_b.html) (Generated by doxygen). Virtual functions declared in the main interfaces listed:

```C++
virtual Status Put (const WriteOptions &options, const Slice &key, const Slice &value)
virtual Status Delete (const WriteOptions &options, const Slice &key)
virtual Status Write (const WriteOptions &options, WriteBatch *updates)
virtual Get (const ReadOptions &options, const Slice &key, std::string *value)
virtual Iterator * 	NewIterator (const ReadOptions &options)
virtual const Snapshot * GetSnapshot ()
virtual void ReleaseSnapshot (const Snapshot *snapshot)
virtual bool GetProperty (const Slice &property, std::string *value)
virtual void GetApproximateSizes (const Range *range, int n, uint64_t *sizes)
virtual void CompactRange (const Slice *begin, const Slice *end)
```

#### 4. Implementation of in-memory table (~ Week 2, June 2020)

At present, the [in-memory table](https://github.com/google/leveldb/blob/master/db/memtable.h) of LevelDB is implemented as a [skip-list](https://github.com/google/leveldb/blob/master/db/skiplist.h). In this part I will look into the implementation of the skip-list.

#### 5. Sorted tables and manifest (~ Week 2, June 2020)

On-disk file structures and their implementations and file operations in the source code.

- Log files (*.log)
- Sorted tables (*.ldb)
- Manifest (like index of the on-disk tables)
- CURRENT: a simple text file that contains the name of the latest MANIFEST file.
- Info logs (LOG and LOG.old)
- Others (LOCK, *.dbtmp)

#### 6. Compactions (~ Week 3, June 2020)

#### 7. Bloom Filters and Other Utilities (~ Week 4, June 2020)

#### 8. Recovery (~ Week 4, June 2020)

### Multi-Version Algorithm Design