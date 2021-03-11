# Mongo DB

Overview

- MongoDB NoSQL Database
  - Comprised of "documents" within "collections"
    - collections
      - groups documents into buckets
      - no schema
    - documents
      - essentially a key-value pair
      - similar to a JSON (actually BSON data format)
      - Documents do not consistent format
      - data can be heterogenous
      - supports nesting/embedding documents/objects

Benefits:

- flexibility
- scalability
- less relations
  - unlike relational databases which separate data into strictly defined schemas, Non-relational databases like MongoDB store related pieces of information together in documents
- performance
  - in some scenarios, dependent on proper database design
  - generally able to avoid joining data as you would do in relational databases

The MongoDB Ecosystem

- MongoDB Database
  - This is the core product
  - MongoDB Types
    - self-managed/enterprise
    - atlas (cloud)
    - mobile (runs on a mobile device)
  - Related tools
    - CloudManager
    - Ops Manager
    - Compass
    - Bi Connectors
    - MongoDB Charts
- Stitch
  - Serverless API Query data
  - Serverless Functions
  - Database triggers
  - Real-time Sync

## The Mongo CLI

The most straight-forward way to get started working with data in MongoDB is via the MongoDB CLI which enables us to directly query and manage databases, collections, and documents within MongoDB.

### Starting the MongoDB Background Process

Before we can start working with the our MongoDB database, we need to make sure the process is running. Assuming the shell path is correctly linked to the install directory containing the MongoDB executables, the command `mongod` should start the background process.

You'll know the `mongod` command worked if you see output similar to the snippet below:

```
{"t":{"$date":"2021-03-09T07:59:14.621-07:00"},"s":"I",  "c":"CONTROL",  "id":23285,   "ctx":"main","msg":"Automatically disabling TLS 1.0, to force-enable TLS 1.0 specify --sslDisabledProtocols 'none'"}
{"t":{"$date":"2021-03-09T07:59:14.636-07:00"},"s":"W",  "c":"ASIO",     "id":22601,   "ctx":"main","msg":"No TransportLayer configured during NetworkInterface startup"}
{"t":{"$date":"2021-03-09T07:59:14.636-07:00"},"s":"I",  "c":"NETWORK",  "id":4648602, "ctx":"main","msg":"Implicit TCP FastOpen in use."}
{"t":{"$date":"2021-03-09T07:59:14.639-07:00"},"s":"I",  "c":"STORAGE",  "id":4615611, "ctx":"initandlisten","msg":"MongoDB starting","attr":{"pid":64117,"port":27017,"dbPath":"/data/db","architecture":"64-bit","host":"Apollo"}}
{"t":{"$date":"2021-03-09T07:59:14.639-07:00"},"s":"I",  "c":"CONTROL",  "id":23403,   "ctx":"initandlisten","msg":"Build Info","attr":{"buildInfo":{"version":"4.4.3","gitVersion":"913d6b62acfbb344dde1b116f4161360acd8fd13","modules":[],"allocator":"system","environment":{"distarch":"x86_64","target_arch":"x86_64"}}}}
{"t":{"$date":"2021-03-09T07:59:14.639-07:00"},"s":"I",  "c":"CONTROL",  "id":51765,   "ctx":"initandlisten","msg":"Operating System","attr":{"os":{"name":"Mac OS X","version":"20.3.0"}}}
{"t":{"$date":"2021-03-09T07:59:14.639-07:00"},"s":"I",  "c":"CONTROL",  "id":21951,   "ctx":"initandlisten","msg":"Options set by command line","attr":{"options":{}}}
{"t":{"$date":"2021-03-09T07:59:14.642-07:00"},"s":"E",  "c":"STORAGE",  "id":20568,   "ctx":"initandlisten","msg":"Error setting up listener","attr":{"error":{"code":9001,"codeName":"SocketException","errmsg":"Address already in use"}}}
{"t":{"$date":"2021-03-09T07:59:14.642-07:00"},"s":"I",  "c":"REPL",     "id":4784900, "ctx":"initandlisten","msg":"Stepping down the ReplicationCoordinator for shutdown","attr":{"waitTimeMillis":10000}}
{"t":{"$date":"2021-03-09T07:59:14.644-07:00"},"s":"I",  "c":"COMMAND",  "id":4784901, "ctx":"initandlisten","msg":"Shutting down the MirrorMaestro"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"SHARDING", "id":4784902, "ctx":"initandlisten","msg":"Shutting down the WaitForMajorityService"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"NETWORK",  "id":4784905, "ctx":"initandlisten","msg":"Shutting down the global connection pool"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"NETWORK",  "id":4784918, "ctx":"initandlisten","msg":"Shutting down the ReplicaSetMonitor"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"SHARDING", "id":4784921, "ctx":"initandlisten","msg":"Shutting down the MigrationUtilExecutor"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"CONTROL",  "id":4784925, "ctx":"initandlisten","msg":"Shutting down free monitoring"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"STORAGE",  "id":4784927, "ctx":"initandlisten","msg":"Shutting down the HealthLog"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"STORAGE",  "id":4784929, "ctx":"initandlisten","msg":"Acquiring the global lock for shutdown"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"-",        "id":4784931, "ctx":"initandlisten","msg":"Dropping the scope cache for shutdown"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"FTDC",     "id":4784926, "ctx":"initandlisten","msg":"Shutting down full-time data capture"}
{"t":{"$date":"2021-03-09T07:59:14.645-07:00"},"s":"I",  "c":"CONTROL",  "id":20565,   "ctx":"initandlisten","msg":"Now exiting"}
{"t":{"$date":"2021-03-09T07:59:14.646-07:00"},"s":"I",  "c":"CONTROL",  "id":23138,   "ctx":"initandlisten","msg":"Shutting down","attr":{"exitCode":48}}
```

> By default, MongoDB will start on port 27017. You can override this configuration use the command line switch `--port <port-number>`, however doing so will require you also pass the `--port` switch to the `mongo` command (which enters you into the Mongo CLI shell)

### Entering Mongo CLI

Once the background process has been initialized, we can launch the the Mongo CLI (Command Line Interface) and begin interacting with our local MongoDB server.

To open the Mongo CLI, simple enter the `mongo` command.

Executing the `mongo` command should enter you into the MongoDB CLI or REPL. You will know it worked if you see the shell version printed within the shell.

Output from `mongo` should appear similar to:

```sh
MongoDB shell version v4.4.3
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("81cd403b-b691-435d-8e2d-c9fd1291b964") }
MongoDB server version: 4.4.3
```

### Mongo Databases and Collections

Once in the Mongo CLI, we can see the existing databases using the command: `show dbs`.

Out of the box, MongoDB will come pre-configure 3 databases:

1. admin
2. config
3. local

The `use` command is how we select the database we want to work with:

```sh
use <database-name>
```

It is important to note, calling the `use` command will create the database if it does not already exist. So the `use` command is inherently multipurpose. If executed with a database that already exists, the `use` command will specify which database inserts, queries and updates occur on, but if the the `use` command is followed by the name of a database that does _not_ already exist - the database will be created and selected as the target database.

Once selected via the `use` command. You reference the current database as `db`. You then reference the collection you want to work with using dot notation.

```
db.collection
```

We then continue using dot notation to access various operations for that collections.

## CRUD and MongoDB

The acronym CRUD stands for _create, read, update, and delete_ - and just like other database products, MongoDB supports all four (4) operations.

### Basic Inserts (Create)

To insert a _single_ new document into a collection, we use the `insertOne()` function. Between the parenthesis we specify the object to be added to the MongoDB collection. The syntax for `insertOne()` requires a single parameter (the data), but also supports a second parameter (options).

Syntax: Inserting a Single Document

- data represents the object to be inserted
- options allow you to specify additional information for how the data should be added to the collection

```js
insertOne(data, options);
```

For example, if we had a collection called `products`, we could call `database.products.insertOne()` and then provide the product information to be added to the collection within the parenthesis.

This might look something like:

```js
db.products.insertOne({
  name: "The Subtle Art of Not Giving a F**k",
  author: "Mark Manson",
  type: "Book",
  price: 15.99,
});
```

The Mongo shell will respond with the result of the insert operation, and return an object. Successful inserts will return an object that has an `acknowledged` property set to `true` and `insertedId` property that matches the generated ID of the new document:

```js
{
	"acknowledged" : true,
	"insertedId" : ObjectId("60478dfa2befa0ebcbe082d0")
}
```

The `"insertedId"` is a special type specific to MongoDB, and is an automatically generated unique identifier or index for the document within the collection.

MongoDB generates and assigns an "ObjectID" for every document, but this functionality can be overridden by specifying an alternative ID or index for the document as part of the object definition specified during the `insert()` or `insertOne()` operation as the `_id` field.

This might look like:

```
db.products.insertOne({
  name: "LaserJet P1606",
  manufacturer: "HP",
  type: "Laser",
  _id: "MFJT_HPSBP",
});
```

While an `_id` property is required - the associated data-type is up to you. The `_id` field supports any valid MongoDB data type so long as the `_id` property is unique within the collection.

If you want to insert more than one (1) document at a time, MongoDB exposes the `insertMany()` function which follows the same pattern as `insertOne()`. The difference is related to the the expected value of for the `data` parameter, and the way in which the `options` (optionally provided) will mutate that data as it gets added to the collection:

Syntax: Inserting Multiple documents

- `data` represents an array of JSON objects to be inserted into the collection.
- `options` is an optional parameter that can be used for more advanced inserts.

```
insertMany(data, options)
```

#### Example

Given a database with a collection called `flights`, multiple documents could be inserted in a single command by passing an array of objects/documents to the `db.collection.insertMany()` function:

```
db.flights.insertMany([
   {
     "departureAirport": "MUC",
     "arrivalAirport": "SFO",
     "aircraft": "Airbus A380",
     "distance": 12000,
     "intercontinental": true
   },
   {
     "departureAirport": "LHR",
     "arrivalAirport": "TXL",
     "aircraft": "Airbus A320",
     "distance": 950,
     "intercontinental": false
   }
 ])
```

Which if successful will return the inserted document's IDs:

```
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("604a5b612b6c12ccca9f4c32"),
		ObjectId("604a5b612b6c12ccca9f4c33")
	]
}
```

> In the above example, the output resulting from the `insertMany()` operation contains two (2) `ObjectId()`'s - You many notice that the IDs for both inserts are almost the same, with only the last digit in the `ObjectID` differing - because the first `ObjectId()` ends in `32` and the the second one ends in `33`, they will be returned in that order when queried together (i.e. 32 will be _before_ 33).

### Retrieving Documents (Read)

Once data has been entered into the database, the next logical operation is to be able to look at that data. In MongoDB, data is "read" or retrieved from a collection using the `find()` function.

Just like `insertOne()` and `insertMany()` the find operation uses dot notation (`.`) to specify the collection upon which the `find()` operation should be executing.

Syntax:

- The `find()` operation takes two (2) _optional_ parameters:
  - `filter` which accepts arguments to filter the returned documents
  - `options` which configures more advanced querying

```
db.collection.find(filter, options)
```

> Without passing an argument to the `find()` function, MongoDB will return **all** the documents in the collection specified.

Alternative to the `find()` function, we can use the `findOne()` function to return a **single** document from a collection.

Syntax:

- The `findOne()` operation takes one (1) _optional_ parameter and one (1) required parameter:
  - required
    - `filter` which accepts arguments to filter the returned documents
  - optional
    - `options` which configures more advanced querying

```
db.collection.findOne(filter, options)
```

The only difference between the `db.collection.find()` and `db.collection.findOne()` operations is the number of documents that will be returned. The `findOne()` function will return **only the first** document that matches the `filter` condition(s) specified, where as the `db.collection.find()` will return _all_ documents that match the filter condition.

#### Example

In a collection called `flights` there is some documents with flight information. This We can see this by running `db.flights.find()` without any parameters to return all the documents contained within that collection.

Given the **Input** command:

```
db.flights.find()
```

We should see **output** of all the documents in the `flights` collection:

```
{ "_id" : ObjectId("6047ee28c39b55b0ea6cb0d7"), "departureAirport" : "MUC", "arrivalAirport" : "SFO", "aircraft" : "Airbus A380", "distance" : 12000, "intercontinental" : true }
{ "_id" : ObjectId("6047ee48c39b55b0ea6cb0d8"), "departureAirport" : "LHR", "arrivalAirport" : "TXL", "aircraft" : "Airbus A320", "distance" : 950, "intercontinental" : false }
{ "_id" : ObjectId("604a5b612b6c12ccca9f4c32"), "departureAirport" : "MUC", "arrivalAirport" : "SFO", "aircraft" : "Airbus A380", "distance" : 12000, "intercontinental" : true }
{ "_id" : ObjectId("604a5b612b6c12ccca9f4c33"), "departureAirport" : "LHR", "arrivalAirport" : "TXL", "aircraft" : "Airbus A320", "distance" : 950, "intercontinental" : false }
```

#### Pretty-ing Output

You can append or chain the `pretty()` function to make the results returned by the `find()` function more readable:

```
db.collection.find().pretty()
```

For example, reading the documents from a hypothetical collection called `flights` could return the following output using just `find()` might go something like:

So without the `pretty()` Command:

```
db.flights.find()
```

The **Output** should be as follows:

```
{ "_id" : ObjectId("6047ee28c39b55b0ea6cb0d7"), "departureAirport" : "MUC", "arrivalAirport" : "SFO", "aircraft" : "Airbus A380", "distance" : 12000, "intercontinental" : true }
{ "_id" : ObjectId("6047ee48c39b55b0ea6cb0d8"), "departureAirport" : "LHR", "arrivalAirport" : "TXL", "aircraft" : "Airbus A320", "distance" : 950, "intercontinental" : false }
{ "_id" : ObjectId("604a5b612b6c12ccca9f4c32"), "departureAirport" : "MUC", "arrivalAirport" : "SFO", "aircraft" : "Airbus A380", "distance" : 12000, "intercontinental" : true }
{ "_id" : ObjectId("604a5b612b6c12ccca9f4c33"), "departureAirport" : "LHR", "arrivalAirport" : "TXL", "aircraft" : "Airbus A320", "distance" : 950, "intercontinental" : false }
```

Appending `pretty()` simply re-format the output so that its easier to read:

Command:

```
db.flights.find().pretty();
```

Output:

```
{
	"_id" : ObjectId("6047ee28c39b55b0ea6cb0d7"),
	"departureAirport" : "MUC",
	"arrivalAirport" : "SFO",
	"aircraft" : "Airbus A380",
	"distance" : 12000,
	"intercontinental" : true
}
{
	"_id" : ObjectId("6047ee48c39b55b0ea6cb0d8"),
	"departureAirport" : "LHR",
	"arrivalAirport" : "TXL",
	"aircraft" : "Airbus A320",
	"distance" : 950,
	"intercontinental" : false
}
{
	"_id" : ObjectId("604a5b612b6c12ccca9f4c32"),
	"departureAirport" : "MUC",
	"arrivalAirport" : "SFO",
	"aircraft" : "Airbus A380",
	"distance" : 12000,
	"intercontinental" : true
}
{
	"_id" : ObjectId("604a5b612b6c12ccca9f4c33"),
	"departureAirport" : "LHR",
	"arrivalAirport" : "TXL",
	"aircraft" : "Airbus A320",
	"distance" : 950,
	"intercontinental" : false
}
```

> The `pretty()` command can be used in any function that will return one or more documents.

### Changing Document Information (Update)

Updating documents in a collection is the third letter of CRUD. In MongoDB, we use the `updateOne(filter, data, options)`, `update(filter, data, options)` and the `replaceOne(filter, data, options)`.

Unlike `find()`, `findOne()`, `insertMany()` and `insertOne()` - which take two (2), arguments - `updateOne()`, `updateMany()` and `replaceOne()` all take three (3) arguments.

#### Single Document Update

The `updateOne()` function is designed for modifying a single document.

##### Syntax

Updating a single document

- Two (2) required parameters and one (1) optional parameter
  - required:
    - `filter` specifies the match conditions
    - `data` specifies the new values for the document
  - optional
    - `options` ???

```
updateOne(filter, data, options)
```

Updating multiple documents:

- Two (2) required parameters and one (1) optional parameter
  - required:
    - `filter` specifies the match conditions
    - `data` specifies the new values for the document
  - optional
    - `options` ???

```
updateMany(filter, data, options)
```

The `filter` parameter should have a key value that matches the one of the document's root level keys. The value associated with the `filter` key is the matching value that the document being updated should have.

The `data` parameter is another object, but a special one at that. The keys of the provided document should use one of MongoDB's reserved functions for updating documents:

Adding a key of `$set` in the `data` parameter specifies that the value of `$set` is a document containing key-value pairs that should be assigned to the document(s) which match the specified filter condition(s). `$set` is known as an _atomic operator_.

> Passing a regular object (or document) as the `data` parameter will throw an error: `Error: the update operation document must contain atopic operators`

Example:

```
db.flights.updateOne({ distance: 12000}, { $set: { marker: "red"}})
```

The snipped above says to update the _first_ (since this is an `updateOne()` function) document in the `flights` collection with a property (or "key") defined as `distance` and the associated value should be `12000`.

### Atomic Operators

**atomic operators** are special functions used for filtering and updating documents in MongoDB

### Removing Documents (Delete)

We use either the `deleteOne(filter, options)` or the `deleteMany(filter, options)` to remove documents from collections in MongoDB.

Syntax

```
db.collection.delete(filter, options)
```

- Parameters:
  - `filter` specifies the match conditions for the documents to be deleted.
  - `options` - ???

**Important** - using `db.collection.deleteMany({})` will delete all records in the collection. Passing an empty object as the filter is similar to wild-carding with `*` in SQL, it specifies that we want to match everything.

### Filters

With deletes, reads, and updates - we use a `filter` parameter to specify which documents should be deleted, updated, or read.

The `filter` parameter is syntactically very similar to the JSON representation of the document we use in the `insertOne()` and `insertMany()` functions. The documents that have key-value pairs that match those provided via the `filter` parameter, are the only documents that are read (`find()`/`findOne`), updated (`updateOne()`/`updateMany()`,replaceOne()`), or deleted (`deleteOne()`/`deleteMany()`).

So if we have a collection called `flights`, we can view all the documents in the collection using `db.flights.find()` - or `db.flights.find().pretty()` to return a more readable set of documents:

Command:

```js
db.flights.find().pretty();
```

Output:

```js
{
}
```

### Mongo and CRUD Summary

Creating new records, reading records, updating records, and deleting records are the four fundamental operations necessary for any database product.

In MongoDB, we have the following utility functions to execute these operations:

1. **C**reate
   - `insertOne(data, options)`
     - used to insert a single document
   - `insertMany(data, options)`
     - used to insert multiple documents
2. **R**ead
   - `find(filter, options)`
     - used to query multiple documents
   - `findOne(filter, options)`
     - used to query a single document
     - return the _first_ document that matches the filter condition
3. **U**pdate
   - `updateOne(filter, data, options)
     - used to update a single document
   - `update(filter, data, options)
     - used to update a multiple documents
   - `replaceOne(filter, data, options)`
     - used to replace an existing document with a new document
     - similar to a delete + insert operation
4. **D**elete
   - `deleteOne(filter, options)`
     - used to delete a single document
   - `deleteMany(filter, options)`
     - used to delete multiple documents

#### Tips, Tricks and Takeaways

**Ids are can be defined however you want** - as long as every document in a collection has a _unique_ id, the value of the id is unimportant. By default MongoDB will generate a `_id` property with an associated `ObjectId` which is a custom type unique to MongoDB.

**filters are defined as object/documents** - Filters are common to every CRUD operation except "Create" (e.g. "inserts"). Filters are objects that specify the properties we are looking to match.

**Commands are always invoked on the `db` object** - In order for any of the above function to execute any of the CRUD operations, we must specify (using dot notation) the `db` and collection upon which the functions should be invoked. ( e.g `db.collection.function(args)`)

**Making output more readable** - With any of the Mongo functions above, we can append the `pretty()` function to format the output such that is easier to parse and consume by a human-being.
