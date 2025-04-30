# Codd's Twelve Rules

1. **Information Rule** - All data is represented in tables.
2. **Guaranteed Access Rule** - Every value accessible via table, row, column.
3. **Systematic Treatment of Nulls** - Uniform handling of missing data (NULL).
4. **Dynamic Online Catalog** - Metadata stored in tables, queryable by SQL.
5. **Comprehensive Data Sublanguage Rule** - Single language (e.g., SQL) for definition, manipulation, and transactions.
6. **View Updating Rule** - Updatable views propagate changes to base tables.
7. **Set-Level Insertion, Update, and Deletion** - Bulk operations on multiple rows.
8. **Physical Data Independence** - Storage changes don’t affect applications.
9. **Logical Data Independence** - Schema changes (new columns/tables) don’t break apps.
10. **Integrity Independence** - Constraints managed in catalog, not in code.
11. **Distribution Independence** - Data distribution is transparent to users.
12. **Nonsubversion Rule** - Low‑level interfaces cannot bypass relational constraints.

---

# Dilbert's View

A tongue-in-cheek take on how real-world corporate practices often clash with Codd’s relational ideals—highlighting bureaucratic workarounds, inconsistent data handling, and routine circumvention of database constraints.

---

# Why Relational DBMSs Fail

- Can they handle Web‑Scale?
  - Not sure;
  - RDBMSs have trouble with petabytes and exabytes—why?
  - But so do some NoSQL DBMSs
- Limited horizontal scalability along rows?
  - Adding a column to a table means migrating to a new schema
- Most built on a scale‑up rather than scale‑out architecture
- Inflexible data model: Tables of rows and columns
  - Cannot represent graphs or social networks easily
  - Difficult to represent documents, sound files, and images; hence, BLOBs (Binary Large Objects)
  - Cannot effectively address data that are completely unstructured and unknown in advance
- Relational model maps poorly to some types of problems
- Limitations imposed by OS

---

# ACID Properties of Transactions

- **Atomic** – All of the work in a transaction completes (commit) or none of it completes
  - Failure to complete any step of a transaction results in rollback to the starting state.
- **Consistent** – Ensures the database moves from one valid state to another by enforcing all integrity constraints.
  - Validates entity, referential, and domain constraints at commit time.
  - If any constraint is violated, the entire transaction is rolled back to preserve data integrity.
- **Isolated** – The results of any changes made during a transaction are not visible until the transaction has committed.
  - Ensures that multiple transactions are synchronized so that only one completes at a time.
  - During transaction execution, the system may not be consistent.
  - At transaction completion, the system is consistent.
- **Durable** – The results of a committed transaction survive failures.
  - Changes made to the system by successful transactions are durable.

*A transaction is a single logical unit of work which accesses and possibly modifies the contents of a database.*

---

# NoSQL Data Management Systems

A class of data management systems that are inherently:

- Non-relational
- Distributed with replication (and redundancy)
- Vertically and horizontally scalable
- With optional or no schemas
- Provide simple APIs

They typically:

- Break rules 1 and 12 of Codd’s 12 Rules (see Appendix)
- Use the CAP Theorem
- Offer flexible data models (hashtables, maps, dictionaries, etc.)

## Rationale for NoSQL Databases

- **Need for Speed:**
  - When a fast response is required, data needs to be stored in memory.
  - *(e.g., caching user session data in Redis for sub-millisecond retrieval)*
- **Need for Scale:**
  - Databases need to scale with increasing number of users and volume of data.
  - Generally, users fluctuate, but data continually increases.
- **Need for Continuous Availability:**
  - Achieved via redundant copies of data spread across a cluster of servers.
  - Ensures service remains online despite node failures (true for both RDBMS and NoSQL).
- **Need for Location Independence:**
  - Ability to serve data from multiple geographic locations seamlessly.
  - RDBMS with master-slave architectures often struggle to provide write access from all regions.

## BASE Properties

- **Basically Available:** Guarantees the system remains available even under failures.
- **Soft state:** System state may change over time, even without input, due to eventual propagation.
- **Eventual consistency:** Updates are propagated asynchronously; replicas converge given no new updates.

**Characteristics:**

- Alternative to ACID in NoSQL contexts, preferring availability over strong consistency.
- Allows stale reads and best-effort responses—useful for large-scale, distributed workloads.
- Optimistic approach enables high throughput and low latency.

*Used in NoSQL systems where speed, partition tolerance, and fault-tolerance are prioritized—e.g., Dynamo-style key–value stores, Cassandra, and Riak.*

## CAP Theorem

The CAP Theorem states that a distributed system can only guarantee two of the following three properties simultaneously:

- **Consistency (Eventual)**\
  All nodes see the same data at the same time; with no new updates, replicas will converge to the same state.
- **Availability**\
  Every request receives a response (success or failure), and surviving nodes continue operating despite failures.
- **Partition Tolerance**\
  The system continues to operate despite arbitrary network failures or message loss between nodes.

In practice, network partitions are inevitable in distributed environments, so NoSQL systems choose to trade off between Consistency and Availability:

- **AP Systems** (e.g., Cassandra, Riak, DynamoDB)\
  Prioritize Availability and Partition tolerance; may return stale data during partitions.
- **CP Systems** (e.g., HBase, MongoDB in certain configurations)\
  Prioritize Consistency and Partition tolerance; may reject or delay requests under partitions to maintain consistency.

CA (Consistency + Availability) without Partition tolerance is only achievable in single-site deployments or tightly coupled clusters.

*By acknowledging the CAP trade-offs, system architects can select the right NoSQL database configuration to match application requirements.*

## Key-Value Stores

A simple and highly performant NoSQL model where data is represented as unique key–value pairs:

- **Basic operations:**
  - **GET(key):** Retrieve the value for a given key.
  - **PUT(key, value):** Insert or overwrite the value for a key.
  - **DELETE(key):** Remove a key and its associated value.
- **Key characteristics:**
  - Keys are arbitrary, unique strings; values are opaque blobs—interpretation is the client’s responsibility.
  - Native lookup is only by key; querying on value fields isn’t supported.
- **Scalability and availability:**
  - Built for easy horizontal scaling—data sharding by key.
  - Replicated across clusters for redundancy and fault tolerance, but replication complicates transactional guarantees.
- **Management features:**
  - Some systems offer namespaces or sets to group related keys for easier administration.
- **Use cases:**
  - Caching (e.g., session stores), user preferences, feature flags, and any scenario requiring sub-millisecond lookups.

*Ref: **[https://db-engines.com/en/ranking/key-value+store](https://db-engines.com/en/ranking/key-value+store)*

## Column-Oriented Stores

Column-oriented (wide-column) databases organize data by columns rather than rows:

- **Data Model:**
  - Data stored in column families, each identified by a row key.
  - Each row can have variable columns; names and formats may differ per row.
  - Columns indexed by (row key, column key, timestamp); values are opaque byte arrays.
- **Performance Benefits:**
  - **Reduced I/O:** Only relevant columns are read for a query, minimizing disk access.
  - **Better Caching:** Columnar storage improves cache locality and compression ratios.
  - **Efficient Aggregation:** Summation, averaging, counting across columns execute faster.
- **Flexibility:**
  - New columns can be added dynamically on a per-row basis without schema migrations.
  - Group related columns into families for logical organization.
- **Scalability and Availability:**
  - Horizontally scalable across clusters with automatic partitioning by row key.
  - Built‑in replication ensures fault tolerance and high availability.
- **Major Examples:**
  - Apache Cassandra, HBase (inspired by Google Bigtable), ScyllaDB.

*Column stores excel in analytical workloads, time-series data, and scenarios requiring fast reads of a subset of attributes across many records.*

---

# Cassandra

## Overview

- Originally developed at Facebook; open sourced in July 2008.
- Hybrid model: combines key–value semantics with wide‑column storage.
- Designed for high availability and eventual consistency, trading off strong consistency.
- Supports incremental scalability with peer-to-peer, masterless architecture.
- Uses optimistic (lazy) replication across nodes.
- Provides tunable settings to balance consistency, durability, and latency.
- Low total cost of ownership and minimal administrative overhead.

*Ref: NoSQL and Big Data Processing, adapted from slides by Perry Hoekstra, Jiaheng Lu, Avinash Lakshman, Prashant Malik, and Jimmy Lin*

## Architecture

- **Peer-to-Peer Ring:** All nodes are equal; no single master.
  - Nodes form a logical ring; data is partitioned by hashing the row key.
  - Each node is responsible for a range of data, determined by token assignments.
- **Replication:**
  - Configurable replication factor across multiple nodes for fault tolerance.
  - Writes are recorded in a commit log for durability.
  - Replication can be synchronous (quorum) or asynchronous, depending on consistency settings.
- **Request Handling:**
  - Any node can service read/write requests regardless of data location.
  - During node failures or network partitions, other replicas continue serving requests.
- \**Durability and Consistency:\**
  - Commit log ensures writes survive node crashes.
  - Hinted handoff and read-repair mechanisms maintain eventual consistency.

## Write Operations

- **Coordinator Node:** The node receiving the client’s write request acts as the coordinator.
  - It hashes the partition key to determine the replica set based on the cluster’s token ring.
- **Commit Log & Memtable:** The coordinator writes the mutation to its commit log (for durability) and applies it to the in-memory memtable.
- **Replication:** The coordinator forwards the write to all replicas as per the replication factor; each replica also logs and applies the mutation.
- **Consistency Level:** The client’s desired consistency level (ALL, QUORUM, ONE, etc.) dictates how many replicas must acknowledge before returning success.
- **Flushing to SSTables:** Full memtables are flushed to disk as immutable SSTable files, and periodic compaction merges SSTables to optimize reads.

## Read Operations

- **Coordinator Node:** Upon a read request, the coordinator identifies replicas responsible for the partition key.
- **Consistency Level:** Based on the requested consistency level, the coordinator queries the required number of replicas (e.g., QUORUM, ALL, ONE).
- **Memtable & SSTable Lookup:** Each contacted replica checks its memtable and SSTables for the data, returning the latest version it has.
- **Result Merging:** The coordinator merges replica responses, selects the newest values by timestamp, and filters out tombstones (deletions).
- **Read Repair & Hinted Handoff:** If inconsistencies are detected between replicas, background read-repair fixes out-of-date replicas, and hinted handoff delivers missed writes to recovering nodes.

## Data Model

Cassandra’s flexible, wide-column data model is defined via CQL but stored as column families under the hood:

- **Keyspace:**\
  Top-level namespace (analogous to a database/schema) that groups tables and specifies replication strategy and factor.

- **Tables (Column Families):**\
  Defined in CQL, each table has a primary key and dynamic columns. Underlying storage remains a column family for sparse, efficient storage.

- **Primary Key:**\
  Composed of a **partition key** and optional **clustering columns**:

  - **Partition Key:** Determines data distribution across the ring by hashing; can include multiple columns for composite partitioning.
  - **Clustering Columns:** Define the sort order of rows within a partition, enabling efficient range queries.

- **Rows and Columns:**\
  Each row is uniquely identified by its primary key. Columns are flexible—new columns can be added at any time without schema migrations, and rows need not share identical columns (sparse storage).

- **Collection Types & UDTs:**\
  Supports native collections (sets, lists, maps) and user-defined types for grouped data in a single column.

- **Wide Rows:**\
  Optimized for scenarios like time-series or IoT data—rows can contain thousands or millions of columns under the same partition.

- **TTL and Tombstones:**\
  Columns and rows can have a time-to-live (TTL) for automatic expiration; deletions generate tombstones that are later purged during compaction.

- **Replication & Storage Granularity:**

  - **Row as Unit of Replication:** Entire partitions (rows identified by the partition key) are the granularity for replication across the cluster.
  - **Column as Unit of Storage:** Individual columns within rows are stored, read, and compressed independently, enabling efficient disk I/O and flexible schema evolution.

*This data model combines RDBMS‑like table definitions with NoSQL flexibility under the hood.*

---

# Document Stores

- **Key–Document stores:**  
  Documents are collections of named fields and values, typically represented as JSON, XML, BSON, RDF, etc.  
- **Dynamic schema:**  
  Each document can contain different fields; new fields may be added without impacting other documents.  
- **Structured data:**  
  Unlike opaque blobs, documents maintain internal structure, making them self-describing and easy to read/write.  
- **No JOINs:**  
  All related data resides within the same document, eliminating multi-record transactions and joins (queries across documents must be handled in application logic).  
- **In-place updates:**  
  Most document databases support partial updates, so you can modify specific fields without rewriting the entire document.  
- **Querying:**  
  Supports search by field names and values, often with rich JSON-based query languages and indexing.  
- **Use cases & examples:**  
  Ideal for content management, user profiles, event logging, and anywhere flexibility is needed.  
  Common systems include MongoDB, Couchbase, and Amazon DocumentDB.

---

# MongoDB

## Background
- Developed by 10Gen (founded in 2007; over 200 employees).
- Aims to scale horizontally over commodity hardware.
- Addresses challenges like long-running multi-row transactions and JOINs by avoiding them.
- Supports in-memory processing when possible, auto-sharding, and dynamic capacity management without downtime.
- Allows querying both document contents and structure.

## Features
- **Document-Oriented Storage:**  
  - JSON-style documents with dynamic schemas stored in BSON (binary JSON) format.  
- **Indexing:**  
  - Full index support on any attribute, similar to relational databases.  
- **Durability & High Availability:**  
  - Replica sets across LANs and WANs; asynchronous replication ensures durability and uptime.  
- **Querying & Aggregation:**  
  - Rich, document-based queries, aggregation pipelines, and MapReduce functions.  
- **Updates:**  
  - Fast in-place updates with atomic modifiers to avoid contention.  
- **GridFS:**  
  - Stores and retrieves large binary files by splitting them into smaller chunks.  
- **Management:**  
  - MongoDB Management Service (MMS) for monitoring and backups.

## Database Structure
Although often called “schemaless,” MongoDB encourages a schema by convention:
- **Community-Imposed Schema:**  
  Application developers define and enforce document structure through shared guidelines, validation rules, and coding standards.  
- **Schema Validation:**  
  MongoDB’s schema validation (JSON Schema) can enforce field types, required fields, and value constraints at the collection level.  
- **Flexible Fields:**  
  Documents in the same collection may include different fields, but a consistent schema aids maintainability and indexing efficiency.  
- **Index Design:**  
  Indexes on frequently queried fields implicitly shape the effective schema by dictating supported queries and access patterns.  
- **Data Modeling Patterns:**  
  Embed related data or reference across collections based on query patterns, balancing document size, atomicity, and normalization.

## Replication and Sharding
- **Replication (Replica Sets):**  
  - Data is replicated across a set of nodes: one **primary** receives all write operations, and multiple **secondaries** replicate the primary’s oplog.  
  - **Automatic failover:** On primary failure, secondaries hold an election to select a new primary, ensuring high availability.  
  - **Read preferences:** Clients can configure reads from primary or secondaries (e.g., primaryPreferred, secondaryPreferred) to balance consistency and load.  
  
- **Sharding (Horizontal Partitioning):**  
  - Distributes data across multiple shards, each shard being a replica set, to scale writes and storage linearly.  
  - **Shard key:** A field or compound fields chosen to evenly distribute data; MongoDB splits data into chunks based on the shard key’s range or hashed values.  
  - **Config servers:** Store metadata and cluster configuration; usually deployed as a replica set for reliability.  
  - **Query routers (mongos):** Act as stateless routing services that direct client operations to the appropriate shards.  
  - **Balancing:** A background process moves chunks between shards to maintain even data distribution and load.  

## Embedded Data Model in MongoDB
- **Contains relationships:** Parent documents can contain embedded child documents, modeling direct containment of related data.  
- **One-to-many patterns:** Embed arrays of subdocuments to represent one-to-many relationships within a single document.  
- **Single-query retrieval:** All related data is collocated, allowing the application to fetch parent and child documents together in one request.  
- **Data redundancy trade-off:** Embedding can duplicate data across documents, but optimizes read performance by reducing the need for joins or multiple queries.  

---

## Representing Complex Relationships
- **Avoid data duplication:** Use referencing instead of repeated embedding when redundancy grows.
- **Model large hierarchical datasets:** Combine embedding for closely related data and referencing for deeper hierarchies.
- **Support diverse structures:** Accommodate trees, graphs, linked lists via nested documents or manual references.
- **Multiple query patterns:** Design data to satisfy various query types (e.g., fast parent lookup vs. child-specific searches).
- **Performance trade-off:** Excessive embedding can degrade read performance; balance embedding with references.

---

## MongoDB Query Example
```javascript
// Find a user aged 39
db.user.findOne({ age: 39 });
```
**Example result:**
```json
{
  "_id": ObjectId("5114e0bd42…"),
  "first": "John",
  "last": "Doe",
  "age": 39,
  "interests": ["Reading", "Mountain Biking"],
  "favorites": {
    "color": "Blue",
    "sport": "Soccer"
  }
}
```
Data is enclosed in curly braces (`{}`) as key–value pairs.  Document values can be nested arrays or subdocuments for complex data.

---

## MongoDB CRUD Operations
- **Create**
  - `db.collection.insert(<document>)`
  - `db.collection.save(<document>)`
  - `db.collection.update(<query>, <update>, { upsert: true })`
- **Read**
  - `db.collection.find(<query>, <projection>)`
  - `db.collection.findOne(<query>, <projection>)`
- **Update**
  - `db.collection.update(<query>, <update>, <options>)`
- **Delete**
  - `db.collection.remove(<query>, <justOne>)`

---

# JSON (JavaScript Object Notation)

- Text-based, human-readable format for representing structured data.
- Common in client-server communication; converts JavaScript objects to text and back.
- Key syntax elements:
  - `{}` defines objects (key–value maps).
  - `[]` defines arrays (lists).
  - Supports strings, numbers, booleans, and nested structures.
- Widely supported by parsers in virtually all programming languages.
- Preferred over XML or RDF for simplicity and lightweight parsing.

---

# Graph Databases
- **Nodes, Edges, and Properties:** Data is modeled as nodes (entities), edges (relationships), and properties attached to both.
- **Nodes represent entities:** e.g., "Bob" or "Alice," analogous to objects in OOP.
- **Properties:** Key–value pairs that describe nodes (e.g., age: 18) or edges (e.g., since: 2020).
- **Edges connect nodes:** Define relationships between entities; can be directed or undirected.
- **Use cases:** Efficient for social networks, recommendation systems, fraud detection, and any domain with highly interconnected data.
- **Scaling challenges:** Distributing and partitioning graph data across clusters is complex, as traversals often cross partitions and require low-latency joins.

---

# Advantages of NoSQL Databases
NoSQL databases excel in scenarios where flexibility, performance, and scalability are paramount. Key advantages include:

- **Reduced ETL complexity:** Store data in its native format—flat or nested—without extensive transformation pipelines.
- **Unstructured text support:** Many NoSQL systems index and query free‑form text natively or via integration with search engines like Elasticsearch.
- **Schema agility:** Accommodate evolving data models without downtime or complex migrations, enabling rapid feature development.
- **Horizontal scalability:** Seamlessly shard across commodity hardware to handle growing data volumes and user loads.
- **In-place computation:** Co-locate processing (e.g., MapReduce) with data to achieve near-linear performance on large datasets.
- **Functionality breadth:** Choose the best model—key–value, document, column‑family, or graph—to fit specific application needs without forcing a one-size-fits-all schema.
- **Lightweight footprint:** Modern NoSQL offerings minimize legacy bloat, lowering administrative overhead and cost of ownership.

# Disadvantages of NoSQL Databases
Despite their strengths, NoSQL databases require careful consideration of trade‑offs:

- **Evolving maturity:** Data models and feature sets are still growing; geospatial and temporal capabilities may lag behind RDBMS offerings.
- **Limited vendor support:** Open-source versions often rely on community forums; enterprise-grade support may incur additional cost.
- **Analytics integration:** While many NoSQL systems offer APIs for languages like Java, Python, or Scala, integrating with traditional BI tools can require translation layers (e.g., Pig, Hive).
- **Expertise demands:** Effective data modeling, performance tuning, and distributed administration necessitate specialized skills that many teams must acquire.
- **Operational complexity:** Managing a distributed cluster—monitoring, backup, security, and scaling—often surpasses the simplicity of single-node RDBMS deployments.

---

# Issues with NoSQL Databases
NoSQL databases introduce unique challenges across security, consistency, and operational domains:

## 1. Security and Defaults
- **Out-of-the-box risks:** Many NoSQL platforms ship with default ports, open network interfaces, and no authentication enabled.
- **Weak credentials:** Defaults often include simple or no password protection; client–server communication may occur in plaintext.
- **Lack of built‑in encryption/auditing:** Encryption at rest and comprehensive audit logging are not universally supported.
- **Evolving maturity:** Security hardening features are improving but vary widely by vendor.

## 2. Consistency and Transactions
- **Limited strong consistency:** If up-to-the-second accuracy is critical, many NoSQL systems may not meet requirements.
- **ACID vs. BASE convergence:** Some vendors (e.g., Oracle NoSQL Cloud) are blending models, but feature sets remain in flux.
- **Atomic multi-document operations:** Distributed transactions spanning multiple shards or nodes can be problematic or unsupported.

## 3. Query Language Standardization
- **Diverse query syntaxes:** No single standard SQL-like language; each system has its own API and query dialect.
- **Graph query variability:** Graph databases, in particular, lack a universally adopted query language.
- **Interoperability layers:** Tools like Pig or SQL‑Lite adapters exist but add complexity and may not expose all features.

## 4. Sharding and Scalability Pitfalls
- **Automatic sharding limits:** While auto-sharding eases distribution, ensuring uniform key distribution and hotspot avoidance requires careful shard key selection.
- **Unique constraint enforcement:** Validating uniqueness across shards often requires additional coordination or sacrifices performance.

## 5. Data Modeling and Query Patterns
- **Non-relational schema design:** Teams must rethink normalization; designing for access patterns rather than entity models.
- **Multi-query workflows:** Achieving complex joins or aggregations may require multiple round-trips, though each query tends to be faster.
- **Developer expertise:** Effective modeling demands understanding both the data and the system’s access semantics.

## 6. Replication, Caching, and Redundancy
- **Denormalization trade-offs:** Storing redundant foreign data (e.g., embedding usernames) simplifies reads but complicates updates.
- **Cache consistency:** Updates to denormalized fields must be propagated across all copies, increasing maintenance overhead.

*Awareness of these issues helps teams adopt NoSQL solutions with realistic expectations and robust mitigations.*

---

# NoSQL DB Weaknesses
- **Complex transactional data:** Limited support for multi-document or cross-shard transactions can lead to lost updates.
- **Joins & validations:** Joins and data validation logic must be implemented at the application level or via additional tooling, increasing complexity.
- **Consistency concerns:** Strong consistency guarantees are often relaxed, requiring careful trade-offs and client-side safeguards.
- **Data governance:** Lack of mature governance frameworks for metadata management, lineage, and compliance.
- **Evolving security features:** Security capabilities are still maturing; require additional configuration and third-party integrations.
- **Portability between systems:** APIs and query languages vary widely, complicating migrations and vendor lock-in risks.
- **Fault tolerance nuances:** Handling partial server failures and maintaining uninterrupted service demands custom resilience strategies.
- **Support model issues:** FOSS business models often lack enterprise SLAs; professional support may incur additional cost.
- **Ad hoc data fixes & reporting:** Manual corrections and reporting can be error-prone without standardized export/import mechanisms.
- **Data export challenges:** Limited or inconsistent APIs for bulk data export can impede backup, analytics, and interoperability.

---

# Discussion Questions

1. **What does “Data Governance” mean in the context of databases?**  
   Establishing policies, processes, and roles to ensure data quality, consistency, security, and compliance across the organization—covering metadata management, access controls, and lifecycle management.

2. **What applications would you use a document store for and why?**  
   Use document stores for flexible, schema-evolving data such as content management systems, user profiles, and event logging—because they allow nested structures, in-place updates, and efficient retrieval of related data without complex joins.

