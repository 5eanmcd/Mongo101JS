#Indexes

##New in Mongo 3.0 
Pluggable storage engines

```mermaid
graph LR
X((Node Driver)) --> A
A((MongoDB Server)) --> D{Storage Engine}
D --> M(Memory)
M --> C
D --> C(Disk)
```

*All Documents, indexes, metadata data are written to disk by the storage engine.<br>
The storage engine is also making decisions about what data to hold in memory and what to commit to disk*

