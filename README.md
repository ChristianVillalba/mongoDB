# MongoDB
Introduction to MongoDB

Notes from MongoDB module.     
[The Complete 2022 Web Development Bootcamp](https://www.udemy.com/course/the-complete-web-development-bootcamp/)  
Instructor: Dr. Angela Yu   

## Set Up MongoDB
#### Install MongoDB
Link [Dowload MongoDB (Community Server)](https://www.mongodb.com/try/download/community)            
Install MongoDB with the Installation Wizard

#### Setup Alias Shortcuts for Mongo and Mongod
Open up your Hyper terminal running Git Bash.      
```
cd ~
touch .bash_profile
vim .bash_profile
```
In vim, hit the I key on the keyboard to enter insert mode:    
```
alias mongod="/c/Program\ files/MongoDB/Server/5.0/bin/mongod.exe"
alias mongo="/c/Program\ Files/MongoDB/Server/5.0/bin/mongo.exe"
```
The number 5.0 is the version. Change to current version if neccessary.       
Hit the Escape key on your keyboard to exit the insert mode. Type to save and exit Vim:
```
:wq!
```

## Open MongoDB
To run a Mongo Server. In the (Hyper) Terminal type:
```
mongod
```
The our local database will be run on port 27017.      
Then we can open up a brand new Terminal. And this is a different connection
This terminal is connected to the MongoDB database and we can tap into the **Mongo Shell**:
```
mongo
```
The **Mongo Shell** is basically just a way for us to be able to interact with 
our MongoDB databases on our local system using the command line.

## Check the Databases we already have in our system:
In the Terminal:        
`help` (if neccessary) > `show dbs`
It will show 3 preloaded databases: adming, confg & local.         

## Basic Commands:
[MongoDB Documentation](https://docs.mongodb.com/manual/)  

Create Database          
```
use myDatabaseName
```       
Show my current Database         
```
db
```        
### CRUD Operations: [CRUD Operations Documentation](https://docs.mongodb.com/manual/crud/)      
**Create Operations**
```
db.collection.insertOne()
db.collection.insertMany()
```
`collection` is actually the name of a collection.        

Create or insert operations add new documents to a collection.        
If the collection does not currently exist, insert operations will create the collection.     
example:
```
db.users.insertOne(
     {
          name: "Cristian"
          age: 81
          status: "confused"
     }
)
```
Collections in MongoDB is kind of similar to tables in the SQL world.       
They are a collection of documents and a document is simply just a single data record,    
so that would be a single row.      
```
> db.products.insertOne({_id:1, name: "Pen", price: 1.2  })
{ "acknowledged" : true, "insertedId" : 1 }
```
When the data is added to the database, we can show the collections:
```
show collections  
```
**Retrive Operations: Reading & Queries**
```
db.collection.find(query, projection)
```
Both parameters, query & prjection are optional. Example:
```
db.products.find()
```
Will display all the products we put in our database.  

The parameters will narrow down on the data we are going to get back.
```
db.products.find({price: {$gt: 1}, {name: 1}})
```
Using Query will display only the products with a price ($gt:1) grater than 1.      
Using the projection parameter will be display only the specifed field we are looking for:      
The id will be displayed always by default.       
`_id:0` as second parameter in projection will avoid this. `{name: 1 , _id:0}`

**Update Operations**

```
db.products.updateOne({_id:1}, {$set:{stock:32} })
```
The first parameter is which record I want to update (I target the `_id`)     
The second one is the information that will be updated.       
In this case, we use `$set`to add a new field.

**Update Operations**
```
db.collection.deleteOne()
db.collection.deleteMany() 
```
You can specify criteria, or filters, that identify the documents to remove.        
These filters use the same syntax as read operations.
```
db.products.deleteOne({name:"Pencil"})
```

### Relationships in MongoDB

There's two main ways of doing this,        
It will depends on how your data relates to each other and how it's structured.

**One to Many**      
Example: one product might have many reviews or one user might have created many comments.
```
db.products.insert(
     {
     _id:4,
     name: "Pencil",
     stock: 35,
     reviews: [
          {
          authorName: "Jinny",
          rating: 5,
          },
          {
          authorName: "Poopy",
          rating: 4,
          },  
     ]
     }
)
```
And then you could create another collection say a collection of orders this time.      
And for each document in this collection we might have a **orderNumber** and       
**productsOrdered** and this can simply be an array that references the id of the products in the
products collection:
```
{
oderNumber: 3234
productsOrdered: [1, 2]
}
```

### MongoDB with Node.js

Two options to integrate all MongoDB database with our Node.js application:
* MongoDB Native Driver
* Mongoose: ODM (Object Document Mapper) 

**Drivers**       
The MongoDB's Driver is what's going to enable our MongoDB to interact with our application.      
[MongoDB Node Driver](https://docs.mongodb.com/drivers/node/current/)     

**Creating an App:**          
Creating a Folder/Repository if neccessary.        
```
cd myNameProject
touch app.js
npm init -y
npm install mongodb --save
```
...and then open app.js with our Text Editor

**Note:** this project was made using an outdated version of MongoDB (3.5)       
In this case, I used the Documentation (3.5) to        
"Add code to connect to the server and the database myproject:"        
[MongoDb node.js driver 3.5](https://mongodb.github.io/node-mongodb-native/3.5/quick-start/quick-start/).    
I copied in app.js the coded included in the sections:     
- Connect to MongoDB       
- Insert a Document     
- Find All Documents   
Change 
```javascript
const collection = db.collection('documents');
```
to       
```javascript
const collection = db.collection("myCollection");
```
In this case, it will be called `"fruits"`         
Also in `findDocuments`, change docs to fruits.     

**Runnig the App:**   
```
node app.js
```
In older versions, the Error: "failed to cennect to server"     
will mean that we need to run the mongo server first:
```
mongod
```

**Editing our App:**  
We've got a collection called fruits inside our app.js       
And this is equivalent to when we used our a database and we used `db.fruits.insert`      
We use `insertMany` to insert our collections:
```javascript
collection.insertMany(...)
```
We change     
```javascript
client.close();
```
to      
```javascript
insertDocuments(db, function() {
    client.close();
  });
```
This way only once it's done inserting the documents        
do we close the connection to our database.

Note; in case of Error: Cannot read properties of undefined (reading 'n').
change:  
```javascript
assert.equal(3, result.result.n);
assert.equal(3, result.ops.length);
```
to
```javascript
assert.equal(3,result.insertedCount);
assert.equal(3,Object.keys(result.insertedIds).length);
```
Save and run.

**Checking Database in our Terminal (New Tab)**
```
mongo
show dbs
use FruitsDB
show collections
db.fruits.find()
```


This was an example of how to use     
**MongoDB Native Driver**        
Developers will rarely use the native MongoDB driver.       
**Mongoose**     
It vastly simplifies and cuts down on the code that's required to work with a MongoDB database.        





