# MongoDB

MongoDB is a document-based open-source database which provides high performance and reliability. It stores data in JSON-like documents in a binary-encoded format called BSON to provide additional functionalities, such as more data types and ordering.

## Documents

Each document stored in the database has a unique \_id field, automatically generated, used as a primary key. Documents are stored in collections, analogously to tables in relational database, which are then stored in databases.

## Availability

Through replica set, MongoDB provides an automatic failover system and data redundancy. "A replica set is a group of MongoDB servers that maintain the same data set". Each replica set consists of two or more copies of the data, so that replicas act as primary or secondary at any time. This allows the system to replace failing replicas with a secondary one being elected to primary. Secondary replicas can optionally serve read operations.

## Scalability

Horizontal scalability is provided through sharding and data zones. Sharding is the process of distributing data across a cluster of machines based on shard keys. This allows to distribute the system load over multiple servers and to increase availability, though increasing the infrastructure complexity.

## Shell

The mongo shell is an interactive interface that allows to interact with MongoDB by using JavaScript syntax. A rich query language is used to perform CRUD operations as well as text search, geospatial queries, and data aggregation.

### Creating a database

To select an existing database, the command `use databaseName` is used. If the database does not exist, a new one is automatically created when data is first stored in it.

### Creating collections

To explicitly create a collection, it is possible to use the command `db.createCollection()`. Similarly to the database creation process, if a collection does not exist, mongo automatically creates it when data is first stored in it.

### Create

It is possible to insert documents either one-by-one or many at once. This is achieved respectively through the methods `insertOne` and `insertMany` as follows:

`db.collectionName.insertOne(<element>)`

`db.books.insertOne(
    {
        name: "Benjamin Franklin Autobiography", type: "book", quantity: 1, authors: ["Benjamin Franklin"]
    }
)`

The `insertMany` works in the same way, but more elements are passed to the method instead.

### Read

The `find` method allows to retrieve documents from collections. Filters, i.e. queries, can be specified to restrict the returned results based on the specified parameters, while projections allow to specify the fields returned for the data being queried. Additionally, a cursor modifier allows to iterate through the elements being returned.

The syntax for the command is the following:
`db.collectionName.find(<query>,<projection>)`

The following query will return all the elements matching the specified filter
`db.books.find({name: "Benjamin Franklin Autobiography"})`

as

`{
	_id: "somerandomid123",
	name: "Benjamin Franklin Autobiography", 
    type: "book", 
    quantity: 1, 
    authors: ["Benjamin Franklin"]
}`

If we were to return only the quantity, the projection could be used

`db.books.find({name: "Benjamin Franklin Autobiography"}, {quantity: 1})`

so that the result would be
`{
	_id: "somerandomid123",
    quantity: 1
}`

The `_id` field can be removed from the results by setting its value in the projection to 0.

### Update

To modify existing elements it is possible to use either the update methods or the replace one. The difference between `updateOne` and `updateMany`, and `replaceOne` is that the latter completely replaces a document with a new one, while the former are used to update the value(s) of a document without replacing it.

The syntax for the commands is
`
db.collection.updateOne(<filter>, <update>, <options>)
db.collection.updateMany(<filter>, <update>, <options>)
db.collection.replaceOne(<filter>, <update>, <options>)
`

A sample update query update the book quantity could be written as follows
`
db.books.updateOne(
{ name: "Benjamin Franklin Autobiography" },
{
	$set: { quantity: 2 }
}
)
`

### Delete

To delete one or many records, it is possible to use respectively `deleteOne` or `deleteMany`. The syntax is the same for both methods and, to delete all documents from a collection, it will be sufficient to pass an empty filter to them.

To delete a single document, it is possible to use the query
`
db.collection.deleteOne(<filter>)
`

Specifically, to remove our example book, we can use
`
db.books.deleteOne(
{ name: "Benjamin Franklin Autobiography" }
)
`

which will result in the book with the specified name being deleted.