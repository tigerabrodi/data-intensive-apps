# Foundations of Data Systems

## 1. Reliable, Scalable, and Maintainable Applications

## Thinking About Data Systems

Many applications have common needs, including:

1. **Storing Data**: They keep data in databases so it or another app can use it later.
2. **Caching**: They remember previous complex operations to make future access faster.
3. **Search and Filter**: Users can look up data using keywords or apply filters, done through search indexes.
4. **Asynchronous Messaging**: They send messages to other processes for handling later, a part of stream processing.
5. **Batch Processing**: Periodically, they process a large amount of data all at once.

### Reliability

A fault is defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user. Faults are usually caused by hardware or software faults.

Monitoring helps with detecting faults and failures.

### Scalability

Scalability: Describes a system's ability to cope with increased load.

Load parameters, which vary based on system architecture, describe the load on a system. Examples include requests per second for web servers, read/write ratios in databases, active users in chat rooms, or cache hit rates. What's important might be average usage or, in some cases, extreme usage scenarios.

Complex web of connections where one action (a tweet) spreads to many users is what "fan-out" describes.

Latency is the waiting time for a request, while response time includes the entire time from request to reply, covering processing, network, and queueing delays.

Percentile: A number where a certain percentage of the data is lower than that number. For example, the 95th percentile is the number where 95% of the data is lower than that number, e.g. 95% of requests are faster than 200ms.

**Head-of-Line Blocking:** A situation where a few slow requests delay the processing of subsequent requests.

A well-scaling architecture is based on assumptions about common and rare operations (load parameters). If these assumptions are wrong, scaling efforts can be wasteful or even harmful.

### Maintainability

Designing software with future maintenance in mind can prevent creating new legacy problems. Focus on three key design principles:

1. **Operability**: Ensure it's easy for operations teams to manage and maintain the system.
2. **Simplicity**: Aim for ease of understanding for new engineers, focusing on reducing system complexity rather than just simplifying the user interface.
3. **Evolvability (Extensibility/Modifiability)**: Design the system so it's easy to adapt and modify for future, unforeseen needs.

Operations key tasks: monitoring system health, troubleshooting, updating software, managing inter-system ineractions, planning for future issues, setting best practices, and maintaining security and stability.

## 2. Data Models and Query Languages

- SQL: Relational model. Good for structured data with fixed schema.
- NoSQL: Non-relational model. Good for semi-structured data with dynamic schema. Arised because of the need for greater scalability, and the preference for open source software. Specialized queries not supported by SQL are also a reason.
- Applications have varying needs, both relational and non-relational. Polyglot persistence is the idea of using multiple data storage technologies for different needs.
- The traditional SQL data model often clashes with object-oriented programming, creating a so-called impedance mismatch.
- Normalizing data: Splitting up tables into smaller tables and using references between them. This is good for avoiding data duplication, but can be bad for performance.
- Denormalizing data: Storing data in redundant ways to increase performance. This is good for performance, but can be bad for data integrity.
- The main arguments in favor of the document data model are schema flexibility, better performance due to locality, and that for some applications it is closer to the data structures used by the application. The relational model counters by providing better support for joins, and many-to-one and many-to-many relationships.
- The choice between them depends on your application's data structure. Document databases work well for data with a tree-like structure, where you typically load the entire tree at once. However, they have limitations, like poor support for joins and difficulty in referring to nested items.
- For applications with many-to-many relationships, document databases can become complex and less efficient. You might denormalize data to reduce joins, but this adds complexity to maintaining data consistency.
- Accessing only a small part of a large document can be inefficient, as the database loads the entire document. Additionally, updates to a document typically require rewriting the whole document, especially if the update changes its size. Therefore, it's recommended to keep documents small and avoid updates that increase their size.
- When the relational model came out, it introduced SQL, a different way to ask for data. SQL is 'declarative,' which means you just say what you want, not how to get it. Before SQL, databases used 'imperative' code, where you have to spell out every step to find your data.
- In the property graph model, each vertex (node) and edge (relationship) has its unique identifier, connections, and properties (key-value pairs).
- Graphs are flexible and extensible, ideal for evolving applications. They can represent complex relationships, like regional structures varying by country or detailed personal profiles.

## 3. Storage and Retrieval

As an application developer, understanding how databases handle storage and retrieval is crucial for a few reasons:

1. **Choosing the Right Storage Engine**: There are many storage engines available, each optimized for different types of workloads. Knowing how they work helps you choose the best one for your application's needs.

2. **Performance Tuning**: To optimize a storage engine's performance for your specific workload, you need a basic understanding of its internal mechanisms.

3. **Transactional vs. Analytical Optimization**: Storage engines are often optimized for either transactional workloads (frequent updates and reads, like in web applications) or analytics (complex queries, often reading large volumes of data). Understanding this distinction is vital for selecting an engine that aligns with your use case.

4. **Implementing Efficient Operations**: Knowing how storage works under the hood can guide you in designing more efficient database interactions. For instance, understanding how logs work can inform how you handle data updates.

5. **Handling Data Growth and Scalability**: Understanding storage engines helps in planning for data growth and scalability challenges. Different engines handle data growth, concurrency, and fault tolerance in various ways.

### Data Structures That Power Your Database

- **Hash Indexes:**

  - Purpose: Quickly access data using a key.
  - How It Works: Uses a hash function to compute an index where data is stored.
  - Use Case: Best for scenarios where you need fast lookups for exact matches.

- **SSTables (Sorted String Tables):**

  - Purpose: Store large amounts of data in a sorted manner.
  - How It Works: Data is stored in key-value pairs, sorted by key.
  - Use Case: Useful for range queries and efficient merging of data segments.

- **LSM Trees (Log-Structured Merge-Trees):**

  - Purpose: Handle high-volume write operations.
  - How It Works: Writes are added to a memtable, then flushed to SSTables. SSTables are merged and compacted in the background.
  - Use Case: Ideal for write-intensive applications, such as logging systems.

- **B-Trees:**

  - Purpose: Maintain sorted data for efficient reads and writes.
  - How It Works: Organizes data in a balanced tree structure, with data sorted within nodes.
  - Use Case: Common in database systems for indexing due to good performance with both reads and writes.

- **Comparing B-Trees and LSM-Trees:**

  - B-Trees: Generally faster for reads, good for balanced read/write workloads.
  - LSM-Trees: Typically faster for writes, better for write-heavy scenarios.
  - Both have different performance and storage characteristics, so the choice depends on specific application needs.

- **Other Indexing Structures:**
  - Include multi-dimensional, full-text search, and fuzzy indexes.
  - Each specialized for different types of queries, like geospatial data, text search, or data with typos.

### Transaction Processing or Analytics?

- **Data Warehousing:**

  - Purpose: Analyze and report large datasets, separate from transactional databases.
  - Characteristics: Optimized for read-heavy, complex queries. Stores historical data for business intelligence.

- **Stars and Snowflakes (Schemas in Data Warehousing):**
  - Star Schema: Simple, with a central fact table linked to dimension tables.
  - Snowflake Schema: More normalized, breaks down dimension tables into subdimensions.
  - Choice depends on complexity and analytical needs.

### Column-Oriented Storage

#### Column-Oriented Storage

- **Purpose:** Optimized for reading large datasets, especially in analytics and data warehousing.
- **How it Works:** Stores data by columns rather than rows, allowing efficient reading of specific attributes without loading entire records.
- **Use Case:** Ideal for queries scanning large datasets for a few columns, such as aggregations and analytics.

#### Column Compression

- **Purpose:** Reduces storage space and improves read performance.
- **How it Works:** Applies compression techniques like bitmap encoding, exploiting data repetition and patterns within columns.
- **Use Case:** Useful in data warehouses with repetitive or patterned data in columns, enhancing query speed and reducing disk I/O.

#### Sort Order in Column Storage

- **Purpose:** Enhances query performance and compression efficiency.
- **How it Works:** Data is sorted row-wise but stored column-wise. Primary sort columns show strong compression.
- **Use Case:** Beneficial for range queries and analytics where sorting by specific keys (like dates or categories) is common.

#### Writing to Column-Oriented Storage

- **Purpose:** To manage updates and new data insertions efficiently in a columnar format.
- **How it Works:** Uses structures like LSM-trees. Writes are first accumulated in-memory and then merged into on-disk column files.
- **Use Case:** Suitable for systems where read performance is critical, and writes can be batched and processed periodically.

#### Aggregation: Data Cubes and Materialized Views

- **Purpose:** Accelerates common aggregate queries.
- **How it Works:**
  - **Data Cubes:** Precomputes and stores aggregate data in multi-dimensional cubes for rapid querying.
  - **Materialized Views:** Stores the results of aggregate queries on disk for faster access.
- **Use Case:** Effective in scenarios where the same aggregate calculations are performed frequently, such as business intelligence and reporting.

## 4. Encoding and Evolution

- **Evolvability in Applications:** Applications evolve with new features and changing requirements, often leading to changes in the data they store.
- **Relational vs. Schema-on-Read Databases:**
  - Relational databases use a fixed schema which can be updated with migrations.
  - Schema-on-read databases allow mixed data formats, offering more flexibility.
- **Code and Data Format Changes:** When data formats change, application code must also adapt to handle new and old data formats.
- **Compatibility Considerations:**
  - **Backward Compatibility:** New code versions can process data created by older versions.
  - **Forward Compatibility:** Older code versions can handle data created by newer code.
- **Compatibility in Large Applications:** Rolling upgrades and varying user update rates mean both old and new code/data formats coexist, necessitating careful compatibility management.
- **Data Encoding Formats:** Formats like JSON, XML, Protocol Buffers, Thrift, and Avro handle schema changes differently and support compatibility in varying degrees.

### Formats for Encoding Data

- **Language-Specific Formats:**

  - Quick and easy encoding within a single programming language.
  - Not suitable for long-term storage or inter-language communication due to language dependency and security risks.

- **JSON, XML, and Binary Variants:**

  - Language-independent, text-based formats.
  - JSON and XML: Popular for web APIs. Face issues with number precision and binary data handling.
  - Binary variants of JSON/XML: More compact, but less human-readable.

- **Thrift and Protocol Buffers:**

  - Binary encoding formats requiring a schema.
  - Efficient in size, use tag numbers for fields, and support cross-language compatibility.
  - Suitable for high-performance, inter-service communication.

- **Avro:**

  - Binary format that relies on schemas for data structure.
  - Schema evolution allows backward and forward compatibility.
  - No tag numbers in schema, making it friendly for dynamically generated schemas.

- **The Merits of Schemas:**
  - Provide documentation, ensure data format consistency.
  - Enable schema evolution for compatibility.
  - Beneficial for statically typed languages due to compile-time type checking.

### Modes of Dataflow

- **Dataflow Through Databases:**

  - Data is written and later read by different processes.
  - Suitable for long-term storage with forward and backward compatibility considerations.

- **Dataflow Through Services (REST and RPC):**

  - REST: Uses HTTP with emphasis on statelessness and cacheability.
  - RPC: Makes network requests appear as local function calls, with compatibility concerns.
  - Common in microservices and inter-service communication.

- **Message-Passing Dataflow:**
  - Asynchronous communication via message brokers.
  - Offers reliability and decoupling, suitable for distributed systems.
  - Different from direct RPC and database storage due to intermediary (broker) involvement.

# Distributed Data

## Replication

Replication involves storing the same data on multiple network-connected machines.

Why?

- **Close to Users:** Place data near users to reduce latency.
- **High Availability:** Ensure system operation despite failures.
- **Read Scalability:** Increase capacity for handling read requests.

### Leaders and Followers

What's a Replica?

- **Replica**: It's like a copy of your database on different computers.

- **Leader**: One replica is the boss (leader). When you want to add or change data, you tell this leader.

- **Followers**: Other replicas are followers. They copy whatever the leader does. They just look at the leader's changes and make the same changes in their own copies. They get the changes from replication log.

How it works?

1. **Writing Data**: If you need to add new info to your database, you send it to the leader.
2. **Updating Everyone**: The leader first saves this new info for itself, then tells all the followers about this new change.
3. **Followers Update**: Each follower gets this update and adds the new info to their copies, exactly in the order the leader did.

---

1. **Synchronous vs. Asynchronous Replication**:

   - **What**: Methods to replicate data across database nodes.
   - **Purpose**: To synchronize data across multiple replicas for consistency and availability.
   - **How it Works**:
     - **Synchronous**: Leader waits for confirmation from followers before considering a write complete.
     - **Asynchronous**: Leader doesn't wait for followers, reducing write latency but increasing risk of data loss.
   - **Use Cases**: Synchronous for data integrity. Asynchronous for performance and scalability.

2. **Setting Up New Followers**:

   - **What**: Process of adding new replicas to a database system.
   - **Purpose**: To increase data availability and load distribution.
   - **How it Works**: Snapshot leader's data, copy to new node, then sync any changes since snapshot.
   - **Use Cases**: Scaling, load balancing, replacing failed nodes.

3. **Handling Node Outages**:

   - **What**: Maintaining database operation despite node failures.
   - **Purpose**: To ensure high availability and data integrity.
   - **How it Works**:
     - **Follower Failure**: Recover using data change logs.
     - **Leader Failure (Failover)**: Promote a follower to a new leader, reconfigure system.
   - **Use Cases**: Fault tolerance, maintenance without downtime.

4. **Implementation of Replication Logs**:
   - **What**: Mechanisms to record and transmit data changes in replication.
   - **Purpose**: To ensure data consistency across replicas.
   - **How it Works**: Various methods like statement-based, write-ahead logs (WAL), logical logging.
   - **Use Cases**: Data consistency, recovery, change data capture.

### Problems with Replication Lag

- **Problems with Replication Lag**: This occurs when there's a delay in updating data across servers. It can lead to users seeing outdated information. For instance, a user might not see a recent update they made because it hasn't been copied to the server they're accessing.

- **Monotonic Reads**: These prevent users from seeing data out of order. If a user accesses data from multiple servers, monotonic reads ensure they don't see newer data first and then older data, which can be confusing.

- **Consistent Prefix Reads**: These ensure users see events in the right order. For example, in a chat, it ensures responses appear after the questions, not before. This is important in systems where different parts of the data update at different speeds.

- **Solutions for Replication Lag**: To deal with replication lag, you can:
  1. Read from the main server after updates to ensure up-to-date information.
  2. Use timestamps to ensure data read is recent.
  3. Keep track of update sequences to maintain order.
  4. Route user requests to the same server to maintain consistency.
  5. In case of a big lag, consider stronger guarantees like transactions, which can handle order and consistency more effectively.

### Multi-Leader Replication

**Multi-Leader Replication**

- **What**: A replication strategy where multiple nodes (leaders) can independently accept and process write operations.
- **Purpose**: To enhance performance and fault tolerance, especially across geographically distributed systems or disconnected clients.
- **How it Works**: Each leader handles writes locally and then replicates these changes to other leaders.
- **Use Cases**: Multi-datacenter operations, offline client operation, real-time collaborative editing.

**Use Cases for Multi-Leader Replication**

- **Multi-Datacenter Operation**: Each datacenter has its leader, improving write performance and reducing latency.
- **Clients with Offline Operation**: Devices or clients work independently offline and sync when online.
- **Collaborative Editing**: Multiple users edit the same document simultaneously with changes synced in real-time.

**Handling Write Conflicts**

- **What**: Resolving data discrepancies when the same data is modified independently on different leaders.
- **Approaches**:
  1. Conflict Avoidance: Directing all writes for specific data to the same leader.
  2. Last Write Wins: The most recent write overwrites others.
  3. Custom Conflict Resolution: Application-specific logic to merge or choose between conflicting changes.

**Multi-Leader Replication Topologies**

- **All-to-All**: Each leader replicates its changes to every other leader. Offers robust fault tolerance.
- **Circular**: Leaders form a ring, with each node forwarding data to the next. Simple but vulnerable to node failures.
- **Star**: A central "root" node replicates changes to all other nodes. Efficient but creates a single point of failure.

### Leaderless Replication

1. **Leaderless Replication:**

   - **What:** A replication model where any replica can directly accept writes from clients.
   - **Purpose:** Increases robustness against node failures and network issues. Suitable for scenarios where low latency and high availability are crucial.
   - **How It Works:** Clients send write requests to multiple replicas. Reads are also sent to multiple nodes to detect stale data.
   - **Use Cases:** Dynamo-style databases (e.g., Riak, Cassandra, Voldemort), applications needing high availability and handling eventual consistency.

2. **Writing to the Database When a Node Is Down:**

   - **What:** Handling write operations in a leaderless system when one or more replicas are unavailable.
   - **How It Works:** Write requests are sent to all replicas, but only a subset (e.g., 2 out of 3) needs to acknowledge for the write to be successful.
   - **Purpose:** Maintains write availability despite individual node failures.
   - **Use Cases:** Any situation with intermittent node availability, maintenance periods, or network issues.

3. **Limitations of Quorum Consistency:**

   - **What:** Challenges in ensuring that reads always return the most recent write in a leaderless system.
   - **How It Works:** Relies on overlapping sets of nodes for reads and writes (w + r > n) but can face issues with concurrent writes and network delays.
   - **Purpose:** Balances read availability with consistency.
   - **Use Cases:** Optimizing for either read or write performance in distributed systems, handling network partitions.

4. **Sloppy Quorums and Hinted Handoff:**

   - **What:** An extension of the quorum model to increase write availability during network partitions.
   - **How It Works:** Writes are temporarily allowed to non-designated nodes (sloppy quorum), which later hand off the data to the correct nodes (hinted handoff).
   - **Purpose:** Improves write availability during network issues.
   - **Use Cases:** Scenarios with unreliable networks or frequent partitions, enhancing robustness in distributed databases.

5. **Detecting Concurrent Writes:**
   - **What:** Identifying and resolving conflicts from simultaneous writes to the same key in a leaderless system.
   - **How It Works:** Using version numbers or vectors to track write order. Resolving conflicts by merging or picking the latest write.
   - **Purpose:** Ensures eventual consistency even with concurrent data modifications.
   - **Use Cases:** Leaderless databases handling concurrent user interactions, applications requiring automatic conflict resolution.

## Partitioning
