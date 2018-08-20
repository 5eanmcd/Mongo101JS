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
A covered query is one which is satisfied entirely with an index and hence zero documents are required to be inspected.
*It may be likely that you have to project out fields in order to ensure Mongo uses a covered query - if there is anu possibility of having to present a value for a key thats not in the index, then Mongo won't just rely on the index and will result in documents being inspected.*
If using MMAP, an index will use a BTree to store the indexes and speed up the searches.

