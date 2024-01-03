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

## Data Models and Query Languages

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

## Storage and Retrieval

As an application developer, understanding how databases handle storage and retrieval is crucial for a few reasons:

1. **Choosing the Right Storage Engine**: There are many storage engines available, each optimized for different types of workloads. Knowing how they work helps you choose the best one for your application's needs.

2. **Performance Tuning**: To optimize a storage engine's performance for your specific workload, you need a basic understanding of its internal mechanisms.

3. **Transactional vs. Analytical Optimization**: Storage engines are often optimized for either transactional workloads (frequent updates and reads, like in web applications) or analytics (complex queries, often reading large volumes of data). Understanding this distinction is vital for selecting an engine that aligns with your use case.

4. **Implementing Efficient Operations**: Knowing how storage works under the hood can guide you in designing more efficient database interactions. For instance, understanding how logs work can inform how you handle data updates.

5. **Handling Data Growth and Scalability**: Understanding storage engines helps in planning for data growth and scalability challenges. Different engines handle data growth, concurrency, and fault tolerance in various ways.

### Data Structures That Power Your Database

- An index is a separate structure that helps quickly locate data without scanning the entire database. It speeds up reads but can slow down writes, as it also needs updating whenever data changes. This is a key trade-off in storage systems: better read performance at the cost of slower writes.
