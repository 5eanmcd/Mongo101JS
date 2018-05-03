# Monitoring

## Log File
`mongod.log` is our log file and when Mongo feels something is off it will write details to this file.
Some commands that might be useful on windows would be `more` and `findstr`, in linux use `tail` and `grep`. From within the `mongo` shell, the command `show logs` and `show log global`

### Logging levels
The levels are 0-5, 0 being very quiet and 5 being the most verbose. The default logging level is 0.

### Log file format
`<timestamp> <severity> <component> [<context>] <message>`

### Topics
Logging levels can be set for any type of component, below is the list of topics available see [Log Messages](https://docs.mongodb.com/manual/reference/log-messages/#components) for more details.

`ACCESS` - Messages related to access control, such as authentication. 
`COMMAND` - Messages related to  [database commands](https://docs.mongodb.com/manual/reference/command/), such as  [`count`](https://docs.mongodb.com/manual/reference/command/count/#dbcmd.count "count"). 
`CONTROL` - Messages related to control activities, such as initialization. 
`FTDC` - Messages related to the diagnostic data collection mechanism, such as server statistics and status messages. 
`GEO` - Messages related to the parsing of geospatial shapes, such as verifying the GeoJSON shapes. 
`INDEX` - Messages related to indexing operations, such as creating indexes. 
`NETWORK` - Messages related to network activities, such as accepting connections. 
`QUERY` - Messages related to queries, including query planner activities. 
`REPL` - Messages related to replica sets, such as initial sync, heartbeats, steady state replication, and rollback. 
`REPL_HB` - Messages related specifically to replica set heartbeats. 
`ROLLBACK` - Messages related to  [rollback](https://docs.mongodb.com/manual/core/replica-set-rollbacks/#replica-set-rollbacks)  operations. 
`SHARDING` - Messages related to sharding activities, such as the startup of the  [`mongos`].
`STORAGE` - Messages related to storage activities, such as processes involved in the  [`fsync`](https://docs.mongodb.com/manual/reference/command/fsync/#dbcmd.fsync "fsync")  command. 
[`STORAGE`](https://docs.mongodb.com/manual/reference/log-messages/#STORAGE "STORAGE")  is the parent component of  [`JOURNAL`](https://docs.mongodb.com/manual/reference/log-messages/#JOURNAL "JOURNAL"). 
`JOURNAL` - Messages related specifically to journaling activities. 
`WRITE` - Messages related to write operations, such as [`update`](https://docs.mongodb.com/manual/reference/command/update/#dbcmd.update "update") commands.

## Query Profiler

We can use to query profiler to identify long running queries. There are 3 profiling levels (0,1,2).
* 0 means no profiling data is collected.
* 1 means only queries that exceed a defined threshold are logged.
* 2 means all operations are logged.

## mongostat
We need a baseline view of the DB and watch for things which are out of the ordinary.
>mongostat --host `<hostName>` --port `<portNumber>` --rowcount `<number>`

This will return data in a format similar to below:
|insert|query|update|delete|getmore|command|dirty|used|flushes|vsize|res|qrw|arw|net_in|net_out|conn|time|
|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|-|
|991|*0|*0|*0|0|2/0|3.4%|4.5%|0|2.90G|297M|0/0|0/0|12.9m|84.2k|2|Oct  6 09:45:37.478|
|989|*0|*0|*0|0|2/0|3.6%|4.7%|0|2.91G|310M|0/0|0/0|12.9m|84.1k|2|Oct  6 09:45:38.476|
|988|*0|*0|*0|0|1/0|3.7%|4.8%|0|2.92G|323M|0/0|0/0|12.8m|83.8k|2|Oct  6 09:45:39.481|

**Table values explained**
`insert`|`query`|`update`|`delete` - refers to the number of operations of this type per second.
`res` - when a query returns many documents and they do not fit in the first batch of the cursor, then the application will be issuing a `getmore`. If many of the queries result in this behavior you will see a low count of queries with a higher count of getmores.
`flushes` - the number of disk flushes per second. High flush rates mean more I/O pressure.
`res` - the number of Megabytes of resident memory.
``

## mongotop
This command will give a sense of where mongo spends most of its time, time spent doing reads/writes in the most active collections. It will let you know the most active collections and helps focus attention to those. 
*Note:* `mongotop` continuously polls and display values.

>mongotop --host `<hostname>` --port `<portNumber>`

## db.stats()
Helps to details disk and memory usage estimates.

## db.serverStatus()
## MMS
