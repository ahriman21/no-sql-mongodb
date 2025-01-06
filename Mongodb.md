MongoDB is a document-oriented No-SQL database designed for flexibility, scalability, and ease of development. It stores data in JSON-like documents, allowing for a dynamic schema, which means fields can vary from document to document. The database supports rich queries, indexing, and aggregation, making data retrieval efficient.
MongoDB's architecture is built for horizontal scaling, enabling it to handle large volumes of data and high throughput. It is widely used for its ability to quickly adapt to changing data models and support modern application development. You can find the MongoDB manual in this [link](https://www.mongodb.com/docs/manual/)


> [! Table of Content]
> - [[#Collections]]
> - [[#CRUD Operations]]
> - [[#Data Types in MongoDB]]
> - [[#Embedded documents]]
> - [[#Working With Arrays in MongoDB]]
> - [[#Get Query Options (Searching)]]
> - [[#Relationships]]
> - [[#Strict Schema (Input Validations)]]
> - [[#Deeper Concepts In MongoDB]]
> 

### Collections
In MongoDB, a `collection` is a set of relevant data grouped together to represent an object. For instance collection of ``customers`` includes data about customers.
each which represents key values for a customer is called `document`.

- Display list of databases
  ```bash
	show dbs
	```
- Choose a database
  ```
	use <db name>
	```
- Display list of collection of the database
  ```
	show collections
	```
- choose the collection to apply methods on it
  ```
	db.<collection-name>.<method name>()
	```



---


### CRUD Operations
You can use following commands to Create, Read, Update, Delete an object to/from a collection.

![[Pasted image 20240812115221.png]]
##### 1- CREATE
creating an object using `insertOne` and `insertMany` methods.

- insert one object:
```js
db.collectionName.insertOne({ field1: "value1", field2: "value2" });
```

- insert many objects at once (remember to put JSONs in brackets): 
```js
db.collectionName.insertMany([
  { field1: "value1", field2: "value2" },
  { field1: "value3", field2: "value4" }
]);
```


##### 2- READ
To read or fetch data in MongoDB, you can use the `find` and `findOne` methods:

- **`find`**: Retrieves multiple documents that match the specified criteria.

  ```javascript
  // Fetch all documents
  db.collectionName.find();

  // Fetch documents with a specific condition
  db.collectionName.find({ field1: "value1" });
  ```

- **`findOne`**: Retrieves a single document that matches the specified criteria.

  ```javascript
  // Fetch the first document with a specific condition
  db.collectionName.findOne({ field1: "value1" });
  ```

You can also use additional options and methods for filtering, sorting, and limiting the results.


##### 3- UPDATE
In MongoDB, you can update documents using `updateOne`, `updateMany`, and `findOneAndUpdate`. Here’s how they work:

- **`updateOne`**: Updates the first document that matches the filter.

  ```javascript
  db.collectionName.updateOne(
    { field1: "value1" },         // Filter
    { $set: { field2: "newValue" } } // Update
  );
  ```

- **`updateMany`**: Updates all documents that match the filter.

  ```javascript
  db.collectionName.updateMany(
    { field1: "value1" },         // Filter
    { $set: { field2: "newValue" } } // Update
  );
  ```

- **`findOneAndUpdate`**: Finds a document and updates it, returning the document before or after the update.

  ```javascript
  db.collectionName.findOneAndUpdate(
    { field1: "value1" },         // Filter
    { $set: { field2: "newValue" } }, // Update
    { returnOriginal: false }     // Options: return the updated document
  );
  ```

> [!NOTE]
> --> Update Operators:
> 
> - **`$set`**: Sets the value of a field.
> - **`$inc`**: Increments the value of a field.
> - **`$unset`**: Removes a field from the document.
> - **`$push`**: Adds an element to an array field.
> - **`$addToSet`**: Adds an element to an array field only if it doesn't exist.
> 
> 

##### 4- DELETE
To delete documents in MongoDB, you can use `deleteOne`, `deleteMany`, or `findOneAndDelete`. Here’s how each method works:

- **`deleteOne`**: Deletes the first document that matches the filter.

  ```javascript
  db.collectionName.deleteOne({ field1: "value1" }); // Filter
  ```

- **`deleteMany`**: Deletes all documents that match the filter.

  ```javascript
  db.collectionName.deleteMany({ field1: "value1" }); // Filter
  ```

- **`findOneAndDelete`**: Finds and deletes a single document, returning the deleted document.

  ```javascript
  db.collectionName.findOneAndDelete({ field1: "value1" }); // Filter
  ```


> [!NOTE]
> --> Points to Note
> 
> - **Filter Criteria**: The filter criteria determine which documents are deleted. Be careful with the filter to avoid accidental deletions.
> - **Return Values**: `deleteOne` and `deleteMany` return information about the operation, such as the number of documents deleted. `findOneAndDelete` returns the deleted document itself.
> - **Use Cases**: Choose the appropriate method based on whether you need to delete one, multiple, or a specific document and need a response with the document.



---


### Data Types in MongoDB

1.  `string` which is represented in double quotes.

2.  `numeric` which includes numbers like int, flout, decimal.
```js
{ highPrecisionValue: NumberDecimal("123.456789123456789") }
```

3. `date` use it like :
```js
  // 1
  // simple string representing year, month, day
  {date: new Date("2023-12-30")}
  
  // 2
  // Year, Month (0-11), Day, Hour, Minute, Second
  {date: new Date(2024, 7, 12, 10, 0, 0)} 
  
  // 3
  // Current date and time
  {date: new Date()}
  
```

4. `boolean` which is either `true` or `false`.

5.  `array` it's like `["Iran", "Tajikstan",]`.

6. `ObjectId` is a datatype for `_id` field which normally is a unique value.
```js
{ _id: ObjectId("507f191e810c19729de860ea") }
```



---


### Embedded documents
Embedded document is a nested JSON object inside another JSON object. In this example, `address` is an embedded document:

```js
{
  "_id": ObjectId("user_id"),
  "name": "John Doe",
  "addresses": [
    {
      "street": "123 Main St",
      "city": "New York",
    },
    {
      "street": "456 Market St",
      "city": "San Francisco",
    }
  ]
}

```

##### Querying Embedded Documents
- querying a __specific field in the embedded document__:
```js
db.users.find({ "addresses.city": "New York" });
```

- query for a __specific embedded document__:
```js
db.users.find({ "addresses": { "street": "123 Main St", "city": "New York", "state": "NY", "zip": "10001" } });

```


---


### Working With Arrays in MongoDB
How to work with fields which are type of arrays.

##### 1- Inserting Documents with Arrays
You can include arrays directly in your documents when inserting them.

```javascript
db.collectionName.insertOne({
  name: "Alice",
  skills: ["JavaScript", "Python", "MongoDB"]
});
```

#####  2- Querying Arrays
MongoDB offers various ways to query documents that contain arrays.

- Find documents where the array contains a specific value.
```javascript
db.collectionName.find({ skills: "JavaScript" });
```

- Find documents where the array contains all specified values.
```javascript
db.collectionName.find({ skills: { $all: ["JavaScript", "MongoDB"] } });
```

- Query Using Array Index
	Find documents where a specific position in the array matches a value.
```javascript
db.collectionName.find({ "skills.0": "JavaScript" });
```

- Query for Array Length
	Find documents where the array has a specific length.
```javascript
db.collectionName.find({ skills: { $size: 3 } });
```

##### 3- Updating Arrays
MongoDB provides several operators to modify arrays in a document.

- Add Elements to an Array
	Add elements to an array using the `$push` operator.
```javascript
db.collectionName.updateOne(
  { _id: "3221" }, // filter query.
  { $push: { skills: "C++" } } // update the skills aray.
);
```

- Add Unique Elements to an Array
	Use the `$addToSet` operator to add unique elements to an array.
```javascript
db.collectionName.updateOne(
  { _id: "3221" }, // filter query.
  { $addToSet: { skills: "Python" } } // Won't add "Python" again if it's already there
);
```

- Add Multiple Elements to an Array
	Add multiple elements to an array at once.
```javascript
db.collectionName.updateOne(
  { _id: "3221" }, // filter query.
  { $push: { skills: { $each: ["Ruby", "Go"] } } }
);
```

-  Remove Elements from an Array
	Remove elements from an array using the `$pull` operator.
```javascript
db.collectionName.updateOne(
  { _id: "3221" }, // filter query.
  { $pull: { skills: "Python" } }
);
```



---


### Get Query Options (Searching)
##### 1. **Comparison Operators**

- **Equal to**: `db.collectionName.find({ age: 25 });`
- **Not equal to**: `db.collectionName.find({ age: { $ne: 25 } });`
- **Greater than**: `db.collectionName.find({ age: { $gt: 25 } });`
- **Greater than or equal to**: `db.collectionName.find({ age: { $gte: 25 } });`
- **Less than**: `db.collectionName.find({ age: { $lt: 25 } });`
- **Less than or equal to**: `db.collectionName.find({ age: { $lte: 25 } });`

##### 2. **Logical Operators**
- **AND**: Combine multiple conditions using implicit AND.
```js
db.collectionName.find({ age: { $gt: 25 }, status: "active" });
```

- **OR**: Match documents that satisfy any of the specified conditions.
```js
db.collectionName.find({ $or: [{ age: { $lt: 25 } }, { status: "inactive" }] });
```

- **NOR**: Match documents that do not satisfy any of the specified conditions.
```js
db.collectionName.find({ $nor: [{ age: { $lt: 25 } }, { status: "inactive" }] });
```

- **NOT**: Negate a condition.
```js
db.collectionName.find({ age: { $not: { $gt: 25 } } });
```

##### 3. **Element Operators**
- **Exists**: Check if a field exists.
```js
db.collectionName.find({ fieldName: { $exists: true } });
```

- **Type**: Match documents where the field is of a specific type.
```js
db.collectionName.find({ fieldName: { $type: "string" } });
```

##### 4. Aggregation / Sort / Limit / Joins
- ==Sorting and limiting==
```js
db.posts.find({ status: "published" })  // Filter for published posts.
    .sort({ pub_date: -1 })      // Sort by `pub_date` in descending order.
    .limit(9);                  // Limit the results to 9 documents.

```


- ==Join between collections==
	Here’s how you can use the `$lookup` stage in an aggregation pipeline to join these two collections:
```js
db.orders.aggregate([
  {
    $lookup:
      {
        from: "orderDetails",       // The foreign collection name
        localField: "orderId",      // Field from the local collection
        foreignField: "orderId",    // Field from the foreign collection
        as: "orderDetails"          // Output array field
      }
  }
]);

```

advanced lookup : You can further customize the join by adding additional stages to the aggregation pipeline, such as:

- **`$match`**: Filter the documents before or after the join.
- **`$project`**: Select or exclude specific fields from the output.
- **`$unwind`**: Deconstruct the array resulting from `$lookup` into separate documents.
```js
db.orders.aggregate([
  {
    $lookup:
      {
        from: "orderDetails",
        localField: "orderId",
        foreignField: "orderId",
        as: "orderDetails"
      }
  },
  {
    $unwind: "$orderDetails"  // Unwind the orderDetails array
  },
  {
    $match: { "orderDetails.quantity": { $gt: 1 } }  // Filter for quantity > 1
  },
  {
    $project: { "customerName": 1, "orderDetails.product": 1 }  // Select specific fields
  }
]);

```


- ==Group by==
  query:
```js
// group-by query:
db.sales.aggregate([
  {
    $group: {
      _id: "$product", // group by the 'product' field.
      totalQuantity: { $sum: "$quantity" }, 
      totalSales: { $sum: { $multiply: ["$quantity", "$price"] } },
      averagePrice: { $avg: "$price" }
    }
  }
]);

```

result of group-by query:
```js
// result of group-by query
[
  { "_id": "Widget", "totalQuantity": 5, "totalSales": 125, "averagePrice": 25 },
  { "_id": "Gadget", "totalQuantity": 2, "totalSales": 100, "averagePrice": 50 },
  { "_id": "Doohick", "totalQuantity": 1, "totalSales": 75, "averagePrice": 75 }
]

```

---


### Relationships
In **MongoDB**, managing relationships between data is crucial for structuring and querying databases effectively. Relationships can be handled using ***embedded documents, references***, each offering different advantages depending on the use case.


- ***==Embedded Documents==****: This approach stores related data within a single document, ideal for data that is frequently accessed together and mostly unique for the parent data. It simplifies data retrieval and ensures ***data locality****.

> [!example of embedded relationship]
> - In this model, the documents are embedded inside one document. ****For example****, we have two documents one is a student(which contains the basic information of the ****student**** like ****id, name branch****) and another is an address document(which contains the ****address**** of the ****student****).
> - So, instead of creating two different documents, we embed the address documents inside the student document.
> - It will help the user to retrieve the data using a single query rather than writing a bunch of queries.


  
- **==Reference Model==****: This method involves storing references to related documents using unique identifiers, suitable for large or independently accessed data. It allows for normalization and maintains ***data consistency****.

> [!Example of Reference model]
> - In this model, we maintain the documents separately but one document contains the reference of the other documents.
> - ****For example****, we have two documents one is a student(which contains the basic information of the student like id, name branch) and another is an address document(which contains the address of the student).
> - So, here student document contains the reference to the address document’s id field. Now using this reference id we can query the address and get the address of the student.
> - This model is generally used to design the normalized relationships.


##### One-to-One Relationship using embedded document
in the example of `student` and `address`, each student document has one address document:
```js
// example of one-to-one relationship between student and address
{  
    StudentName: GeeksA,  
    StudentId: g_f_g_1209,  
    Branch:CSE  
    Address:{  // this is the embedded one-to-one relationship
        PermanentAddress: XXXXXXX,  
        City: Delhi,  
        PinCode:202333  
    }   
}
```


##### One-to-Many Relationship using embedded document
In the example of `author` and `books`,
```js
{
author_name: 'Ali',
author_age: 43
books:[
	{
		book_name: 'somthing',
		publish_year: 2004
	},
	{
		book_name: 'soemthing else',
		publish_year: 2007
	}
]
}
```


##### One-to-One relationship using reference model
In MongoDB, a one-to-one relationship using the reference model can be established by storing a reference (typically an `ObjectID`) from one document in another document.
Here we have the example of `users` and `userProfiles`:
```js
// users collection
{
  "_id": ObjectId("64c13f2f1023e827ef56bf4a"),
  "name": "John Doe",
  "email": "john.doe@example.com",
}
```

```js
// userProfiles collection
{
  "_id": ObjectId("64c13f411023e827ef56bf4b"),
  "address": "123 Main St",
  "phoneNumber": "123-456-7890",
  "userId": ObjectId("64c13f2f1023e827ef56bf4a") // Reference to the user
}

```

> [!tip]
> **Ensure Unique Profiles**: You should ensure that each user has only one profile. This can be done by creating a unique index on the `userId` field in the `profiles` collection. :
> ```js
> db.profiles.createIndex({ userId: 1 }, { unique: true });

--> make query for one-to-one relationship:
```js
db.users.aggregate([
  {
    $lookup: {
      from: "profiles",           // The collection to join with
      localField: "_id",          // Field from the `users` collection
      foreignField: "userId",     // Field from the `profiles` collection
      as: "profile"               // Name of the array field to add
    }
  },
  {
    $unwind: "$profile"   // Deconstructs the profile array to a single object
  }
]);

```

##### One-to-Many relationship using reference model
Let's consider a real-world example of a blog application with `categories` and `posts` collections:

- **`categories` Collection**: Stores information about different categories.
- **`posts` Collection**: Stores blog posts, each of which belongs to a single category.

```js
// category collection
{
  "_id": ObjectId("5f8d0d55b54764421b7156e9"),
  "name": "Technology"
}


```

```js
// posts collection
{
  "_id": ObjectId("5f8d0d5ab54764421b7156ea"),
  "title": "Latest Tech Trends",
  "content": "Content of the post",
  "categoryId": ObjectId("5f8d0d55b54764421b7156e9")// reference to the category
}


```

--> fetching posts with specific category:
```js
db.posts.find({ categoryId: ObjectId("5f8d0d55b54764421b7156e9") });

```

--> fetching categories with their posts:
```js
db.categories.aggregate([
  {
    $lookup: {
      from: "posts",               // The collection to join with
      localField: "_id",           // Field from the `categories` collection
      foreignField: "categoryId",  // Field from the `posts` collection
      as: "posts"                  // Name of the array field to add
    }
  }
]);

```


##### Many-To-Many relationships in mongodb
Intermediate collection approach involves creating a third collection to act as an intermediate entity. This is often the most flexible method, especially when the relationship itself needs additional attributes.

- **Product Collection:**
```js
// product collection
{
  "_id": ObjectId("product_id"),
  "name": "Product Name",
  "description": "Product Description",
  "price": 100.0
}

```

- **OrderDetail Collection:**
```js
// orderDetails collection
{
  "_id": ObjectId("order_detail_id"),
  "order_id": ObjectId("order_id"),
  "order_date": ISODate("2024-08-10T00:00:00Z")
}

```

- **ProductOrderDetails Collection:**
```js
// ProductOrderDetails Collection
{
  "_id": ObjectId("product_order_detail_id"),
  "product_id": ObjectId("product_id"),
  "order_detail_id": ObjectId("order_detail_id"),
  "quantity": 5
}

```

--> insertion example:
```js
db.productOrderDetails.insertOne({
  "product_id": ObjectId("product_id"),
  "order_detail_id": ObjectId("order_detail_id"),
  "quantity": 5
});

```

--> querying data example:
```js
db.productOrderDetails.aggregate([
  { $match: { product_id: ObjectId("product_id") } },
  {
    $lookup: {
      from: "orderDetails",
      localField: "order_detail_id",
      foreignField: "_id",
      as: "order_details"
    }
  }
]);

```

`$match` : In the given example, the `$match` stage is used in the MongoDB aggregation pipeline to filter documents from the `productOrderDetails` collection based on specific criteria before they are passed on to the next stages in the pipeline. This is often done to reduce the amount of data processed in subsequent stages, which can improve performance.

---


### Strict Schema (Input Validations)
Schema validation in MongoDB is a feature that allows you to enforce certain rules or constraints on the structure and content of the documents in a collection. This helps maintain data integrity and consistency by ensuring that all documents in a collection adhere to a predefined schema. MongoDB uses JSON Schema, a powerful schema language that can express a wide range of validation rules.

##### 1- schema definition
create the schema of a collection before inserting into it.
```js
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        email: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        age: {
          bsonType: "int",
          minimum: 18,
          description: "must be an integer greater than or equal to 18"
        }
      }
    }
  },
  validationLevel: "strict",
  validationAction: "error"
});
```

> [!NOTE]
> **Breakdown of the example:**
> - **`bsonType`**: Specifies the data type of the field. MongoDB supports several BSON data types.
> - **`required`**: Specifies the fields that must be present in every document.
> - **`properties`**: Defines the rules for each field in the document.
> - **`validationLevel`**: Set to `"strict"`, indicating that all inserts and updates are validated.
> - **`validationAction`**: Set to `"error"`, meaning that any document not conforming to the schema is rejected with an error.


##### 2- validation levels and validation actions
In MongoDB, when defining schema validation, you can set the `validationLevel` and `validationAction` to control how strict the validation is and what action to take when a document does not conform to the schema. Here are the possible values for each:

1. ==validation level:==
   - `strict` : all operations must be validated or it will throw an error.
   - `moderate` : validates the new documents but  in the update operation.
   - 
1. ==validation action:==
   - `error` : If a document does not conform to the schema, the operation (insert or update) is rejected, and an error is returned.
   - `warn`: If a document does not conform to the schema, the operation is allowed, but a warning is logged.



---

### Deeper Concepts In MongoDB

##### WriteConcern for Insertion
Right after inserting data into the mongodb server, by default the server returns an acknowledgement. If you had replica or secondary servers for data to be stored in the replica servers as well in addition to your primary server, you could modify the `WriteConcern` to change this behavior.

```js
db.customers.insertOne({name: "Pouri", age: 20}, {writeConcern: {w: 1}})
```

options of `WriteConcern` while inserting:
- `{writeConcern: {w: "majority"}}` --> By default, when you sent the insertion to the primary server, and if that document has been inserted in the primary server, and majority of the secondary server, then acknowledgement will be sent to the client.

-  `{writeConcern: {w: 1}}`: In that case, when the primary received the document and stored it in the primary server, then it will send the acknowledgement to the client. Even though the document hasn't been inserted in the secondary servers. The problem is we won't know if the data is stored in the replica servers or not.

- `{writeConcern: {w: n}}` : If you want to receive the acknowledgment after data was stored in the primary server and all the replica servers you can place the number of your replica servers.

- `{writeConcern: {w: 0}}`: In this case, the client won't wait for the acknowledgment after sending data to the servers.

##### Journal in mongodb
- **Definition**: The journal is a file or set of files on disk where MongoDB writes a sequential log of all write operations that are about to be applied to the database.
- **Purpose**: Its primary purpose is to ensure data durability. If the server crashes or loses power, the journal can be used to recover the database to a consistent state.

**How does journal work:**
- **Write Operations**: When a write operation (insert, update, or delete) is issued, MongoDB writes the operation to the journal before applying it to the actual data files. This is done to ensure that even if the system crashes immediately after the write operation is requested, the database can recover.
    
- **Commit to Disk**: MongoDB periodically (usually every 100 milliseconds by default) writes the journal data from memory to disk. This is known as a "commit" and is controlled by the `journalCommitInterval` setting.
    
- **Crash Recovery**: If MongoDB restarts after a crash, it uses the journal to replay the write operations that were committed to the journal but not applied to the data files before the crash. This ensures that no data is lost and the database remains consistent.


##### Atomicity in mongodb
Atomicity in MongoDB refers to the guarantee that a set of operations either completes fully or does not happen at all. This property is crucial in maintaining data integrity.

**Atomic Operations**: MongoDB ensures that operations on a single document are atomic. This means that any modifications (inserts, updates, or deletes) to a single document are executed as a single, indivisible unit of work. If an update operation modifies multiple fields of a document, all of those changes will either be applied or none of them will.

**example**:
- single document atomicity :  if one field of a document was failed in CRUD operation, the whole document would be failed to be stored.
  
- multiple document atomicity : if you did `insertMany` operation, the first document that fails, the document after that will be ignored to be stored as well.