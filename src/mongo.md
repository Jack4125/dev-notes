# mongo-mongoose

[Intro to Mongo](https://docs.mongodb.com/manual/core/databases-and-collections/)

[Difference between the two](https://stackoverflow.com/questions/28712248/difference-between-mongodb-and-mongoose)

Mongo and Mongoose have their own methods, that can do similar things:

mongoose's [Model.findOneAndUpdate()](https://devdocs.io/mongoose/api/model#model_Model.findOneAndUpdate) calls mongodb's [db.collection.findAndModify()](https://docs.mongodb.com/manual/reference/method/db.collection.findAndModify/) method.

## Database

`Databases` hold one or more collections.

## Collections

`Collections` store documents. `Collections` are analogous to tables in relational databases.

## Documents

[`Documents`](https://docs.mongodb.com/manual/core/document/index.html) are individual BSON (similar to JSON) objects.

As JSON, must be `key: value` pairs. But value can be anything.

`_id` comes with each document automatically.
