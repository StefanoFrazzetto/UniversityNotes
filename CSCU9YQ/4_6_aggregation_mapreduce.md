# Data aggregation

Aggregation consists in retrieving information through a number of operations in such a way so that a single results is returned. This allows to easily use data for statistical analysis or data visualization. MongoDB provides different methods to perform aggregation.

## Aggregation

Aggregation pipeline is a framework where documents are transformed as they go through each stage. It is the preferred method as it uses native operations, can work on sharded collections, and it is around 10 time faster compared to map-reduce. The syntax for the aggregate command is the following:

`db.collection.aggregate([<stage>, <stage>, ...])`

"Pipeline expressions \[stages] can only operate on the current document in the pipeline and cannot refer to data from other documents. \[...] The accumulators, used in the `$group` stage, maintain their state as documents go through the pipeline, while they _do not_ maintain their state when used in the `$project` stage." The information below has been extracted from the [official documentation](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/#aggregation-expressions).

Some of the most used stages are the following:

- `$facet`: processes multiple aggregation pipelines in a single stage on the same set of input documents.
- `$group`: groups input documents by the specified expression and applies the accumulator expression(s), if specified, to each group, then outputs one document for each group.
- `$limit`: passes the first _n_ documents unmodified to the pipeline.
- `$match`: filters the document stream to pass only documents matching the expression.
- `$project`: reshapes each document in the stream, such as by adding new fields or removing existing ones.
- `$sort`: reorders the documents by a specified key.
- `$skip`: skips the first _n_ documents and passes the remaining documents unmodified to the pipeline.
- `$unwind`: deconstructs an array field from the input documents to output a document for each field. The output document will have the same structure as the input document, but the "unwided" field will appear as a deconstructed array instead.

From the following `fruits` collection
```json
[
    { name: "apple", quantity: 5, price: 0.55 },
    { name: "pear", quantity: 2, price: 0.60 },
    { name: "kiwi", quantity: 20, price: 0.20 },
]
```
it is possible to fruits with a quantity higher than five through the expression
```javascript
db.fruits.aggregate([
    { $match: { quantity: { $gte: 5 } } },
    { $project: { name: 1 } }
])
```

To count the total quantity of fruits, it is possible to use
```javascript
db.fruits.aggregate([
     { $group: {total_quantity: {$sum: "$quantity"}, _id:null} }, 
     { $project: {total_quantity: 1} } 
     ])
```
which will return
`
{ "_id" : null, "total_quantity" : 27 }
`

In the group expression, `_id` is set to _null_ because we do not want to aggregate with respect to a key.

MongoDB also provides operations for commonly used commands:
- `count(<query>, <options>)`: uses metadata to run the count when a query is not specified, so its results might be inaccurate.
- `estimatedDocumentCount(<options>)`: uses metadata, so its results might be inaccurate.
- `countDocuments(<query>, <options>)`: wraps aggregation with a `$sum` expression to perform the count.
- `distinct(<field>, <query>, <options>)`: returns an array of documents that have distinct values for the specified field.

These operations can be also concatenated. Example:
`db.collection.find().count()` which is equivalent to `db.collection.count()`.

## Map-Reduce

Map-Reduce is a programming paradigm for condensing large quantities of data into aggregated results. In MongoDB, it is implemented through the `mapReduce` command where JavaScript functions are used to perform operations. On sharded collections, the router will dispatch the map-reduce operation in parallel to each of the shards.

As the name says, map-reduce is composed of two different functions, namely _map_ and _reduce_. MongoDB applies the map function to each input document matching the query condition. The function emits key-value pairs, then - for the keys having multiple values - the reduce function is applied to condense data. The output, which can optionally be passed to a _finalize_ function to further condense data or process it, is finally stored in a collection.

The base syntax for the operation is
```javascript
db.collection.mapReduce(
    map: <function>,
    reduce: <function>,
    {
        query: <document>,
        output: <output>
    }
)
```

In the function, the keyword `this` refers to the document currently being processed. 

The map function produces new documents by calling the `emit` function which takes two parameters. Example:
` var mapFunction = function() { emit(this.name, this.quantity); };`

The reduce function takes two arguments, namely the key and the value. It can only access the variables defined in the scope of the function.

To retrieve the quantity of fruits from the "fruits" collection
```javascript
db.fruits.find()
{ "_id" : ObjectId("5cbb6e8d8cda248ccada3127"), "name" : "apple", "quantity" : 5, "price" : 0.55 }
{ "_id" : ObjectId("5cbb6e8d8cda248ccada3128"), "name" : "pear", "quantity" : 2, "price" : 0.6 }
{ "_id" : ObjectId("5cbb6e8d8cda248ccada3129"), "name" : "kiwi", "quantity" : 20, "price" : 0.2 }
```

and store the resulting document in a collection named "fruit_quantities", the following query can be used
```javascript
db.fruits.mapReduce(
    function(){emit(this.name, this.quantity);}, 
    function(key, value) { return value;}, 
    {out: "fruit_quantities"}
)
```

The result can be seen below
```javascript
db.fruit_quantities.find()
{ "_id" : "apple", "value" : 5 }
{ "_id" : "kiwi", "value" : 20 }
{ "_id" : "pear", "value" : 2 }
```