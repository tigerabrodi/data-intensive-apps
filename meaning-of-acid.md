# Meaning of ACID

The well known acronym ACID stands for Atomicity, Consistency, Isolation, and Durability. These are the properties that a transaction should have in order to be considered reliable.

All databases do not implement these properties in the same way. For example, there are different levels of isolation that a database can provide. The most common levels are Read Uncommitted, Read Committed, Repeatable Read, and Serializable.

I'll write a different post explaining the isolation levels in more detail.

# Atomicity

In computing, atomicity will refer to different things. In ACID, atomicity is not about concurrency, but about the transaction itself. A transaction is atomic if it is considered as a single unit that either succeeds completely or fails completely.

If something goes wrong during the transaction, the database should be able to rollback the changes made by the transaction. The transaction is then said to be aborted.

With atomicity, it is safe to retry a transaction that has failed. The database will ensure that the transaction is not executed twice.

# Consistency

Consistency in ACID refers to the state of the database before and after the transaction. The database should be in a consistent state after the transaction, regardless of whether the transaction was successful or not. A "consistent state" is a state that satisfies all the constraints and rules defined in the database schema.

However, the idea of consistency here is not the database's job, but the application's job. The database will ensure that the transaction is atomic, but it is up to the application to ensure that the transaction is consistent.

So actually, the C in ACID shouldn't be there. The database is not responsible for making the transaction consistent.

# Isolation

Isolation in ACID refers to the ability of the database to isolate transactions from each other. This means that the changes made by a transaction should not be visible to other transactions until the transaction is committed. This is the most complex property to get right, and it is the one that is most often violated.

A problem could happen when two transactions are trying to read and write the same data at the same time. If the database does not handle this situation correctly, it could result in data corruption or inconsistency.

When a transaction is isolated, it's as if it is the only transaction running on the database.

AS mentioned earlier, there are different levels of isolation that a database can provide. The most common levels are Read Uncommitted, Read Committed, Repeatable Read, and Serializable. Each level has its own trade-offs in terms of performance and consistency. I'll cover them more in detail in a different post.

# Durability

Durability in ACID refers to the ability of the database to ensure that the changes made by a transaction are permanent. Once a transaction is committed, the changes should be stored in a way that they are not lost even if the database crashes.

In a single-node database, durability is usually achieved by writing the changes to disk. This typically involves a write-ahead log or something similar.

In a distributed database, durability is achieved by replicating the changes to multiple nodes.

Perfect durability is impossible to achieve because if your data center and all its backups are destroyed, you will lose your data. But the goal is to make it as durable as possible.
