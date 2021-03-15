# Data Modeling in Mongo DB

A fundamental step in every application is deciding how to organize and model data for your application.

While MongoDB is **schema-less**, meaning that MongoDB does not enforce that documents contain the same fields and types as a relational database would. And while documents could follow entirely different formats, most applications will use a schema to streamline working with data.

## Data Types

In MongoDB there are the following data types:

### ObjectId

The mongo shell provides the ObjectId() wrapper class around the ObjectId data type.

By Default, MongoDB will create an `ObjectId` for every document at the time of insertion/creation. Thus invoking either the `insert()` or `insertOne()` operations within the MongoDB shell, results in the following the generation of an `ObjectId` which is then assigned to inserted document's `_id` field/property.

The `ObjectId` is a 12-byte Object specific to MongoDB and comprised of a _timestamp value_ (4 bytes), a _random value_ (5 bytes) and a _incrementing counter value_ (3 bytes). Generating `ObjectId`s in this fashion ensures that each document inserted into a collection is _unique_ and can be _sorted chronologically_.

#### Syntax

In the MongoDB shell, `new ObjectId()` will generate a new Object ID, which the console often represent textually like `ObjectId("6047ee28c39b55b0ea6cb0d7")`. Since the `ObjectId()` is comprised of a timestamp value, you can call the `getTimestamp()` function on the `_id` property of any auto-generated document to get the timestamp of the object's creation as a `Date`.

Constructors:

- `x = ObjectId()` - generates a new `ObjectId` with timestamp of constructor invocation.
- `y = ObjectId("507f191e810c19729de860ea") - generates a new `ObjectId` with the provided hexadecimal value.

Methods:

- `document._id.getTimestamp()` - returns the timestamp associated with the `ObjectId`

### Text

#### Description:

- In MongoDB, the `text` data type is used to represent strings (a sequence of characters) as as longer sequences of strings that would be represented by the `text` type in relational databases like SQL.
- There is no need to define the length of the string or text field.

#### Syntax:

- In the MongoDB shell, _text_ and _string_ values are declared between either single (`'`) or double quotes (`"`)

Example: `"Text is enclosed in quotation marks indicating the beginning and end of the string"

#### Limitations

- The size of the cannot exceed the maximum overall document size

- Details: text can be of anything length as long as the individual document size remains under the `16` mb limit.

### Booleans

#### Description:

- Used for "Yes"/"No" values
- Used for "On"/"Off" values
- booleans values can only be `true` or `false

#### Syntax:

#### Limitations

### Numbers

The mongo shell treats **all numbers** as _floating-point_ values by default which represents 64-bit integer values.

#### NumberLong

In the MongoDB shell, you can explicitly assign a `NumberLong` value to an document using the `NumberLong()` wrapper class.

Example:

1. Insert document containing a `calc` field/property that has a value: `2090845886852`
2. Use the `$set` operator to update the value of `calc` (same document) so that the value matches `2555555000000`
3. Use the `$inc` operator to update the value of `calc` (same document) so that the value is incremented by `1`

MongoDB Shell:

```
db.collection.insertOne( { _id: 10, calc: NumberLong("2090845886852") } )
db.collection.updateOne( { _id: 10 },
                      { $set:  { calc: NumberLong("2555555000000") } } )
db.collection.updateOne( { _id: 10 },
                      { $inc: { calc: NumberLong("5") } } )
```

> Although the `NumberLong()` constructor accepts integer values from the mongo shell (i.e. without quotes), this is not recommended. Specifying an integer value larger than JavaScript’s defined `Number.MAX_SAFE_INTEGER `may lead to unexpected behavior.

#### NumberInt

The mongo shell provides the `NumberInt()` constructor to explicitly specify 32-bit integers.

Example: Insert a new document into a collection called `planes` with the following properties: - `capacity` of type `NumberInt` - `name` of type `Text` - `max_speed` of type `NumberInt`

Input:

```
db.planes.insertOne({ name: 'Trans-America Mini', max_speed: NumberInt(450), capacity: NumberInt(20)})
```

Output:

```
{
	"acknowledged" : true,
	"insertedId" : ObjectId("604fbe76acfb0f7c61bbfe3b")
}
```

Verification:

```
{
	"_id" : ObjectId("604fbe76acfb0f7c61bbfe3b"),
	"name" : "Trans-America Mini",
	"max_speed" : 450,
	"capacity" : 20
}
```

#### Number - `NumberDecimal`

The mongo shell provides the `NumberDecimal()` constructor to explicitly specify **128-bit decimal-based floating-point** values capable of emulating decimal rounding with exact precision. This functionality is intended for applications that handle monetary data, such as financial, tax, and scientific computations.

> NumberDecimal is a new data type as of MongoDB version `3.4`

Example: Insert a document into a collection called `transactions` with a property of `amount` that has a NumberDecimal defined using the `NumberDecimal()` constructor and defined the value of `1000.55` as a string:

Input:

```
db.transactions.insertOne({ customer: "Alex Schomp", amount: NumberDecimal("1000.55")})
```

Output:

```
{
	"acknowledged" : true,
	"insertedId" : ObjectId("604fbc8bacfb0f7c61bbfe3a")
}
```

Verification:

```
db.transactions.findOne({ _id: new ObjectId("604fbc8bacfb0f7c61bbfe3a")})
```

Output:

```
{
	"_id" : ObjectId("604fbc8bacfb0f7c61bbfe3a"),
	"customer" : "Alex Schomp",
	"amount" : NumberDecimal("1000.55")
}
```

> The decimal BSON type uses the IEEE 754 decimal128 floating-point numbering format which supports 34 decimal digits (i.e. significant digits) and an exponent range of −6143 to +6144.

#### Relevant Operators for Numbers

- `$inc` - can be used to increment the value of a `NumberLong`
- `$set` - can be used to change/assign a value to a document

#### Sorting & Equality of Numbers

Values of `decimal` types are compared/sorted based on their actual numeric value.

As a result, values of type `double` may have an _approximately_ "equal" representation, but differ in their `decimal` representation will be considered **unequal**.

Example:

- Task: Use the `insertMany()` function to create a five (`5`) documents in a collection called `numbers`
- Details
  - Each document an `_id` property starting with `1` for the first record, and incrementing to `5`.
  - Each document should have a `val` property:
    - The first (`1`) document: `val: NumberDecimal("9.99")` _explicit decimal_
    - The second (`2`) document `val: 9.99` - _implicit decimal_
    - The third (`3`) document `val: 10` - _implicit integer_
    - The fourth (`4`) document `val: NumberLong("10")` _explicit long integer_
    - The fifth (`5`) document `val: NumberDecimal("10.0")` - _explicit decimal_
  - Each document should also have an `val` property in which a number is associated.

Example Insert:

```
db.numbers.insertMany([
    { "_id" : 1, "val" : NumberDecimal( "9.99" ) },
    { "_id" : 2, "val" : 9.99 },
    { "_id" : 3, "val" : 10, },
    { "_id" : 4, "val" : NumberLong("10")},
    { "_id" : 5, "val" : NumberDecimal( "10.0" ) },
    ])
```

Successful execution should produce the output:

```
{ "acknowledged" : true, "insertedIds" : [ 1, 2, 3, 4, 5 ] }
```

Exercises: Execute the following queries below using the `find()` function below. Note which documents are returned from each query:

1. Query `numbers` for documents with a `val` matching the _implicit double_ `9.99`

   - Query: `db.numbers.find({ val: 9.99 })`
   - Output: `{ "_id" : 2, "val" : 9.99 }`
   - Understanding Check: Why isn't the document of `_id: 1` returned?

2. Query `numbers` for documents with a `val` matching the _explicit double_ of `9.99`:

   - Query: `db.numbers.find({ val: NumberDecimal('9.99')})`
   - Output: `{ "_id" : 1, "val" : NumberDecimal("9.99") }`
   - Understanding Check: Why isn't the document of `_id: 2` returned?

> _Double_ value types are **excluded** because their values do not match teh _decimal_ representation of `9.99`

3. Search `numbers` for that have an _implicit_ value of `10`.

   - Query: `db.numbers.find({ val: 10 })`
   - Output:
     ```
     { "_id" : 3, "val" : 10 }
     { "_id" : 4, "val" : NumberLong(10) }
     { "_id" : 5, "val" : NumberDecimal("10.0") }
     ```
   - Understanding Check: Why are documents with `NumberLong("10")` and `NumberDecimal("10")` values returned in addition to documents where `val` is defined _implicitly_ as `10`?

Query: `db.numbers.find({ val: NumberDecimal( “10” ) })`

- should match `3` documents ( ids: `3`, `4`, and `5`)

### Dates

The mongo shell provides various methods to return the date, either as a string or as a `Date` object. The mongo shell wraps objects of `Date` type with the `ISODate` helper; however, the objects remain of type `Date`. As a result you can declare date values using `new Date()` or `new ISODate()` with the same result.

#### Syntax:

ISODate

- Type: `Date`
- Syntax:
  - `new Date("<YYYY-mm-dd>")`
    - returns the ISODate with the specified date.
  - `new Date("<YYYY-mm-ddTHH:MM:ss>")`
    - specifies the datetime in the client’s local timezone and returns the `ISODate` with the specified datetime in UTC.
  - `new Date("<YYYY-mm-ddTHH:MM:ssZ>")`
    - specifies the datetime in UTC and returns the ISODate with the specified datetime in UTC.
  - `new Date(<integer>)`
    - specifies the datetime as milliseconds since the Unix epoch (Jan 1, 1970), and returns the resulting ISODate instance.
- Details:
  - Internally, Date objects are stored as a signed 64-bit integer representing the number of milliseconds since the Unix epoch (Jan 1, 1970).
  - Typically used for calculating event time/duration
  - assign to user-defined time: `new Date("2018-09-09")`
  - assign the date to current time using `{ date: new Date() }`
- Operators:
  - `$set: { item: "apple" }` sets the matching document's `item` property to

### Timestamps

- Type: `Timestamp`
- Syntax: `Timestamp(11421532)`
- Details:
  - Guaranteed to be unique, even documents created in th same command will have slightly different timestamps by leveraging an ordinal value.

#### Limitations

- Internally, Date objects are stored as a signed 64-bit integer representing the number of milliseconds since the Unix epoch (Jan 1, 1970).
- Not all database operations and drivers support the full 64-bit range. You may safely work with dates with years within the inclusive range 0 through 9999.

### Embedded Documents

#### Description

#### Syntax:

#### Limitations

- Type: `document`
- Syntax: ` { "a": { ... }}`
- Details:
  - represents a document containing another document
  - multiple documents can be nested inside each document
  - embedded or child documents will follow the same rules as any other document
  - up to 100 levels of nesting _or_ up to 16 mb of overall document size (whichever occurs first)

### Arrays

#### Description

#### Syntax:

#### Limitations

- Type: `Array`
- Syntax: `[...]`
- Details:
  - Arrays can contain any data-type (text, number, date, timestamp, boolean, another document, or another array)
