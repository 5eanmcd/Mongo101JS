
# CRUD

## Create operations:
The are 2 specific commands to insert a document to a database collection:
    * insert()
    * insertMany()
    When using insertMany(), if specified ordered: false will see Mongo attempt to insert as many documents as possible.

Update commands can/may result in inserts, these are known as upserts.

## ID 

MongoDB Document id is comprised of the following elements in a 12 byte hex string:

    Date|MAC Addr|PID|Counter<br>
    xxxx|xxx|xx|xx

## Read operations:

    A typical find command appears as below:<br>
>db.<\<databaseName>>.find({})

    The find method accepts a document to match against, in this example it will match all documents. <br>
    Here the below command will match documents that have a property "rated" equal to 13.
>db.movieDetails.find({ "rated" : "PG-13" }).pretty()

    <u>Exact Array Matches</u>

    If you wish to match an array against a set of exact values
>db.movieDetails.find({ "writers": ["Ethan Coen", "Joel Coen"]})

    Moving, or re-ordering the search criteria will have different results.

### Specific Array Match
>db.movieDetails.find({ "writers": "Ethan Coen" })

### Specific Array Position Match
>db.movieDetails.find({ "writers.0": "Ethan Coen" })

### Additional Topics

1. Cursors<br>
    Within the _mongo_ shell If a find command isn't assigned to a var the when the find is executed to returned cursor is iterated for up to 20 times.
    Generally returned documents are provided in batches, with batches by default not exceeding 101 documents or not exceeding 1MB. Subsequent batches will be up to 4MB.
2. Projections<br>
    Projections reduce the size of the data returned from a query. They reduce network overhead and processing requirements. The below example returns just the title (and the _id field)
>db.movieDetails.find({ "rated" : "PG-13"}, { "title" : 1})

    The _id field is always returned unless explicitly excluded similar to below:
>db.movieDetails.find({ "rated" : "PG-13"}, { "title" : 1, "_id" : 0})

## Comparison Operators

See https://docs.mongodb.com/manual/reference/operator/query/

    An example of a conditional operator (greater than) below:
>db.movieDetails.find({"runTime" : { $gt: 90 } })

    A range query is built as below (greater than or equal to AND less than or equal to):
>db.movieDetails.find({"runTime" : { $gte: 90, $lte: 110 } })

    A query where a string is equal to a range of values:
>db.movieDetails.find({"rated" : { $in : [ "G", "PG", "PG-13"] } })

## Element Operators

* $exists
>db.movieDetails.find({"tomato.meter" : { $exists : true }})
    
* $type
>db.movieDetails.find({"_id" : { $type: "string" }})

## Logical Operators

* $or
>db.movieDetails.find({ $or : [ {"tomato.mater" : { $gt: 95 }}, { "metacritic" : { $gt: 88 }}
    ]})

* $and
>db.movieDetails.find({ $or : [ {"metacritic" : { $exists: true }}, { "metacritic" : { $ne: null }}
    ]})

This command can be superfluous in queries as the expected document matches can just be listed. The command does however allows for multiple constraints on the same field.

## Regex Operator

To allow for a query match based on a regex expression
>db.movieDetails.find( { "awards.text" : { $regex : /^Won\s.*/ } })

## Array Operators

* $all <br>
    This command ensures that the returned data matches all the fields specified in the query document
>db.movieDetails.find({ "genres" : $all [ "Comedy", "Crime", "Drama"] })

* $elemMatch
    This query matches elements in an array where all conditions are met
>db.movieDetails.find({ "boxOffice" : { $elemMatch : { "country" : "UK", "revenue" : { $gt: 15 } } } })

* $size <br>
    This command allow for queries to return data based on the size of an array
>db.movieDetails.find({ "countries" : { $size: 1} })

## Updating Documents

* updateOne()

    This method allows for a single document to be updated
>db.movieDetails.updateOne({ "title": "The Martian"}, { $set : { "poster" : "http://poster.jpg" } })

This will update the FIRST document that matches this query. The response to that update statement will be in the form:

```javascript
{ "acknowledged" : true, "matchedCount": 1, "modifiedCount": 1 }
```

The following paramater allows us to increment a value(s) as part of an update statement:

>db.movieDetails.updateOne({ "title": "The Martian"}, { $inc : { "tomato.reviews" : 3, "tomato.userReviews": 25 } })

Array update operators include: $pop, $addToSet, $pull
-> These will require further training.

* updateMany()

    Similar to updateOne(), this method however will update all documents that match the search criteria.

>db.movieDetails.updateMany( { "rated" : null }, { $unset: { "rated" : "" } } )

In the above example the rated element will be removed from documents where the value of this field is null.

* Upsert

Similar to an insert, upsert allows for documents to be created. See example:

>db.movieDetails.updateOne(<br>
    { "imdb.id" : object.imdb.id }, -> matches the imdb.id against the object we are using to insert<br>
    { $set : detail }, -> we set the detail property of the movieDetails doc<br>
    { $upsert: true } -> this setting means that in the event the imdb.id is not matched that we will create a new document with the object's data
    )

* replaceOne()
>db.movieDetails.replaceOne(<br>
        { "imdb.id" : object.imdb.id }, -> matches the imdb.id against the object we are using to insert<br>
        detail -> we set the detail property <br>
    )