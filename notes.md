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

You can append or "chain" the `pretty()` function to make the results returned by the `find()` function more readable:

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

> The `pretty()` command can be used in any function that returns a _cursor_.

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

#### Deleting Every Document in a Collection

Its important to note using `db.collection.deleteMany({})` will delete all records in the collection. Passing an empty object as the filter is similar to wild-carding with `*` in SQL, it specifies that we want to match everything.

### Filters

With deletes, reads, and updates - we use a `filter` parameter to specify which documents should be deleted, updated, or read.

The `filter` parameter is syntactically very similar to the JSON representation of the document we use in the `insertOne()` and `insertMany()` functions. The documents that have key-value pairs that match those provided via the `filter` parameter, are the only documents that are read (`find()`/`findOne`), updated (`updateOne()`/`updateMany()`,replaceOne()`), or deleted (`deleteOne()`/`deleteMany()`).

So if we have a collection called `flights`, we can view all the documents in the collection using `db.flights.find()` - or `db.flights.find().pretty()` to return a more readable set of documents:

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

## Cursors

As collections get larger and contain an increasing number of documents, returning all the documents in a single function may not be the desired result. Since a `find()` command could match 1, 10, 100, 1000, or 1,000,000 documents - it doesn't technically return the documents in an array (which is how we inserted them into the database), instead it uses a _cursor_.

> Looking back to the results of `db.flights.find()` , notice there are no square brackets (`[]`) or commas (`,`) in the documents that are printed to the screen. This is because MongoDB didn't return an array of documents, rather it returned a _cursor_ object representing the matching documents.

**Cursors** are objects returned from the `find()` operation that contain the matching documents. With _cursors_ the returned results can be cycled through, providing a sort of pagination functionality.

To see this in action, let's enter more documents. For this example, we will create a collection called `passengers` and alongside the `flights` collection.

Input:

```
db.passengers.insertMany([
   {
     "name": "Max Schwarzmueller",
     "age": 29
   },
   {
     "name": "Manu Lorenz",
     "age": 30
   },
   {
     "name": "Chris Hayton",
     "age": 35
   },
   {
     "name": "Sandeep Kumar",
     "age": 28
   },
   {
     "name": "Maria Jones",
     "age": 30
   },
   {
     "name": "Alexandra Maier",
     "age": 27
   },
   {
     "name": "Dr. Phil Evans",
     "age": 47
   },
   {
     "name": "Sandra Brugge",
     "age": 33
   },
   {
     "name": "Elisabeth Mayr",
     "age": 29
   },
   {
     "name": "Frank Cube",
     "age": 41
   },
   {
     "name": "Karandeep Alun",
     "age": 48
   },
   {
     "name": "Michaela Drayer",
     "age": 39
   },
   {
     "name": "Bernd Hoftstadt",
     "age": 22
   },
   {
     "name": "Scott Tolib",
     "age": 44
   },
   {
     "name": "Freddy Melver",
     "age": 41
   },
   {
     "name": "Alexis Bohed",
     "age": 35
   },
   {
     "name": "Melanie Palace",
     "age": 27
   },
   {
     "name": "Armin Glutch",
     "age": 35
   },
   {
     "name": "Klaus Arber",
     "age": 53
   },
   {
     "name": "Albert Twostone",
     "age": 68
   },
   {
     "name": "Gordon Black",
     "age": 38
   }
 ]
 )
```

Executed successfully will produce the following:

```
{
	"acknowledged" : true,
	"insertedIds" : [
		ObjectId("604a65c63918215ca022eed6"),
		ObjectId("604a65c63918215ca022eed7"),
		ObjectId("604a65c63918215ca022eed8"),
		ObjectId("604a65c63918215ca022eed9"),
		ObjectId("604a65c63918215ca022eeda"),
		ObjectId("604a65c63918215ca022eedb"),
		ObjectId("604a65c63918215ca022eedc"),
		ObjectId("604a65c63918215ca022eedd"),
		ObjectId("604a65c63918215ca022eede"),
		ObjectId("604a65c63918215ca022eedf"),
		ObjectId("604a65c63918215ca022eee0"),
		ObjectId("604a65c63918215ca022eee1"),
		ObjectId("604a65c63918215ca022eee2"),
		ObjectId("604a65c63918215ca022eee3"),
		ObjectId("604a65c63918215ca022eee4"),
		ObjectId("604a65c63918215ca022eee5"),
		ObjectId("604a65c63918215ca022eee6"),
		ObjectId("604a65c63918215ca022eee7"),
		ObjectId("604a65c63918215ca022eee8"),
		ObjectId("604a65c63918215ca022eee9"),
		ObjectId("604a65c63918215ca022eeea")
	]
}
```

This should work regardless of whether or not the `passengers` collection exists. If it already exists the `insertMany()` operation will simply add the records to the collection, however if it doesn't exist, the mongo shell should automatically create the collection and insert the records.

It should also be noted that the `insertMany()` operation inserts each document _in the same order_ that it was entered/executed.

Now calling `db.passengers.find().pretty()` will execute a wild-card query on the `passengers` collection in the database and should produce the output:

```
{
	"_id" : ObjectId("604a65c63918215ca022eed6"),
	"name" : "Max Schwarzmueller",
	"age" : 29
}
{
	"_id" : ObjectId("604a65c63918215ca022eed7"),
	"name" : "Manu Lorenz",
	"age" : 30
}
{
	"_id" : ObjectId("604a65c63918215ca022eed8"),
	"name" : "Chris Hayton",
	"age" : 35
}
{
	"_id" : ObjectId("604a65c63918215ca022eed9"),
	"name" : "Sandeep Kumar",
	"age" : 28
}
{
	"_id" : ObjectId("604a65c63918215ca022eeda"),
	"name" : "Maria Jones",
	"age" : 30
}
{
	"_id" : ObjectId("604a65c63918215ca022eedb"),
	"name" : "Alexandra Maier",
	"age" : 27
}
{
	"_id" : ObjectId("604a65c63918215ca022eedc"),
	"name" : "Dr. Phil Evans",
	"age" : 47
}
{
	"_id" : ObjectId("604a65c63918215ca022eedd"),
	"name" : "Sandra Brugge",
	"age" : 33
}
{
	"_id" : ObjectId("604a65c63918215ca022eede"),
	"name" : "Elisabeth Mayr",
	"age" : 29
}
{
	"_id" : ObjectId("604a65c63918215ca022eedf"),
	"name" : "Frank Cube",
	"age" : 41
}
{
	"_id" : ObjectId("604a65c63918215ca022eee0"),
	"name" : "Karandeep Alun",
	"age" : 48
}
{
	"_id" : ObjectId("604a65c63918215ca022eee1"),
	"name" : "Michaela Drayer",
	"age" : 39
}
{
	"_id" : ObjectId("604a65c63918215ca022eee2"),
	"name" : "Bernd Hoftstadt",
	"age" : 22
}
{
	"_id" : ObjectId("604a65c63918215ca022eee3"),
	"name" : "Scott Tolib",
	"age" : 44
}
{
	"_id" : ObjectId("604a65c63918215ca022eee4"),
	"name" : "Freddy Melver",
	"age" : 41
}
{
	"_id" : ObjectId("604a65c63918215ca022eee5"),
	"name" : "Alexis Bohed",
	"age" : 35
}
{
	"_id" : ObjectId("604a65c63918215ca022eee6"),
	"name" : "Melanie Palace",
	"age" : 27
}
{
	"_id" : ObjectId("604a65c63918215ca022eee7"),
	"name" : "Armin Glutch",
	"age" : 35
}
{
	"_id" : ObjectId("604a65c63918215ca022eee8"),
	"name" : "Klaus Arber",
	"age" : 53
}
{
	"_id" : ObjectId("604a65c63918215ca022eee9"),
	"name" : "Albert Twostone",
	"age" : 68
}
Type "it" for more
```

There's a few things to notice here:

1. Notice that the documents printed out are not delimited with commas (`,`). They are also not wrapped in square brackets (`[]`). That is because the actual return value is not an array of documents, but rather a _cursor_ object.
2. While the query did technically return all the documents within the collection, the output does not contain all the documents. Notice the last document in the output pertains to `"Albert Twostone"`, but the array of documents we provided in the `insertMany()` operation ended with a passenger whose `{"name": "Gordon Black"}`.
3. In order to get the next set of documents we can the `it` command, which will append output with the final record. However if there number of documents in the result set had more than `20` additional documents, we would need to keep using the `it` command to cycle through all the documents in the result set.

### Cursor functions - `toArray()`

In the event that working with the documents in the result set requires that the dcouments be arranged within a data structure like an array, appending the `toArray()` function to the `find()` function, will return an the matching documents as an array.

Command:

```
db.passengers.find().toArray()
```

Output:

```
[
	{
		"_id" : ObjectId("604a65c63918215ca022eed6"),
		"name" : "Max Schwarzmueller",
		"age" : 29
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eed7"),
		"name" : "Manu Lorenz",
		"age" : 30
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eed8"),
		"name" : "Chris Hayton",
		"age" : 35
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eed9"),
		"name" : "Sandeep Kumar",
		"age" : 28
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eeda"),
		"name" : "Maria Jones",
		"age" : 30
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eedb"),
		"name" : "Alexandra Maier",
		"age" : 27
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eedc"),
		"name" : "Dr. Phil Evans",
		"age" : 47
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eedd"),
		"name" : "Sandra Brugge",
		"age" : 33
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eede"),
		"name" : "Elisabeth Mayr",
		"age" : 29
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eedf"),
		"name" : "Frank Cube",
		"age" : 41
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee0"),
		"name" : "Karandeep Alun",
		"age" : 48
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee1"),
		"name" : "Michaela Drayer",
		"age" : 39
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee2"),
		"name" : "Bernd Hoftstadt",
		"age" : 22
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee3"),
		"name" : "Scott Tolib",
		"age" : 44
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee4"),
		"name" : "Freddy Melver",
		"age" : 41
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee5"),
		"name" : "Alexis Bohed",
		"age" : 35
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee6"),
		"name" : "Melanie Palace",
		"age" : 27
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee7"),
		"name" : "Armin Glutch",
		"age" : 35
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee8"),
		"name" : "Klaus Arber",
		"age" : 53
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eee9"),
		"name" : "Albert Twostone",
		"age" : 68
	},
	{
		"_id" : ObjectId("604a65c63918215ca022eeea"),
		"name" : "Gordon Black",
		"age" : 38
	}
]
```

> Notice that now _all the documents_ that match the `find()` condition are returned here in an array.

### Cursor Functions - `forEach()`

In the "real world", a more practical way of handling the result set may be to invoke the `db.collection.find().forEach()` function. Since the `mongo` shell is based on JavaScript, working with the `forEach()` function in the mongo shell uses the same syntax as the `forEach()` function called upon arrays in JavaScript.

> The next example is going to follow the syntax used in the `mongo` shell (e.g. JavaScript syntax), while syntax may very depending on the environment (Python, C#, etc.), the concept will be very similar.

Since the `mongo` shell uses the same syntax as JavaScript, it is not just valid to define and execute an inline or anonymous function - it is expected.

#### Syntax: `forEach()

The `forEach()` function will provide the parameter function (aka "inline" or "anonymous" function) with a variable representing each individual document. Then the function will be invoked asynchronously, passing in each document and invoking the function.

Designed to support this functionality, the `mongo` shell offers a helper function `printJson()` which prints a document (the parameter) in JSON format. So the results as JSON using the `forEach()` function would follow the general format:

```
db.collection.find().forEach((itemInCollection) => { printJson(itemInCollection) */})
```

> This is essentially what happens by default when you invoke the `find()` command - except that it paginates the results in groups of 20 document (by default)

## Working with Documents

Initially the amount of data contained in the results of a `find()` operation are small, but as the number of documents, as well as the size of each of those documents increases, these operations can get increasingly demanding and more consideration must be given to the queries. As such understanding how documents work in MongoDB, as well as how we can transform our results becomes increasingly essential.

### Projections

Arguably the biggest challenges in the web and mobile development arena pertain to bandwidth: as such tools like tree shaking, code-bundling/minification, and compression algorithms are used to reduce the amount of data an application needs to send over the wire.

With a **Projection**, the documents returned from `find()` or `findOne()` can be stripped of any extraneous information, With small documents (e.g. few fields, no nested documents, etc.) - projection may have little impact, but as documents increase in size, using projections can dramatically reduce the amount of data returned from queries to MongoDB resulting in faster applications operating with greater efficiency.

#### Reviewing the Passengers Collection

Recall, using the `find()` function to query the `passengers` collection returns a _cursor_, enabling data mutation and pagination of the query results:

> Reload the data set using the [passengers.json](/data/02-passengers.json) file if needed.

Command:

```
db.passengers.find()
```

Output:

```
{ "_id" : ObjectId("604a65c63918215ca022eed6"), "name" : "Max Schwarzmueller", "age" : 29 }
{ "_id" : ObjectId("604a65c63918215ca022eed7"), "name" : "Manu Lorenz", "age" : 30 }
{ "_id" : ObjectId("604a65c63918215ca022eed8"), "name" : "Chris Hayton", "age" : 35 }
{ "_id" : ObjectId("604a65c63918215ca022eed9"), "name" : "Sandeep Kumar", "age" : 28 }
{ "_id" : ObjectId("604a65c63918215ca022eeda"), "name" : "Maria Jones", "age" : 30 }
{ "_id" : ObjectId("604a65c63918215ca022eedb"), "name" : "Alexandra Maier", "age" : 27 }
{ "_id" : ObjectId("604a65c63918215ca022eedc"), "name" : "Dr. Phil Evans", "age" : 47 }
{ "_id" : ObjectId("604a65c63918215ca022eedd"), "name" : "Sandra Brugge", "age" : 33 }
{ "_id" : ObjectId("604a65c63918215ca022eede"), "name" : "Elisabeth Mayr", "age" : 29 }
{ "_id" : ObjectId("604a65c63918215ca022eedf"), "name" : "Frank Cube", "age" : 41 }
{ "_id" : ObjectId("604a65c63918215ca022eee0"), "name" : "Karandeep Alun", "age" : 48 }
{ "_id" : ObjectId("604a65c63918215ca022eee1"), "name" : "Michaela Drayer", "age" : 39 }
{ "_id" : ObjectId("604a65c63918215ca022eee2"), "name" : "Bernd Hoftstadt", "age" : 22 }
{ "_id" : ObjectId("604a65c63918215ca022eee3"), "name" : "Scott Tolib", "age" : 44 }
{ "_id" : ObjectId("604a65c63918215ca022eee4"), "name" : "Freddy Melver", "age" : 41 }
{ "_id" : ObjectId("604a65c63918215ca022eee5"), "name" : "Alexis Bohed", "age" : 35 }
{ "_id" : ObjectId("604a65c63918215ca022eee6"), "name" : "Melanie Palace", "age" : 27 }
{ "_id" : ObjectId("604a65c63918215ca022eee7"), "name" : "Armin Glutch", "age" : 35 }
{ "_id" : ObjectId("604a65c63918215ca022eee8"), "name" : "Klaus Arber", "age" : 53 }
{ "_id" : ObjectId("604a65c63918215ca022eee9"), "name" : "Albert Twostone", "age" : 68 }
```

**Takeaway** - Projections help avoid wasted bandwidth by limiting the fields/properties returned in each document as a result of query to MongoDB.

#### Projecting on the Passenger Collection

In order to perform a _projection_ operation, we will need to pass a filter argument. Passing the `filter` argument is required because without a `filter` argument, the `options` argument cannot be defiend. Since we still want to return all documents, we can wildcard the `filter` argument using an empty object/document: `{}`.

With a `filter` defined, a second argument (the `options` argument) can be defined as the second parameter to the `find()` command. The `options` argument is another document/object with keys that match the property names of the documents, along with an associated numeric value: `1` indicating that that field should be returned - conversely you can explicitely _exclude_ fields/properties by specifying the document/object key as with the name (the same as _include_), but assigning an assocaited value of `0` (indicating the field should be excluded).

Example: Get all the `name`s of passengers contained within the `passengers` collection:

Command:

```
db.passengers.find({}, { name: 1})
```

Output:

```
{ "_id" : ObjectId("604a65c63918215ca022eed6"), "name" : "Max Schwarzmueller" }
{ "_id" : ObjectId("604a65c63918215ca022eed7"), "name" : "Manu Lorenz" }
{ "_id" : ObjectId("604a65c63918215ca022eed8"), "name" : "Chris Hayton" }
{ "_id" : ObjectId("604a65c63918215ca022eed9"), "name" : "Sandeep Kumar" }
{ "_id" : ObjectId("604a65c63918215ca022eeda"), "name" : "Maria Jones" }
{ "_id" : ObjectId("604a65c63918215ca022eedb"), "name" : "Alexandra Maier" }
{ "_id" : ObjectId("604a65c63918215ca022eedc"), "name" : "Dr. Phil Evans" }
{ "_id" : ObjectId("604a65c63918215ca022eedd"), "name" : "Sandra Brugge" }
{ "_id" : ObjectId("604a65c63918215ca022eede"), "name" : "Elisabeth Mayr" }
{ "_id" : ObjectId("604a65c63918215ca022eedf"), "name" : "Frank Cube" }
{ "_id" : ObjectId("604a65c63918215ca022eee0"), "name" : "Karandeep Alun" }
{ "_id" : ObjectId("604a65c63918215ca022eee1"), "name" : "Michaela Drayer" }
{ "_id" : ObjectId("604a65c63918215ca022eee2"), "name" : "Bernd Hoftstadt" }
{ "_id" : ObjectId("604a65c63918215ca022eee3"), "name" : "Scott Tolib" }
{ "_id" : ObjectId("604a65c63918215ca022eee4"), "name" : "Freddy Melver" }
{ "_id" : ObjectId("604a65c63918215ca022eee5"), "name" : "Alexis Bohed" }
{ "_id" : ObjectId("604a65c63918215ca022eee6"), "name" : "Melanie Palace" }
{ "_id" : ObjectId("604a65c63918215ca022eee7"), "name" : "Armin Glutch" }
{ "_id" : ObjectId("604a65c63918215ca022eee8"), "name" : "Klaus Arber" }
{ "_id" : ObjectId("604a65c63918215ca022eee9"), "name" : "Albert Twostone" }
Type "it" for more
```

It is important to note that all the fields/properties are still there, the _projection_ operation doesn't actual mutate the value of the documents in the collection, just returns a partial document (as specified in the `options` parameter).

The results of `db.passengers.find({}, { name: 1})` returned documents including two (2) fields: `_id` and `name` - but only `name` was specified in the `options` argument of the `find()` operation (the _projection_). By default, MongoDB will _always return the `_id` property of projections_.

In order to exclude the `_id` property from the result-set requires explicitely defining that the property be excluded:

**Command** - Query all the `passengers` documents, and return _just_ the `name` field

```
db.passengers.find({}, { name: 1, _id: 0})
```

**Output:**

```
{ "name" : "Max Schwarzmueller" }
{ "name" : "Manu Lorenz" }
{ "name" : "Chris Hayton" }
{ "name" : "Sandeep Kumar" }
{ "name" : "Maria Jones" }
{ "name" : "Alexandra Maier" }
{ "name" : "Dr. Phil Evans" }
{ "name" : "Sandra Brugge" }
{ "name" : "Elisabeth Mayr" }
{ "name" : "Frank Cube" }
{ "name" : "Karandeep Alun" }
{ "name" : "Michaela Drayer" }
{ "name" : "Bernd Hoftstadt" }
{ "name" : "Scott Tolib" }
{ "name" : "Freddy Melver" }
{ "name" : "Alexis Bohed" }
{ "name" : "Melanie Palace" }
{ "name" : "Armin Glutch" }
{ "name" : "Klaus Arber" }
{ "name" : "Albert Twostone" }
Type "it" for more
```

> The `_id` field is the only field that requires _explicit exclusion from projections_. Any other fields not _included_ in the `options` argument will be excluded by default.

Since MongoDB documents support **up to 100 levels of nested documents** - _projections_ increase in utility as documents increase in size or in scenarios where there are multiple levels of nesting contained within each document. The only other restriction MongoDB places on documents is that they must be less than `16mb` in size (which is substantial considering documents only text).
