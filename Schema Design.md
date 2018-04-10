# Schema Design

In MongoDB we attempt to describe the data in a way thats its used by the application. Because that's the way we will achieve the best performance and will assist in making application programming much more straightforward.

*Note:* It's important to remember there is no support for transactions in MongoDB, however we do have _atomic operations_.

## Atomic Operations
This means that when you work on a single document, that the work will be completed before anyone else sees the document ie. they see *all* the changes you make or *none*.