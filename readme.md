## mongodDB Setup
--
[mongo-db](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/) - Docs
[mongo-download](https://fastdl.mongodb.org/osx/mongodb-macos-x86_64-4.4.4.tgz) - Download mongoDB here


1. Once you have it in your download folder, enter the code below.

2. sudo mv '/Users/mcooper/Downloads/mongodb-macos-x86_64-4.4.4.tgz' /usr/local/mongodb

3. open /usr/local/mongodb to verify the directory now exists

4. Now we need to connect our binaries

5. cd ~ to got root of profile

#### IF your LOCAL DB aka mongod command stops working, do step 6 again ###

6. alias mongod="sudo mongod --dbpath /Users/mcooper/data/db"  // This sets the alias "mongod" to run our db from the specified path.

7. mongo --version from shell to confirm installation was successful. 


## mongoDB - CRUD Operations - CREATE
---

1. First we must make sure our DB is up and running
    - mongod from the terminal/shell will spin it up, confirm "listening on port 27017" is in the console.

2. Now we need to tap into the Mongo Shell next, lets open another terminal window.
    - mongo   without the d should bring a prompt witht he > bracket to confirm it worked.

3. This shell lets us interact with our local databases
    - help from the shell lets us see other commands
    - show dbs will show the default DBS, admin, config, local

4. Create our very first DB
    - use nameofDB  to create a DB
    - it wont show up, in show db until is has content/data!
    - use shopDB // For our tutorial

5. To show what DB your currently in, just type db

6. To create a collection we can type db.collection.insertOne({}) 
    - db.products.insertOne({
        name: "sue",
        age: 26,
        status: "pending"
    });

7. We insert an javaScript Object to add our item to the DB and its properties.

8. Lets add a pen from our previous module using the mongo code
    -db.products.insertOne({id: 1, name: "Pen", price: 1.20})


9. Now if we type show collections, we should have our newly created collection 'products'

10. Now lets add our 'Pencil' from previous module and add the stock columns
    - db.products.insertOne({id: 2, name: "Pencil", price: 0.80, stock: 12})

## mongoDB - CRUD Operations - Read
---

[mongodb-query-operators](https://docs.mongodb.com/manual/reference/operator/query/)

1. To read from the collection we just created we can use the command below
    - db.collection.find()    //Template command; 
    - db.products.find()    // This finds everything in the collection

    - db.collection.find(query, projection) // Template command

2. Query Operators let you seach more granularly and find more specific data
    
    Comparison Operators
    For comparison of different BSON type values, see the specified BSON comparison order.

    Name	Description
    $eq	Matches values that are equal to a specified value.
    $gt	Matches values that are greater than a specified value.
    $gte	Matches values that are greater than or equal to a specified value.
    $in	Matches any of the values specified in an array.
    $lt	Matches values that are less than a specified value.
    $lte	Matches values that are less than or equal to a specified value.
    $ne	Matches all values that are not equal to a specified value.
    $nin	Matches none of the values specified in an array.

    Logical
    Name	Description
    $and	Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
    $not	Inverts the effect of a query expression and returns documents that do not match the query expression.
    $nor	Joins query clauses with a logical NOR returns all documents that fail to match both clauses.
    $or	Joins query clauses with a logical OR returns all documents that match the conditions of either clause.

    - db.products.find({name: "Pencil"})  // This will find us all items with the name 'Pencil'

    - db.products.find({price: {$gt: 1}}) // This will seach for all items with a price thats 'Greater Than" 1. Another nested object with the $GT flag.

3. Projections allow us to specify what fields/columns are returned from our documents that meet the query criteria
    - db.users.find({   // Basic Command to find
        {age: { $gt 18}},   // Nested objects for age query, then another for query operator
        {name: 1, addresss: 1} // The next object is for projection, lets you specify to show just the name and addresses of the query documents
    }).limit(5)  // This lets you return a specified amount, 5 in this example

    - ID will always comeback unless you add the projection {_id: 0}


## mongoDB - CRUD Operations - Update
---

1. db.products.updateOne({id: 1}, {$set: {stock: 32}}) // To updateOne product, in the first obeject we select our prodcut, while in the second object we use the $set: {stock:32}

2. db.products.find() // To show all the products and columns, and you will see our id 1now has a stock and value



## mongoDB - CRUD Operations - Delete
---

[mongoDB-Delete](https://docs.mongodb.com/manual/crud/#delete-operations)

1. db.users.deleteOne({status: "reject"})  // Template, users is the collection, status: "reject" is the filter
    - Delete Operation Filters use the same syntax as read operations

2. For your challenge, delete the "pencil" item from the DB using the mongoDB documentation
    - db.products.find({id: 2})  // Since the syntax is the same as FIND, lets find it first with this code
    - db.products.deleteOne({id: 2}) // Now we can run this code to deleteOne and it should delete the pencil
    - { "acknowledged" : true, "deletedCount" : 1 }  // Successful Deletion
    - db.products.find() // confirm


## mongoDB Relationships with mongoDB
---
Embedding Documents - Aka documents inside documents or objects inside objects

1. In our product below we can see we have a reviews array with another json object that has more data about the product.

2. This lets us establish relationships between data that are related.

3. If we add this code below to our mongo shell and enter, we can db.products.find() and see we know have items with id 1 and 3

```
db.products.insert({
    _id: 3,
    name: 'Rubber',
    price: 1.30,
    stock: 43,
    reviews: [
        {
            authorName: "Sally",
            rating: 5,
            review: "Best Rubber Ever!"
        },
        {
            authorName: "John",
            rating: 5,
            review: "Awesome Rubber!"
        }
    ]
})

```


## Embedding Documents Challenge
--

1. For this challenge lets add back in our "Pencil" item, with embedded document, reviews.

```
db.products.insert({
    _id: 2,
    name: 'Pencil',
    price: 0.80,
    stock: 12,
    reviews: [
        {
            authorName: "Mike",
            rating: 2,
            review: "Shitty Pencil Dude!"
        },
        {
            authorName: "Dick",
            rating: 5,
            review: "It just works!"
        }
    ]
})

```


## mongoDB with the Native Mongo Driver
---

[MongoDB-Docs](https://mongodb.github.io/node-mongodb-native/3.6/tutorials/connect/)

1.  The Native MongoDB Driver is needed to understand but most will end up using mongoose. But it is a good practice to understand how it works.
    - Mongoose cuts back on how much code is actually needed to incorporate

2.  The Driver is what allows mongoDB to interact with our application
    - Depending on what languagte your app was developed in, will determine which Driver you need

3. We will use the native mongoDB Node.js driver

4. Create a new project, create a FruitsProject Directory

5. Touch App.js from the FruitsProject directory

6. npm init -y to initialize and auto yes the prompts

7. npm install mongodb 

8. Connect with the boilerplate code below

```
const MongoClient = require('mongodb').MongoClient;
const assert = require('assert');

// Connection URL
const url = 'mongodb://localhost:27017';

// Database Name
const dbName = 'fruitDb';

// Create a new MongoClient
const client = new MongoClient(url);

// Use connect method to connect to the Server
client.connect(function(err) {
  assert.equal(null, err);
  console.log("Connected successfully to server");

  const db = client.db(dbName);

  client.close();
});

```

9. Type your command in the consode to start the mongo shell, mongod or mongo (i have it bound to mongo)
    - node app.js from bash to see if we can connect

10. add insertMany code to end of app.js
```
const insertDocuments = function(db, callback) {
    // Get the documents collection
    const collection = db.collection('fruitDB');
    // Insert some documents
    collection.insertMany([
        {name : "Apple",
        score : 8,
        review: "Great Fruit"},
        {name : "Apple",
        score : 6,
        review: "Kinda Sour"},
        {name : "Apple",
        score : 9,
        review: "Great Stuff"},
    ], function(err, result) {
      assert.equal(err, null);
      assert.equal(3, result.result.n);
      assert.equal(3, result.ops.length);
      console.log("Inserted 3 documents into the collection");
      callback(result);
    });
  }
```

11. Once we've add the insertMany items, we can add the insertDocuments function to the connection. Replace client.close() with ALL the code below.
  
  ```
    insertDocuments(db, function() {
    client.close();
  });
  ```

12. Now lets save and run node app.js again and we should get 
    - "Sucessfully connected to server"
    - "Inserted 3 Documents into the server"

13. show dbs, use fruitDb, db.fruitsDb() // These steps will confirm our work was successfull.


## Inserting Documents
-- 

1. Copy the code to insert from the documenation
```
 const findDocuments = function(db, callback) {
    // Get the documents collection
    const collection = db.collection('FruitsDb');
    // Find some documents
    collection.find({}).toArray(function(err, fruit) {
      assert.equal(err, null);
      console.log("Found the following records");
      console.log(fruit)
      callback(docs);
    });
  }
```

2. add the findDocuments function to the client.connect

```
findDocuments(db, function(){
      client.close()
  })
```

3. We can leave the previous function as its not being called in the client connect, but we must be aware what will be called everytime its ran. 

4. As you can see the Native MongoDB driver is quite wordy. This is why mongoose is used. Lets jump to the next module and figure out how much easier it can be.