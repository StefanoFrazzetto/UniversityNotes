# Indexes

Indexes allow to execute queries more efficiently. Without them the database has to scan every document in the collection, to find the documents matching the search query. In MongDB, indexes are implemented to B-trees.

## Performance considerations

If a find operation is run to match an indexed element, the database will return results quickly because only the indexes will be scanned.

As indexes consume disk space and memory, particular attention has to be paid when creating them. In applications with a high write-to-read ratio, they may have a negative impact on performance since each insert has to also update the indexes. "Collections with high read-to-write ration often benefit from additional indexes."

For small documents, it may be possible to explicitly assign the otherwise automatically generated `_id` field; since this field is automatically indexed, this allows to save space, though it must be unique for each document as the primary key cannot have collisions in the collection.

Although it's possible to used different types of indexes, including, but not limited to, compound (multi-key) fields, text, geospatial, to obtain evenly distributed shards, it is preferable to make use of hashed indexes.

## Properties

Indexes can have the following properties:

- **Unique**: will cause MongoDB to reject duplicates.
- **Partial**: only the documents that meet a specified filter expression are indexed.
- **Sparse**: only documents that have the indexed field will be indexed.
- **TTL**: documents will be automatically removed from a collection after a certain amount of time.