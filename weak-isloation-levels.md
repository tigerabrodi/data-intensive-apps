# Weak Isolation Levels

If two transactions don't modify the same data, they can run concurrently without interfering with each other. However, if they do modify the same data, issues can arise.

This can lead to concurrency bugs, such as lost updates, dirty reads, and write skews. To prevent these issues, databases use isolation levels.

Isolation levels define the degree to which transactions are isolated from each other. We'll go over the weaker ones in this post.

Serializable is the strongest isolation level. It's as if transactions are running one after the other, even though they're running concurrently. Many databases don't want to pay the performance cost of Serializable, so they use weaker isolation levels. We'll discuss Serializable in a future post.

# Read Committed

Read committed is the default isolation level for many databases. It makes two guarantees:

- A transaction can't read data that another transaction has written but not yet committed, preventing dirty reads.
- A transaction can't read data that hasn't been committed, preventing dirty reads.

## Dirty Reads

A dirty read occurs when a transaction reads data that another transaction has written but not yet committed. This can lead to incorrect results if the other transaction rolls back.

For example, consider two transactions where T1 writes a value of 1 and T2 reads that value. If T1 rolls back, T2 has read a value that doesn't exist. T1 may roll back if something goes wrong.

## Dirty Writes

A dirty write happens when a transaction writes data that another transaction has written but not yet committed. This can lead to lost updates if the other transaction rolls back.

For example, consider two transactions where T1 writes a value of 1 to a row and T2 writes a value of 2 to the same row. If T2 commits first, T1's write is lost.

It's a bit confusing, but what it means is that T1's write is lost because T2's write overwrites it before T1 commits (has finished).

## Implementing Read Committed

Many databases prevent dirty writes by using row-level rocks. When a transaction modifies a row, it locks the row until the transaction commits. This prevents other transactions from modifying the row until the first transaction commits.

How do we prevent dirty reads? You might think that we could use row-level locks too. This will work. However, it will prevent read-only transactions from reading the row until the first transaction commits. This is a problem because read-only transactions don't modify the data, so they should be able to read data that's already committed.

Because of this, most databases remember the old committed value and the new uncommitted value. If another transaction reads the row, it will receive the old committed value. This prevents dirty reads.

# Snapshot Isolation and Repeatable Read

## Non-Repeatable Reads

Read committed prevents dirty reads and writes, but it doesn't prevent read skew. Read skew occurs when two transactions read the same data, but one transaction modifies the data in between the two reads and commits.

Consider two transactions, T1 and T2. T1 reads a row, and T2 reads the same row. T1 modifies the row and commits. T2 reads the row again. If T2 reads the row's new value, it's read a different value than the first time it read the row. This is read skew. It happens because T1 modified the row and committed between T2's two reads.

A read skew is an example of a non-repeatable read. A non-repeatable read occurs when a transaction reads the same row twice but gets different values each time.

## Snapshot Isolation

Snapshot isolation is a stronger isolation level than read committed. It prevents read skew by using snapshots of the database at the start of each transaction. This way, a transaction reads the same data throughout its execution, even if other transactions modify the data and commit.

Snapshot isolation is great for long-running, read-only transactions such as analytics queries.

### Implementing Snapshot Isolation

Snapshot isolation also uses locks to prevent dirty writes. However, reads don't use locks. A key principle of snapshot isolation is that reads don't block writes, and writes don't block reads. This means a database can handle big reading tasks and changes at the same time without any problems.

To make snapshot isolation work, databases keep several versions of the same piece of data. This way, different changes in progress can each look at the data as it was at the right time for them. This method is called multi-version concurrency control (MVCC).

For just read committed, where you see only complete changes, a database keeps two versions: the final and the one still being changed. For snapshot isolation, which lets you work with a consistent view of data, databases often use a method where read committed gets a new view for each query, showing only finished changes.

Snapshot isolation, on the other hand, creates one consistent view at the start and uses it for all actions in that session.

When a transaction begins, it gets a unique, increasing ID. Anything written to the database by this transaction is marked with its ID. Each table row has a "created_by" field showing who added it and a "deleted_by" field that starts empty.

If a transaction tries to delete a row, the row isn't immediately removed. Instead, it's marked for deletion with the transaction's ID in the "deleted_by" field. Later, when no one needs the deleted data anymore, the database cleans up and removes these marked rows to make space.

## Visibility rules for observing a consistent snapshot

When a transaction reads data, it uses transaction IDs to figure out which data it can see. The database sets rules to make sure the application sees a stable version of the data. Here’s how it does it:

1. At the beginning of a transaction, the database notes down all other transactions that are still going on (not finished or cancelled). Any changes these transactions are making won’t be seen, even if they finish successfully later.

2. Changes made by transactions that didn’t complete successfully are ignored.

3. Changes made by transactions that started after this one are also ignored, no matter if they have finished.

4. All other changes can be seen by the application.

These rules apply to both creation and deletion of data.
