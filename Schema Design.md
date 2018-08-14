# Schema Design

In MongoDB we attempt to describe the data in a way thats its used by the application. Because that's the way we will achieve the best performance and will assist in making application programming much more straightforward.

*Note:* It's important to remember there is no support for transactions in MongoDB, however we do have _atomic operations_.

## Living without constraints
MongoDB doesn't allow for foreign key constraints, so if an application requires that data is held in 2 separate tables then this behavior must be guaranteed by developers.

## Living without transactions

** <b>Transactions now supported with v4.0</b>

### Atomic Operations
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

## One to One relationships
Entities can be linked across tables programmatically, e.g. an employee may have one resume, one resume may have one employee. Or we can embed an entity within another document e.g. a resume within an employee.

Decisions to be made in terms of schema may be based on:
1. Frequency of use:
* How often do we need to view the resume information? 
2. Size of entities:
* How large is the resume entity associated with an employee?
3. Frequency of updates:
* Is one entity likely to face a lot of updates? More that another? (If so you may not wish to embed).
4. Consistency of the data:
If we can't tolerate any inconsistency in the data? So the atomicity includes both entities, you may consider embedding within one document.

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

## Multikey Indexing
By providing a new index on a table, we enable multikey indexing on the table. We can identify when multikey indexes are used with the explain() function.
