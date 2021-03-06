
# NodeJS

### Script execution
1. The driver will send a query to MongoDB when we call a cursor method passing a callback function to process the results e.g. forEach() or toArray().

2. The functions limit, skip and sort are cursor level methods that define the way in which the data is retrieved from MongoDB. 

### Driver Queries
When looking to execute an or statement the below is an example to follow:<br>
`query["$or"] = [`<br>
`    {"overview": {"$regex": options.overview, "$options": "i"}},`<br>
`    {"tag_list": {"$regex": options.overview, "$options": "i"}}`<br>
`];`<br>

`And` Statements are a little more straightforward:<br>
`query.founded_year = {"$gte": options.firstYear, "$lte": options.lastYear };`

mongoimport is a command that will take a JSON file as an input and load that data to a specified database and collection e.g.
>mongoimport -d _databaseName_ -c _collectionName_ _fileName_

Batching and reduced memory footprint - rather than casting a cursor to a complete array in a find, the result set can be batched and returned as required.

>insertOne()

>insertMany()


