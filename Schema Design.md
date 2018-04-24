# Schema Design

In MongoDB we attempt to describe the data in a way thats its used by the application. Because that's the way we will achieve the best performance and will assist in making application programming much more straightforward.

*Note:* It's important to remember there is no support for transactions in MongoDB, however we do have _atomic operations_.

## Atomic Operations
This means that when you work on a single document, that the work will be completed before anyone else sees the document ie. they see *all* the changes you make or *none*.

## How to we build for transaction-like capabilities?
1. Restructure<br>
    Restructure your code so that you are working with a single document and taking advantage of the atomic operations that we offer within that document.
2. Implement in software<br>
    Some sort of locking could be implemented, e.g. critical section
3. Tolerate<br>
    Allow for some level of inconsistency within the application.

## Seperate collections
It might make sense to have keep documents in a seperate collection even if the documents have a one-to-one relationship if:
* we are looking to reduce the working set size of the application.
* because the combined size of the documents would be larger than 16MB

## One to Many relationships
When we have a genuine one to many relationship e.g. person to city, true linking would appear to the right design decision.
```java
//person collection
{
    "name": "Andrew",
    "city": "NYC"
}

//city collection
{
    "_id": "NYC",
    ...
}
```

## Benefits of embedding
* Improved read performance.
* Once round trip to the DB.

