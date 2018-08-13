
# NodeJS

### Script execution
The driver will send a query to MongoDB when we call a cursor method passing a callback function to process the results.

mongoimport is a command that will take a JSON file as an input and load that data to a specified database and collection e.g.
>mongoimport -d _databaseName_ -c _collectionName_ _fileName_

Batching and reduced memory footprint - rather than casting a cursor to a complete array in a find, the result set can be batched and returned as required.

>insertOne()

>insertMany()


