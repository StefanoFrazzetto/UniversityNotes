# NoSQL databases

NoSQL database are commonly chosen because of the flexibility they offer over traditional relational databases. The common characteristics of NoSQL databases are:
- Lack of relational model, resulting in a flexible structure
- Particularly useful for big data applications
- Easily scalable

## Data models

Depending on the data model adopted by databases, they fall into four different categories:
- Key-value stores
- Wide-column stores
- Document databases
- Graph databases

**Key-value** are the simplest and associate each key to a value. The most known databases falling into this category are Redis, Riak, and Voldemort.

**Wide-column** store values into columns instead of rows and are particularly efficient for querying over large datasets. The most popular are Cassandra and HBase.

**Document-oriented** databases associate each key with complex data, known as document. The most popular databases of this type are MongoDB, CouchDB, ElasticSearch

**Graph databases** store data in graph structures to model data as a network of relationships. The most know database of this kind is Neo4J. 

Information from [mongoDB](https://www.mongodb.com/scale/types-of-nosql-databases).

### Embedded vs normalized

MongoDB allows to use both embedded and normalized data models. When using the former, all the data is stored in single self-contained documents. With the latter model, documents may reference other documents which contain other information. The last approach is useful when embedding documents would result in complex many-to-many relationships, or significant data duplication without any performance advantages.

## Scaling

As previously mentioned, NoSQL database are chosen because of their speed compared to relational ones, but also because of the ease of scaling they provide. Scaling can be either vertical or horizontal. The former consists in increasing the hardware resources for the machine were the server resides, whereas the latter consists in spreading the data across multiple machines, which can be done through replication and/or sharding. Replication and sharding are often combined together in MongoDB and use a voting system to elect replicas from secondary to primary, and to confirm write operations across replicas in sharded clusters.

### Replication

Replication is obtained by creating identical copies of data which are then spread across a number of machines.

#### Master-slave

The process can be executed either synchronously or asynchronously when using a master-slave model. In the former case all replicas are updated on every write, which as the advantage of keeping data always up-to-date at the expense of write speed. This is known as **strict consistency**.

Asynchronous replication has the advantage of providing higher write speed, though replicas might not be always up-to-date since changes have to be propagated from a master, or primary, replica first. This is known as **eventual consistency**.

#### Peer-to-peer

Since the master replica could be a cause for bottlenecks, a peer-to-peer replication can be used, so that all nodes can accept data changes and spread them across the network of nodes. This approach has the advantage of removing bottlenecks, though it could be a source of inconsistencies, especially when different peers receive conflicting updates.

### Sharding

Sharding consists in having a set of data divided across multiple servers at the same time. This allows to distribute the load across different machines. A sharding key is used to determine where data will be split on a separate machine. To achieve a more even distribution of data, a hashed key is often used instead of values from collections. In MongoDB, sharding is performed at collection level, so that a cluster can contain both sharded and non-sharded collections. Since applications never directly communicate with shards, `mongos` -- a router for MongoDB -- is used to route queries and write operations to shards.