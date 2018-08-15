# Indexes

## No Indexes
If you do a search on a collection without and index you will end up querying the entire collection. 

### Create index command
For a students collection, below creates an index key with the class and and student name fields:<br>
`db.students.createIndex( { "class" : 1, "student_name" : 1 } )`

#### Create a unique index
`db.students.createIndex( { "student_name" : 1 }, { unique : true } )`

### Retrieve indexes command
For a given collection students, below retrieves all associated indexes:<br>
`db.students.getIndexes()`

### Delete index command
For a students collection, below deletes an index:<br>
`db.students.dropIndex( { "class" : 1, "student_name" : 1 } )`

## Multikey Indexes
If you create an index using an array field, you create a *multikey* index. One restriction in the use of multikey indexes is that we can't have a compound index with 2 array fields - only one array field is permitted within a compound array.

If using MMAP, an index will use a BTree to store the indexes and speed up the searches.

