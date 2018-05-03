# Data In/Out

### mongodump

The command can take a number of arguments, but the default as below is to dump all user DBs.
>`mongodump`

If you wish to dump a specific db
>`mongodump --db <dbName>`

If you wish to dump a specific collection within a specific db
>`mongodump --db <dbName> --collection <collectionName>`

Note the admin DB is not backed up, unless explicitly stated. Important as this is where all privileges and accounts are held.

mongodump requires a running instance of mongo[s] - either `mongod` (daemon) or `mongos` (shared cluster setup).

### mongorestore

Allows us to restore DB state based on a restore(dump) file, the default is to do a complete sytem restore.

>`--drop`
Tells the store to drop the target collection before attempting the restore e.g.
>mongorestore --dump backup\dump

Restore specific collection
>`mongorestore --drop --collection <collectionName> --db <dbName> backup\dump\<dbName>\<collectionName>.bson`

We can restore into a new DB to avoid impacting an existing DB. 
>`mongorestore --db newRestoredDB backup\dump\oldDB`

During a backup a number of writes/commits may have occurred, these will have been written to the oplog file, if we wish to to a restore than includes those commits that took place during a backup then we will need to use --oplogReplay.
>mongorestore --oplogReplay --drop dump


### mongoimport
3 file types supported currently 
* .csv (comma seperated)
* .json
* .tsv (tab seperated)

A sample mongoimport command will look as follows:
>mongoimport --db <databaseName> --collection <collectionName> <fileName>.json

#### `--upsert`
In the event that our DB already has an entry for the records in our file, if the file has updates(or contains the most recent/correct data) we can use the `upsert` keyword to instruct that the file is used to update the DB information.
>`mongoimport --db <databaseName> --collection <collectionName> --upsert <fileName>.json `


