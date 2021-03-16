# Database Commands

| Command            | Description                                                |
| ------------------ | ---------------------------------------------------------- |
| `show dbs`         | prints existing databases                                  |
| `show collections` | prints existing collections of currently selected database |

# Database Functions

| Command                                 | Description                                                                                                                                                                  | Docs                                                                                                             |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| `db.createCollection(<name>,<options>)` | creates a collection with the provided `<name>` and optional `<options>`                                                                                                     | [db.createCollection](https://docs.mongodb.com/manual/reference/method/db.createCollection/#db.createCollection) |
| `db.dropDatabase(<writeConcern>)`       | Removes the current database, deleting the associated data files. Takes an optional parameter: `<writeConcern>`, which is a document defining details for the drop operation | [db.createCollection](https://docs.mongodb.com/manual/reference/method/db.createCollection/#db.createCollection) |
| `db.getCollectionNames()`               |                                                                                                                                                                              |

# Data Model Functions

| Function          | Description                                                  |
| ----------------- | ------------------------------------------------------------ |
| `new Timestamp()` | returns a timestamp object representing the current time     |
| `new Date()`      | returns an `ISODate` object for the current date as a string |

|
