## What is Hbase ?

HBase is an open-source, column-oriented distributed database system in a Hadoop environment. Initially, it was Google Big Table, afterward, it was re-named as HBase and is primarily written in Java.  Apache HBase is needed for real-time Big Data applications.

### HBase Unique Features

1. HBase is built for low latency operations
2. HBase is used extensively for random read and write operations
3. HBase stores a large amount of data in terms of tables
4. Provides linear and modular scalability over cluster environment
5. Strictly consistent to read and write operations
6. Automatic and configurable sharding of tables
7. Automatic failover supports between Region Servers
8. Convenient base classes for backing Hadoop MapReduce jobs in HBase tables
9. Easy to use Java API for client access
10. Block cache and Bloom Filters for real-time queries
11. Query predicate pushes down via server-side filters.

### Hbase Architecture

HBase is a column-oriented database and data is stored in tables. The tables are sorted by RowId.

#### Storage Mechanism in Hbase

Coming to HBase the following are the key terms representing table schema

**Table:** Collection of rows present.

**Row:** Collection of column families.

**Column Family:** Collection of columns.

**Column:** Collection of key-value pairs.

**Namespace:** Logical grouping of tables.

**Cell:** A {row, column, version} tuple exactly specifies a cell definition in HBase.

Each table must have an element defined as **Primary Key.**

Row key acts as a Primary key in HBase.

Any access to HBase tables uses this Primary Key.

### Components of Apache HBase Architecture

#### HMaster

  HBase HMaster is a lightweight process that assigns regions to region servers in the Hadoop cluster for load balancing.     Responsibilities of HMaster –

    Manages and Monitors the Hadoop Cluster

    Performs Administration (Interface for creating, updating and deleting tables.)

    Controlling the failover

    DDL operations are handled by the HMaster

    Whenever a client wants to change the schema and change any of the metadata operations, HMaster is responsible for all these operations.

#### Region Server 

  Region Server runs on HDFS DataNode and consists of the following components –

    **Block Cache –** This is the read cache. Most frequently read data is stored in the read cache and whenever the block cache is full, recently used data is evicted.

    **MemStore** This is the write cache and stores new data that is not yet written to the disk. Every column family in a region has a MemStore.

    **Write Ahead Log (WAL)** is a file that stores new data that is not persisted to permanent storage.
  
    **HFile** is the actual storage file that stores the rows as sorted key values on a disk.

#### Zookeeper

  HBase uses ZooKeeper as a distributed coordination service for region assignments and to recover any region server crashes by loading them onto other region servers that are functioning. ZooKeeper is a centralized monitoring server that maintains configuration information and provides distributed synchronization. Whenever a client wants to communicate with regions, they have to approach Zookeeper first. HMaster and Region servers are registered with ZooKeeper service, client needs to access ZooKeeper quorum in order to connect with region servers and HMaster. In case of node failure within an HBase cluster, ZKquoram will trigger error messages and start repairing failed nodes.

  ZooKeeper service keeps track of all the region servers that are there in an HBase cluster- tracking information about how many region servers are there and which region servers are holding which DataNode. HMaster contacts ZooKeeper to get the details of region servers. Various services that Zookeeper provides include –

    . Establishing client communication with region servers.
    . Tracking server failure and network partitions.
    . Maintain Configuration Information
    . Provides ephemeral nodes, which represent different region servers.
    
    
### HBase Auto Sharding Concept and Explanation

One of the interesting capabilities in HBase is auto sharding, which simply means that tables are dynamically distributed by the system to different region servers when they become too large. In other word, Splitting and serving regions can be thought of as auto sharding, as offered by other systems.

In Hbase, the scalability and load balancing is handled using region. Regions are contiguous ranges of rows stored together. Regions are dynamically split by the HBase storage manager system when they become too large. Alternatively, they may also be merged to reduce their number and required storage files.

Each region is served by exactly one region server, and each region server can serve many regions at any given point of time.

Initially, when you HBase create table, there is only one region for a table. When regions become too large when you start adding more rows, the region is split into two at the middle key, creating two roughly equal halves.

  #### Identify Row Key and Region Server

   Client system does not have to contact master to put or get rows from table. It can query the meta table to identify the region server that is responsible for handling set of keys which are available in region.

  META table is a HBase system table. The mapping of Regions and Region Servers is kept in a HBase system table called META.  META table has information about which region is responsible for your key. For any read or write operation client will directly go to region server. Hmaster is not at all involved! Regions servers are responsible to server the requested data to client applications.



