---
author : "Sumit"
title : "Database Isolation Levels"
date : "2023-07-05"
description : "Analysis of diffeent isolation levels in databases"
tags : [
    "database",
]
summary: "Analysis of diffeent isolation levels in databases"
published : true
---

Recently, I have been learning about a relatively new SQL database [CockroachDB](https://www.cockroachlabs.com/). There is an interesting choice thay they have made regarding the isolation provided by the database. In this post, I will explain what database isolation levels are, why they are important, and how they affect the performance and correctness of concurrent transactions. I will also describe each of the four standard isolation levels defined by SQL, and discuss their pros and cons.

## What is database isolation?

Database isolation refers to the ability of a database to allow a transaction to execute as if there are no other concurrently running transactions (even though in reality there can be a large number of concurrently running transactions). The overarching goal is to prevent anomalies that can occur as a result of inter-leaving transactions
and can possibly corrupt the database.

## What are database isolation levels?

In ACID, __I__ stands for Isolation, i.e. multiple concurrent transactions do not interfere with each other. But the reality is not as simple. Such isolation comes at a cost of throughput. Different applications have different requirements, and based on that databases offer different levels of isolations.

Database isolation levels are used to define the level of isolation from other transactions that a transaction can achieve when accessing data. The more isolated a transaction is, the less likely it will interfere with other transactions operating on the same data at the same time.

Isolation is important because it ensures that transactions do not interfere with each other in an undesirable way. For example, if two transactions are trying to update the same data at the same time, one of them might overwrite the changes made by the other, resulting in data inconsistency or loss.

Similarly, if one transaction reads data that another transaction has not yet committed, it might see incorrect or incomplete data, leading to errors or incorrect results. To prevent these problems, database systems use various mechanisms to control the concurrency of transactions, such as locking, timestamps, snapshots, or multiversioning.

However, achieving perfect isolation is not always possible or desirable, because it comes at a cost of performance and scalability. For example, locking a data item for a long time might prevent other transactions from accessing it, causing delays or deadlocks.

Executing the transactions in a serial manner will provide the highest level of isolation, but at the same time will provide very poor throughput. Therefore, database systems allow users to choose different levels of isolation for their transactions, depending on their needs and preferences. The SQL standard defines four levels of isolation: ```read uncommitted, read committed, repeatable read, and serializable```. Each level has different guarantees and trade-offs regarding the visibility and effects of concurrent transactions.

## Factors deciding Isolation levels

The four standard isolation levels are defined by the following phenomena that might occur when a transaction reads data that another transaction might have updated:

- __Dirty read:__ A dirty read occurs when a transaction reads data that has not yet been committed by another transaction. For example, if transaction T1 updates a row and leaves it uncommitted, and transaction T2 reads the same row before T1 commits or rolls back, then T2 has performed a dirty read. If T1 rolls back the change, T2 will have read data that is considered never to have existed.
- __Non-repeatable read:__ A non-repeatable read occurs when a transaction reads the same row twice and gets a different value each time. For example, if transaction T1 reads a row, and then transaction T2 updates or deletes the same row and commits, and then T1 reads the same row again, then T1 has performed a non-repeatable read. The value that T1 read the first time is no longer valid.
- __Phantom read:__ A phantom read occurs when two identical queries are executed by a transaction, but the rows retrieved by the two queries are different. For example, if transaction T1 retrieves a set of rows that satisfy some search criteria, and then transaction T2 inserts or deletes some rows that match the same criteria and commits, and then T1 executes the same query again,
then T1 has performed a phantom read. The set of rows that T1 retrieved the first time is no longer valid.

## What are the four standard isolation levels?

Based on above phenomenas, the four defined isolation levels are:
![Database Isolation levels and anomalies](/images/blogs/database_isolation_levels/TrasactionsIsolationsLevels.svg)

- __Read uncommitted__: This is the lowest isolation level. In this level, a transaction may read uncommitted changes made by other transactions,
thereby allowing dirty reads. This level does not prevent any of the concurrency problems mentioned above,
but it provides the highest level of performance and concurrency,
as transactions do not need to acquire any locks on the data they access.
- __Read committed__: This isolation level guarantees that any data read by a transaction has been committed at the moment it is read.
Thus it prevents dirty reads,
but it does not prevent non-repeatable reads or phantom reads.
This level is achieved by holding a shared lock on the data item until it is read,
and then releasing it immediately after reading.
This allows other transactions to update or delete the data item after it has been read by another transaction.
- __Repeatable read__: This isolation level guarantees that any data read by a transaction will not change during the duration of the transaction.
Thus it prevents dirty reads and non-repeatable reads,
but it does not prevent phantom reads.
This level is achieved by holding a shared lock on the data item until the end of the transaction,
preventing other transactions from updating or deleting it.
However,
other transactions can still insert new rows that match the search criteria of another transaction,
causing phantom reads.
- __Serializable__: This is the highest isolation level. A serializable execution is equivalent to a serial execution, where transactions are executed one after another, without any concurrency. Thus it prevents all the concurrency problems mentioned above, but it also requires the most system resources and reduces the concurrency and scalability of the system. This level is achieved by using various techniques, such as locking, timestamps, or snapshot isolation, to ensure that transactions do not conflict with each other.

## What are the pros and cons of each isolation level?

Each isolation level has its own advantages and disadvantages, depending on the application requirements and expectations. Some of the most general pros and cons of each level are:

- __Read uncommitted__: The main advantage of this level is that it provides the highest performance and concurrency, as transactions do not need to wait for locks or check for conflicts. On the other hand, it allows dirty reads, which might cause errors or inconsistencies in the application logic or output. This level is mostly used for applications that require high throughput and can tolerate some inconsistencies, like data analytics or reporting.
- __Read committed__: On this level, the case of dirty reads is no longer a concern. Transactions only read data that has already been commited. This level is used by many applications and many databases like postgres operates on this level by default. But it still suffers from non-repeatable reads and phantom reads, which could compromise the integrity of the application data. This level is suitable for applications that can tolerate some degree of inconsistency, such as online shopping search results or social media.
- __Repeatable read__: The main advantage of this level is that it prevents dirty reads and non-repeatable reads, which improves data accuracy and consistency. The main disadvantage is that it allows phantom reads, which might cause errors or inconsistencies in the application logic or output. This level is suitable for applications that need a consistent view of the data, such as banking or accounting.
- __Serializable__: This is the strongest isolation guarantee provided by any database. All the transactions can be serialized as if they happened serially, and no anomalies occur at this level. The main disadvantage is that it reduces performance and concurrency, as transactions need to wait for locks or check for conflicts. This level is suitable for applications that need the highest level of data integrity and correctness, such as financial transactions or inventory management.

## Conclusion

In this blog post, I have explained what database isolation levels are, why they are important, and how they affect the performance and correctness of concurrent transactions. We also went through the four standard isolation levels defined by SQL, and discussed their pros and cons. 

For further reading, this [article by faunadb](https://fauna.com/blog/introduction-to-transaction-isolation-levels) explains in detail how the isolation levels as defined by SQL should be comprehended by us.
