# MongoDB Getting started (introduction)

## mongorestore
* a command that allows us to take a database dump (restore point) and populate the database with that backed up data.

## mongoimport
* a command that will take a JSON file as an input and load that data to a specified database and collection e.g.
>mongoimport -d _databaseName_ -c _collectionName_ _fileName_
