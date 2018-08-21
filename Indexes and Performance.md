# Indexes

## No Indexes
If you do a search on a collection without and index you will end up querying the entire collection. 

### Create index command
For a students collection, below creates an index key with the class and and student name fields:<br>
`db.students.createIndex( { "class" : 1, "student_name" : 1 } )`

#### Create a unique index
The below command creates a unique index for the students collection, it means that the student collection cannot have multiple documents with the same value for student_name.<br>
`db.students.createIndex( { "student_name" : 1 }, { unique : true } )`

### Retrieve indexes command
For a given collection students, below retrieves all associated indexes:<br>
`db.students.getIndexes()`

### Delete index command
For a students collection, below deletes an index:<br>
`db.students.dropIndex( { "class" : 1, "student_name" : 1 } )`

## Multikey Indexes
If you create an index using an array field, you create a *multikey* index. One restriction in the use of multikey indexes is that we can't have a compound index with 2 array fields - only one array field is permitted within a compound array.

## Sparse indexes
If you want to create an index on a collection when the index doesn't exist for certain documents, then you need to create a sparse index. Those documents without this indexed field will not form part of the collection index.<br>
`db.students.createIndex( { "student_phone" : 1 }, { unique : true, sparse: true } )`

*Note:* Sparse indexes are not used is you wish to sort on that field - rather than an index scan being used a collection scan will be used to read the data.

## Foreground and Background index collection creation.
The default foreground index collection creation will block all reads and writes to the collection. It is however faster than background index creation. Background index creation is slower, however it doesn't block read / write activity. An alternative to running background index creation (if you have concerns about the performance) is to take one of the DB servers in a replica set and update individually before returning to the replica set.

## Explain
To determine how your query/update will execute you can run an explain command as below:<br>
`db.students.explain().find({ student_name: "Sean"})`

## Covered Queries
A covered query is one which is satisfied entirely with an index and hence zero documents are required to be inspected.<br>
*It may be likely that you have to project out fields in order to ensure Mongo uses a covered query - if there is anu possibility of having to present a value for a key thats not in the index, then Mongo won't just rely on the index and will result in documents being inspected.*

## Cache - query plans
One Mongo has selected the winning plan for a given query or query pattern, the winning query is stored in the cache for future use. The cache will be updated if there are 1000 writes, index is rebuilt, indexes are added/dropped or finally if the mongod process is restarted.

## Indexes and Storage Engines

### MMAP
If using MMAP, an index will use a BTree to store the indexes and speed up the searches.
### WiredTiger
WiredTiger suppors a few different types of compression, one of which called prefix suppression allows us to have smaller indexes.

## Number of Index Entries
Be aware there is a cost to maintaining indexes. 

|                |Number of Indexes|
|----------------|--------------|
|Regular|1:1|
|Sparse|<=numberOfDocuments|
|Multikey|>numberOfDocuments|

## Geospatial indexes
Indexes of this type `2d` allow for a *$near* fuction to be executed to identify locations nearest a given point.

## Geospherical indexes
Indexes of this type allow for Mongo to return documents based on their lat long values. The query below for example returns documents near the lat long -130, 391. Results are limited to those within 1,000 kms of this point.<br>
`db.stores.find({ loc:{ $near: { $geometry: { type: "Point", coordinates: [-130, 39]}, $maxDistance:1000000 } } })`

## Text indexes

Using an index type of text as below will allow for word search queries to be executed: <br>
`db.mycollection.ensureIndex({ 'fieldname' : 'text' })`
The query below will now search the index for and text fields with the word dog.<br>
`db.mycollection.find({ $text : {$search: "dog" } })`

