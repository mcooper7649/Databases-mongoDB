## mongodDB Setup
--
[mongo-db](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/) - Docs
[mongo-download](https://fastdl.mongodb.org/osx/mongodb-macos-x86_64-4.4.4.tgz) - Download mongoDB here


1. Once you have it in your download folder, enter the code below.

2. sudo mv '/Users/mcooper/Downloads/mongodb-macos-x86_64-4.4.4.tgz' /usr/local/mongodb

3. open /usr/local/mongodb to verify the directory now exists

4. Now we need to connect our binaries

5. cd ~ to got root of profile

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

11. 