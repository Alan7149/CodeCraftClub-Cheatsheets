# 🍃 MongoDB Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · MongoDB (NoSQL document DB) quick reference.

---

## Shell & Databases

```bash
mongosh                          # start the shell
```

```javascript
show dbs
use shop                         // switch / create DB
db                               // current DB
show collections
db.dropDatabase()
db.users.drop()
```

## Insert

```javascript
db.users.insertOne({ name: "Alan", age: 21, tags: ["admin"] })

db.users.insertMany([
  { name: "Bob", age: 30 },
  { name: "Cara", age: 25, address: { city: "Jaipur" } }
])
```

## Find (Query)

```javascript
db.users.find()                          // all
db.users.findOne({ name: "Alan" })
db.users.find({ age: 21 })
db.users.find({ age: { $gte: 18 } })     // >= 18
db.users.find({ name: /^A/ })            // regex: starts with A
db.users.find({ tags: "admin" })         // array contains
db.users.find({ "address.city": "Jaipur" })  // nested field

// Operators: $gt $gte $lt $lte $ne $in $nin $exists $and $or
db.users.find({ $or: [{ age: 21 }, { name: "Bob" }] })
db.users.find({ age: { $in: [21, 25, 30] } })

// Projection (which fields)
db.users.find({}, { name: 1, _id: 0 })

// Sort, limit, skip
db.users.find().sort({ age: -1 }).limit(10).skip(20)
db.users.countDocuments({ age: { $gte: 18 } })
```

## Update

```javascript
db.users.updateOne({ name: "Alan" }, { $set: { age: 22 } })
db.users.updateMany({ active: false }, { $set: { active: true } })

// Operators
db.users.updateOne({ _id: id }, { $inc: { age: 1 } })         // increment
db.users.updateOne({ _id: id }, { $push: { tags: "vip" } })   // array add
db.users.updateOne({ _id: id }, { $pull: { tags: "old" } })   // array remove
db.users.updateOne({ _id: id }, { $unset: { temp: "" } })     // remove field

// Upsert: insert if not found
db.users.updateOne({ email: "x@y.com" }, { $set: { name: "X" } }, { upsert: true })
```

## Delete

```javascript
db.users.deleteOne({ name: "Alan" })
db.users.deleteMany({ age: { $lt: 18 } })
```

## Aggregation Pipeline

```javascript
db.orders.aggregate([
  { $match: { status: "paid" } },                 // filter
  { $group: {                                      // group + aggregate
      _id: "$userId",
      total: { $sum: "$amount" },
      count: { $sum: 1 }
  }},
  { $sort: { total: -1 } },                        // sort
  { $limit: 10 },
  { $project: { user: "$_id", total: 1, _id: 0 } } // reshape
])

// $lookup = join
db.orders.aggregate([
  { $lookup: {
      from: "users", localField: "userId",
      foreignField: "_id", as: "user"
  }}
])
```

## Indexes

```javascript
db.users.createIndex({ email: 1 }, { unique: true })  // 1 asc, -1 desc
db.users.createIndex({ name: "text" })                // text search
db.users.getIndexes()
db.users.find({ email: "a@b.com" }).explain("executionStats")
```

## Mongoose (Node.js ODM)

```javascript
const mongoose = require("mongoose");
await mongoose.connect("mongodb://localhost/shop");

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, unique: true },
  age: { type: Number, default: 0 },
});
const User = mongoose.model("User", userSchema);

await User.create({ name: "Alan" });
await User.find({ age: { $gte: 18 } });
await User.findById(id);
await User.findByIdAndUpdate(id, { age: 22 }, { new: true });
await User.findByIdAndDelete(id);
```

---

[🔝 Back to README](../README.md)
