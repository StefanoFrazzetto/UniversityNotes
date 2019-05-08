# Transactions

A database transaction represents a unit of work that should be carried out as a whole. They allow database to recover from failure keeping it in a consistent state, and provide isolation between processes accessing it at the same time, so that data is always consistent.

## ACID

Transactions have properties defined with the acronym ACID:

- **Atomicity**: all changes are made at the same time, or not made at all.
- **Consistency**: the change happens only if the system state will be valid after it.
- **Isolation**: the execution concurrent transactions results in the same state that would result from executing them sequentially. 
- **Durability**: once a transaction is complete, it will remain committed even in case of system failure.

In MongoDB, an operation on a single document is atomic. Since it's possible to embed documents to capture relationships between data, there is no need to use transactions when only a single document is involved. For situations where atomic updates have to be carried out for multiple documents, e.g. on normalized schemas, this is achieved through transactions.

By default all reads operations in MongoDB are directed to the primary replica, though this can be changed for different use cases, such as on systems where they do not affect front-end applications, on geographically distributed systems, or on systems that need to keep working in case of failure of the primary replica. Reading from a secondary replica is discouraged since data has to be propagated from the primary first, so this can result in stale data.

## CAP theorem

The CAP theorem, also known as Brewer's theorem, states that it's impossible for a distributed data store system to have more than two of the following properties:

- Consistency: all clients always have the same view of the data.
- Availability: each client can always read and write data.
- Partition tolerance: the system can operate despite network failures between clusters.

"Ensuring reads never return data that is not in the same causal order as the primary replica, **MongoDB blocks readers while oplog entries are applied in batches to the secondary**. This can cause secondary reads to have variable latency, especially when bulk loading new data. Why does MongoDB do this? When applying a sequence of writes to a document, then MongoDB is designed so that each of the replicas must show those writes in the same causal order. So if you change field "A" and then field "B", it isn’t possible to see that document with changed field "B" and not changed field "A". **Eventually consistent systems suffer from this behavior, but MongoDB does not.**" -MongoDB Docs

"MongoDB is strongly consistent by default - if you do a write and then do a read, assuming the write was successful you will always be able to read the result of the write you just read. This is because MongoDB is a single-master system and all reads go to the primary by default. If you optionally enable reading from the secondaries then MongoDB becomes eventually consistent where it's possible to read out-of-date results." -StackOverflow

## Running transactions

A number of operations are allowed in transactions, including aggregate. Because read operations inside transactions can return stale data, the method `findOneAndUpdate` can be used since the transaction aborts if the document is modified from another session during the process.
