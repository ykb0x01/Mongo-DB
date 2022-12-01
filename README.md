# [01] Introduction

## Course Outline

<img width="548" alt="Screenshot 2022-11-21 115916" src="https://user-images.githubusercontent.com/77200870/203033899-7c69f02c-ea8e-446a-9d81-0c86a35efee4.png">

## What is mongoDB?

<img width="431" alt="Screenshot 2022-11-21 105449" src="https://user-images.githubusercontent.com/77200870/203020754-abe64d92-6008-48fc-be58-8a360a5f0523.png">

<img width="529" alt="Screenshot 2022-11-21 120546" src="https://user-images.githubusercontent.com/77200870/203035129-30dddb0f-928e-485c-a3b0-a52b5bae218d.png">

- Derived from *Humongous* because it can store lots of data
- Inside documents we use JSON (BJSON) data format
- Documents allow us to create complex relashions between data and store them in one document

<img width="301" alt="Screenshot 2022-11-21 105830" src="https://user-images.githubusercontent.com/77200870/203030556-525e9e36-3708-4d19-bcb8-6421d47610f4.png">

## Key mongoDB Characteristics

- Schemaless
- No/Few Relashions

## MongoDB Ecosystem

<img width="523" alt="Screenshot 2022-11-21 110736" src="https://user-images.githubusercontent.com/77200870/203023247-f6bd26a0-aadb-4c36-bde4-87c5a1c777ae.png">

## mongoDB configuration

> Install mongoDB server community edition

> Add a /data/db folder in the root (for Windows C:)

> Add an environment variable to the `bin` folder in mongoDB installation folder in PATH

> `net stop MongoDB` to stop the running service and start it manually when needed

> run `mongod` if /data/db is in the root, else run `mongod --dbpath "C:\Program Files\MongoDB\Server\4.0\data\db"`

> run `mongo` to enter the mongo shell which is the environment where we can run commands against the database

## Starting with the shell

### Basic commands

#### List all available databases

> show dbs

#### Switch to a database

(even if does not exist it will be created on teh fly)

> use db_name

#### Delete a database

> use db_name

> db.dropDatabase()

#### Insert a document into a collection 

even if the collection does not exist it will be created

> db.collection_name.insertOne({name: "Max", age: 19})

#### List all documents in a collection

> db.collection_name.find()

(List them in a pretty way)

> db.collection_name.find().pretty()

#### Free a collection

> db.collection_name.deleteMany({})

#### Drop a collection

> db.collection_name.drop()

#### Get Statistics about the database

> use db_name

> db.stats()

## Shell vs Drivers

https://www.mongodb.com/docs/drivers/

- The Shell is language-neutral
- Drivers are ODMs (object-data-mapper) we use to interact with mongoDB from our applications written in diffrent languages (NodeJS, Python...)

## MongoDB + Clients the big picture

<img width="547" alt="Screenshot 2022-11-21 115136" src="https://user-images.githubusercontent.com/77200870/203032720-1e524e3c-b784-426d-8ad1-f2d7e7e1291b.png">

<img width="520" alt="Screenshot 2022-11-21 115322" src="https://user-images.githubusercontent.com/77200870/203032735-996357d3-d893-4af4-a4bb-50c68257b6d0.png">

-----------------------------------------------------------------------

# [02] Basics & Basic CRUD Operations

- Basics about collections & documents
- Basic data types
- Performing CRUD operations

## JSON vs BSON

<img width="540" alt="Screenshot 2022-11-21 122455" src="https://user-images.githubusercontent.com/77200870/203038752-42a1ec80-130c-4849-a548-9377f5ffcfe3.png">

### _id

- This field is automatically assigned by mongoDB, but we can override it with any value we want
- ObjectId is not a valid JSON data type, but mongoDB drivers convert it into a valid type in BSON format 

## CRUD Operations

<img width="531" alt="Screenshot 2022-11-21 211340" src="https://user-images.githubusercontent.com/77200870/203149804-a406500e-0115-4bfa-a446-e9e2eff5dc76.png">

### Updation

#### Update one document

> db.collection_name.updateOne({name: "yakoub"}, {$set: {name: "inv"}})

#### Update many documents

> db.collection_name.updateMany({age: 19}, {$set: {canDrive: true}})

#### Entirely replace the document

After this the document will contain only the `_id` and `name` fields

> db.collection_name.update({_id: ObjectId("3243fd4234r324545")}, {name: "Oussama"})

**⚠️ A better approach:**

> db.collection_name.replaceOne({_id: ObjectId("3243fd4234r324545")}, {name: "Oussama", age: 19})

### Deleteion

#### Delete one document

> db.collection_name.deleteOne({name: "Oussama"})

#### Delete many documents

> db.collection_name.deleteMany({})

### Insertion

#### Insert many documents

we pass an array of documents

> db.collection_name.insertMany([{...}, {...}, ...])

### Finding

#### Return all matching elements

> db.collection_name.find({distance: {$gt: 10000}}).pretty()

#### Return the first matching element

> db.collection_name.findOne({distance: {$gt: 10000}}).pretty()

**⚠️ find() and the Cursor**

> find() in reality returns a cursor to the data, not an array, and the shell by default only displays us with the first 20 documents, but if we want more documents, we run the `toArray()` method on the cursor or instead and better, we use the `forEach((data) => {prindjson(data)}) method`

## Projection

<img width="461" alt="Screenshot 2022-11-21 215122" src="https://user-images.githubusercontent.com/77200870/203155826-4cdf4c12-fa20-42d6-b38a-3fb5a4cb24bc.png">

> db.collection_name.find({}, {field1: 1, field2: 1})

⚠️ This will return the _id of each document as well, so we have to explicitly exclude the _id

> db.collection_name.find({}, {age: 19, _id: 0})

## Embedded Documents

<img width="526" alt="Screenshot 2022-11-21 215851" src="https://user-images.githubusercontent.com/77200870/203157035-66c3f22b-af56-458a-ba3e-c885ed97b867.png">

<img width="538" alt="Screenshot 2022-11-21 215918" src="https://user-images.githubusercontent.com/77200870/203157105-a8c18f23-5b2d-4126-8ac2-9a178ee6a312.png"> 

> db.allflights.updateMany({}, {$set: {captain: {name: "max", age: 25, hobbies: ["sports", "coding", "hacking"]}}

```js
[
  {
    _id: ObjectId("637bdf3779ed705cc635375a"),
    departureAirport: 'MUC',
    arrivalAirport: 'SFO',
    aircraft: 'Airbus A380',
    distance: 12000,
    intercontinental: true,
    captain: {
      name: 'max',
      age: 25,
      hobbies: [ 'sports', 'coding', 'hacking' ]
    }
  },
  {
    _id: ObjectId("637bdf3779ed705cc635375b"),
    departureAirport: 'LHR',
    arrivalAirport: 'TXL',
    aircraft: 'Airbus A320',
    distance: 950,
    intercontinental: false,
    captain: {
      name: 'max',
      age: 25,
      hobbies: [ 'sports', 'coding', 'hacking' ]
    }
  }
]
```

### Accessing embedded documents

> db.allflights.find({"captain.name": "max"})

## Module Summary

<img width="530" alt="Screenshot 2022-11-21 221655" src="https://user-images.githubusercontent.com/77200870/203159843-540c3a0a-9dad-4e41-9c05-63592d524b4b.png">

-------------------------------------------------------------------

# [03] Schemas & Relations How to Structure Documents

## What's In This Module

- Understading Document Schemas & Data Types
- Modelling Relashions
- Schema Validation

<img width="495" alt="Screenshot 2022-11-23 212122" src="https://user-images.githubusercontent.com/77200870/203639355-4203d68a-1313-4ecc-8a8a-41550ae12e8c.png">

we can have these two documents in the same collection!

```js
  {
    _id: ObjectId("637e80685a49faf9d87c5c59"),
    name: 'book',
    price: 12.99
  },
  {
    _id: ObjectId("637e808d5a49faf9d87c5c5a"),
    title: 't-shirt',
    seller: { name: 'oussama', age: 20 }
  }
```

> While it's not necessary to have a schema, it would be better to have one so that we work more effeciently with the data in the backend

## Available Approaches do structure collections

<img width="532" alt="Screenshot 2022-11-23 212554" src="https://user-images.githubusercontent.com/77200870/203640077-d685bde6-28b8-4c54-b261-268082733f5c.png">

> The cases in the middle and on the right are the ones we see more often

**⚠️ If we want to take a SQLish approach, we add the fields that are not common, in all the documents but if a document does not use it, we set it to `null`**

## Data Types

**⚠️ Max size of each document is 16MB**

- Text: "Yacoub"
- Boolean: true, false
- Number
  + Integer(int32)
  + NumberInt()
  + NumberLong(int64)
  + NumberDecimal (high precision floating point number)
- ObjectId
- ISODate
  + Timestamp
- Embedded document
- Arrays

<img width="521" alt="Screenshot 2022-11-23 214216" src="https://user-images.githubusercontent.com/77200870/203642568-123d1d64-25fa-4bd4-a100-95b2635586da.png">

**Eample:**

```js
db.company.insertOne({name: "Fresh Apples Inc", isStartup: true, employeesCount: 33, funding: 12345678901234567890, details: {ceo: "Mark"}, tags: [{title: "super"}, {title: "great"}], foundingDate: new Date(), insertedAt: new Timestamp()})
```

## Data Schemas & Data Modelling

<img width="544" alt="Screenshot 2022-11-23 220612" src="https://user-images.githubusercontent.com/77200870/203646064-20cc1f9b-18eb-4805-b3a5-0800e46bf57e.png">

## Relashions

### Two tastes of relashions

<img width="530" alt="Screenshot 2022-11-24 184035" src="https://user-images.githubusercontent.com/77200870/203841907-85158926-4c3b-454d-80f2-75ce3ea5104f.png">

<img width="530" alt="Screenshot 2022-11-23 220852" src="https://user-images.githubusercontent.com/77200870/203646428-7af7fb32-7af8-4d90-a4d4-4b003055fb58.png">

> If the data we want to refrence does not affect the document when changes, then we embed it, else we should only refrence it with the **id**

#### Example #1: One To One Relashion

**⚠️ Best Approach: Embedded refrenced document**

**⚠️ An Application Driven Approach:** For example, we have `users` and `cars` which is refrenced through `car's id` in the users documents, if our application needs to display all cars and all users seperatly, then it will be a lot more efficient to split the two collections apart

<img width="503" alt="Screenshot 2022-11-23 221144" src="https://user-images.githubusercontent.com/77200870/203646882-8e53f3cf-bc9f-402d-badd-3fc6111b83d2.png">

#### Example #2: One To Many Relashion

<img width="488" alt="Screenshot 2022-11-24 180954" src="https://user-images.githubusercontent.com/77200870/203837620-596d5a1d-4c57-478c-937f-b115b3d570ee.png">

**⚠️ Best Approach: Embedded refrenced document;** because we never fetch the answers alone without the thread, the thread always comes along with the answers.

#### Example #3: One To Many Relashion

<img width="482" alt="Screenshot 2022-11-24 181204" src="https://user-images.githubusercontent.com/77200870/203837934-2e1b1be9-1470-44bc-8a59-9813c59c4f1f.png">

**⚠️ Best Approach: Refrence documents;** because if we just want to fetch the cities for example we will not need to fetch all the citizens as well which can be a lot of data over the wire. So we just refrence the city for each citizen.

```js
db.cities.insertOne({name: "Baverlley", coor: {lat: 23, lng: 55}})
```

```js
db.citizens.insertMany([{name: "oussama", age: 20, cityId: ObjectId("637fa78b5a49faf9d87c5c61")}, {name: "yakoub", age: 25, cityId: ObjectId("637fa78b5a49faf9d87c5c61")}])
```

#### Example #4: Many To One Relashion

> Typically we model many-to-many relashions by refrences

> But, we should first ask, how often do we change our data, and when we change it do we have to change it everywhere.

<img width="483" alt="Screenshot 2022-11-24 182149" src="https://user-images.githubusercontent.com/77200870/203839356-36a6bd60-682e-475f-a716-8b761b46d732.png">

**⚠️ Best Approach: Either works;** because we don't need to have the latest version of the products in the `orders` array inside the `customer` document.

<img width="487" alt="Screenshot 2022-11-24 183027" src="https://user-images.githubusercontent.com/77200870/203840638-e07eef9d-fa7a-4df5-bbe9-c85e4e188125.png">

**⚠️ Best Approach: Refrence documents;** because we need the latest version of data.

## Merging Related Documents Split Up Using Refrences

### lookup()

<img width="446" alt="Screenshot 2022-11-24 184628" src="https://user-images.githubusercontent.com/77200870/203842748-03263640-1fd2-4e2e-b573-c0661b0e40ec.png">

**Authors**

```js
{
    _id: ObjectId("637fab1d5a49faf9d87c5c65"),
    name: 'robert',
    age: 30,
    address: { street: 'main', state: 'TX' }
  },
  {
    _id: ObjectId("637fab1d5a49faf9d87c5c66"),
    name: 'fellip',
    age: 40,
    address: { street: 'main-str', state: 'NY' }
  }
```

**Books**

```js
{
    _id: ObjectId("637faab45a49faf9d87c5c64"),
    title: 'cipher',
    authors: [
      ObjectId("637fab1d5a49faf9d87c5c65"),
      ObjectId("637fab1d5a49faf9d87c5c66")
    ]
}
```

**Merging**

```js
db.books.aggregate([
    {$lookup: {
                from: "authors", 
                localField: "authors", 
                foreignField: "_id", 
                as: "creators"
               }
    }])
```

**Result**

```js
{
    _id: ObjectId("637faab45a49faf9d87c5c64"),
    title: 'cipher',
    authors: [
      ObjectId("637fab1d5a49faf9d87c5c65"),
      ObjectId("637fab1d5a49faf9d87c5c66")
    ],
    creators: [
      {
        _id: ObjectId("637fab1d5a49faf9d87c5c66"),
        name: 'fellip',
        age: 40,
        address: { street: 'main-str', state: 'NY' }
      },
      {
        _id: ObjectId("637fab1d5a49faf9d87c5c65"),
        name: 'robert',
        age: 30,
        address: { street: 'main', state: 'TX' }
      }
    ]
}
```

## Schema Validation

<img width="363" alt="Screenshot 2022-11-24 210404" src="https://user-images.githubusercontent.com/77200870/203857455-9438c942-6fed-4166-b087-f5269e68937e.png">

<img width="527" alt="Screenshot 2022-11-24 210542" src="https://user-images.githubusercontent.com/77200870/203857593-713781a7-267f-4615-b92f-8f35cb84c7d2.png">

> db.createCollection(collection_name, {validation rules})

### Adding Schema Validation When Creating the Collection

```js
db.createCollection("posts", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["title", "text", "writer", "comments"],
            properties: {
                title: {
                    bsonType: "string",
                    description: "muest be a string and is required"
                },
                text: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                writer: {
                    bsonType: "objectId",
                    description: "must be an onjectId and is required"
                },
                comments: {
                    bsonType: "array",
                    description: "must be an array and is required",
                    items: {
                        bsonType: "object",
                        required: ["text", "author"],
                        properties: {
                            text: {
                                bsonType: "string",
                                description: "must be a string and is required"
                            },
                            author: {
                                bsonType: "objectId",
                                description: "muest be an objectId and is required"
                            }
                        }
                    }
                }
            }
        }
    }
})
```

### Adding Schema Validation After Creating the Collection

> db.runCommand({collMod: "collection_name", validator: {validation schma}, validationAction: "error"||"warn"})

**⚠️ When the `validationType` is set to `warn` then even if the document is not valid it gets inserted and a warning gets added to the file**

```js
db.runCommand({
    collMod: "posts",
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["title", "text", "writer", "comments"],
            properties: {
                title: {
                    bsonType: "string",
                    description: "muest be a string and is required"
                },
                text: {
                    bsonType: "string",
                    description: "must be a string and is required"
                },
                writer: {
                    bsonType: "objectId",
                    description: "must be an onjectId and is required"
                },
                comments: {
                    bsonType: "array",
                    description: "must be an array and is required",
                    items: {
                        bsonType: "object",
                        required: ["text", "author"],
                        properties: {
                            text: {
                                bsonType: "string",
                                description: "must be a string and is required"
                            },
                            author: {
                                bsonType: "objectId",
                                description: "muest be an objectId and is required"
                            }
                        }
                    }
                }
            }
        }
    },
    validationAction: "warn"
})
```

## Round Up

<img width="495" alt="Screenshot 2022-11-24 214809" src="https://user-images.githubusercontent.com/77200870/203861159-11167efd-bc59-4d38-ad3d-9c8ca62562f8.png">

<img width="530" alt="Screenshot 2022-11-24 215017" src="https://user-images.githubusercontent.com/77200870/203861172-50b2b1e7-291d-4074-b3bd-1058909be771.png">

----------------------------------------------------------------------------

# [04] The Shell & The Server

### What's inside this module?

- Start mongoDB server as a process or as a service
- Configuring database & log path (and mode)
- Fixing issues

## Server Configuration

> mongod --help

Change the path where the database files are saved

> mongod --dbpath path

Set the logs file

> mongod --logpath path/logs.log

Run the `mongod` server as a background process (works only on mac and linux)

> mongod --fork --logpath path

⚠️ For Windows, when we install the `mongod` server, the service is already running, to stop it we go to the task manager and end the service `MongoDB` or we run on the cmd as admin the command `net stop MongoDB` or we start `mongo` and run the command `db.shutdownServer()`, to start it again, we run the cmd as admin and run `net start MongoDB`

<br/>

> To add a configuration file for our settings, we go to bin and create a file `conf.cfg`

```js
systemLog:
    destination: file
    logAppend: true
    path: "C:\Program Files\MongoDB\Server\6.0\log\mongod.log"
storage:
    dbPath: "C:\Program Files\MongoDB\Server\6.0\data"
```

To use it

> mongod -f conf_path

## Shell Configuration

To get all the commands we can run on the database object

> db.help()

To get all the commands we can run on a collection

> db.test.help()

--------------------------------------------------------------------

# [05] Diving Deeper Into Document Creation

<img width="243" alt="Screenshot 2022-11-25 213053" src="https://user-images.githubusercontent.com/77200870/204052730-27b7be7f-ece6-4ca1-bafd-92024c372124.png">

## Creating Documents

<img width="526" alt="Screenshot 2022-11-25 213147" src="https://user-images.githubusercontent.com/77200870/204052785-e20a2a69-b620-4869-96ca-56a162cf0dad.png">

⚠️ insert() works with one document or an array of documents but it's prone to errors, and makes it hard to understand what's going in the code

<img width="523" alt="Screenshot 2022-11-25 213301" src="https://user-images.githubusercontent.com/77200870/204052885-48429369-b4e0-4de6-9909-5e43a2927d3c.png">

## Working With Ordered Inserts

When inserting multiple documents into the collection with `insertMany`, we can set an option which determines whether we continue inserting after a document fails to get inserted or not

> ordered: true        DON'T CONTINUE
> ordered: false       CONTINUE

```js
db.hobbies.insertMany([{ _id: "yoga", name: "Yoga" }, { _id: "cooking", name: "cooking" }, { _id: "food", name: "Food" }], {ordered: false})
```

## WriteConcern Option

<img width="533" alt="Screenshot 2022-11-25 215554" src="https://user-images.githubusercontent.com/77200870/204054542-4c2d0a5a-6560-4650-97ed-3f7c60a38017.png">

⚠️ `writeConcern` works for all insert, delete and update methods

**Syntax:** 

```js
{ w: <value>, j: <boolean>, wtimeout: <number> }
```

**Example:**
```js
db.persons.insertOne({name: "susan", age: 25}, {writeConcern: {w: 1, j: true, wtimeout: 200}})
```

## Atomicity

<img width="491" alt="Screenshot 2022-11-25 221232" src="https://user-images.githubusercontent.com/77200870/204055760-b58f10c7-6e5d-46b0-bfa5-798caebfc335.png">

## Importing Data

- jsonArray: we are importing an array of documents, not just one document
- drop: if a collection with this name already exists, then delete it and replace it with this new one

```js
mongoimport path_to_json_file -d movieDB -c movies --jsonArray --drop
```

## Read Operations

- Methods, Filters & Operators
- Query Selectors
- Projection Operations

### Query Selectors

#### [01] Comparision Operators

- $eq : equal
- $ne : not equal
- $lt : lower than
- $lte: lower than or equal
- $gt : greater than
- $gte : greater than or equal
- $in : find the matching documents where the field is in the specified array `db.movies.find({runtime: {$in: [30, 42]}})`
- $nin : find the matching documents where the field is not in the specified array

#### [02] Querying Embedded Fields

```js
db.movies.find({"rating.average": {$gt: 9}})
```

> we can go deep in the embedding level as much as we want

#### [03] Querying Arrays

```js
db.movies.find({genres: "Drama"})
```

> This means that "Drama" is included in the `genres` array

⚠️ For exactly matching an array, we add the []

```js
db.movies.find({genres: ["Drama"]})
```

#### [04] Logical Operators

- $or : `db.movies.find({$or: [{"rating.average": {$lt: 5}}, {"rating.average": {$gt: 9.3}}]})`
- $nor : neither of the conditions is met
- $and : `db.movies.find({$and: [{"rating.average": {$gt: 9}}, {genres: "Drama"}]})`

> The default behavior for find(...) is that it takes a bunch or filters and AND them, so why we have an `$and` operator

> We have it because if we want to do something like this `db.movies.find({genres: "Drama", genres: "Thrilling"})` we will get only the arrays that have 'Thrilling', the solution here is to use `$and` >> `db.movies.find({$and: [{genres: "Drama"}, {genres: "Thrilling"}]})`

- $not : look for the opposite or a query, `db.movies.find({runtime: {$not: {$eq: 60}}})`

#### [05] Element Operators

- $exists : check if a field exists >> `db.movies.find({age: {$exists: true}})`

	> Sometimes there are documents where the field exists but it's set to `null`, to bypass this >> `db.movies.find({age: {$exists: true, $ne: null}})`

- $type : the type of the field value >> `db.users.find({phone: {$type: "number"}})`

	> We can also pass an array of types to check any of them

	`db.users.find({phone: {$type: ["number", "string"]}})`

#### [06] Evaluation Operators

- $regex : look for a pattern in a text >> `db.movies.find({summary: {$regex: /musical/}})`
- $expr : compare two fields inside one document
- 
<img width="607" alt="Screenshot 2022-12-01 182912" src="https://user-images.githubusercontent.com/77200870/205120444-e4069eb0-b0e9-4a3c-8649-5c0174c08912.png">

```js
db.sales.find({$expr: {$gt: ["$volume", "$target"]}})
```

```js
db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 10]}, else: "$volume"}}, "$target"]}})
```

```js

let discountedPrice = {
   $cond: {
      if: { $gte: ["$qty", 100] },
      then: { $multiply: ["$price", NumberDecimal("0.50")] },
      else: { $multiply: ["$price", NumberDecimal("0.75")] }
   }
};
// Query the supplies collection using the aggregation expression
db.supplies.find( { $expr: { $lt:[ discountedPrice,  NumberDecimal("5") ] } });
```

**⚠️ Note: Even though $cond calculates an effective discounted price, that price is not reflected in the returned documents. Instead, the returned documents represent the matching documents in their original state.**

#### [07] Querying Arrays

We use `nested path syntax`

```js
db.users.find({"hobbies.title": "sports"})
```

#### [08] Array Query Selectors

- **$size:** matches any array with the number of elements specified by the argument (must be an exact number, we cannot pass a range of numbers or use `$gt`, `$lt`...)

```js
db.users.find({hobbies: {$size: 3}})
```

- **$all**

<img width="614" alt="Screenshot 2022-12-01 190503" src="https://user-images.githubusercontent.com/77200870/205127588-2c775ecd-74b1-4ac2-94e6-ac665711124a.png">

The following operation uses the `$all` operator to query the inventory collection for documents where the value of the tags field is an array whose elements include `appliance`, `school`, and `book`
 
```js
db.inventory.find( { tags: { $all: [ "appliance", "school", "book" ] } } )
```

