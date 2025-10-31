# MongoDB CRUD Operations & Aggregation - Complete Guide

## Table of Contents
1. [Introduction to MongoDB](#introduction)
2. [CRUD Operations](#crud-operations)
3. [Aggregation Framework](#aggregation-framework)
4. [Indexes and Performance](#indexes-and-performance)
5. [Best Practices](#best-practices)

---

## Introduction to MongoDB

MongoDB is a NoSQL document database that stores data in flexible, JSON-like documents called BSON (Binary JSON). Unlike traditional relational databases, MongoDB uses collections instead of tables and documents instead of rows.

### Key Concepts
- **Database**: Container for collections
- **Collection**: Group of MongoDB documents (similar to tables)
- **Document**: A record in MongoDB, stored in BSON format
- **Field**: A key-value pair in a document (similar to columns)

---

## CRUD Operations

CRUD stands for Create, Read, Update, and Delete - the four basic operations for persistent storage.

### 1. CREATE Operations

#### Insert One Document
```javascript
db.collection.insertOne({
  name: "John Doe",
  age: 30,
  email: "john@example.com",
  address: {
    street: "123 Main St",
    city: "New York",
    zipcode: "10001"
  },
  hobbies: ["reading", "gaming", "cooking"]
})
```

**Returns:**
```javascript
{
  acknowledged: true,
  insertedId: ObjectId("507f1f77bcf86cd799439011")
}
```

#### Insert Multiple Documents
```javascript
db.collection.insertMany([
  {
    name: "Jane Smith",
    age: 25,
    email: "jane@example.com"
  },
  {
    name: "Bob Johnson",
    age: 35,
    email: "bob@example.com"
  }
])
```

**Options:**
- `ordered`: Boolean (default: true) - If false, continues inserting even if one fails
- `writeConcern`: Specifies write acknowledgment level

```javascript
db.collection.insertMany(
  [...documents],
  { ordered: false }
)
```

---

### 2. READ Operations

#### Find All Documents
```javascript
db.collection.find()
```

#### Find with Query Filter
```javascript
// Find users older than 25
db.users.find({ age: { $gt: 25 } })

// Find user by exact match
db.users.find({ name: "John Doe" })

// Find with multiple conditions (AND)
db.users.find({ 
  age: { $gte: 25 },
  city: "New York"
})

// Find with OR conditions
db.users.find({
  $or: [
    { age: { $lt: 20 } },
    { age: { $gt: 60 } }
  ]
})
```

#### Find One Document
```javascript
db.collection.findOne({ email: "john@example.com" })
```

#### Projection (Select Specific Fields)
```javascript
// Include only name and email (exclude _id)
db.users.find(
  { age: { $gt: 25 } },
  { name: 1, email: 1, _id: 0 }
)

// Exclude specific fields
db.users.find(
  {},
  { password: 0, ssn: 0 }
)
```

#### Query Operators

**Comparison Operators:**
```javascript
$eq   // Equal to
$ne   // Not equal to
$gt   // Greater than
$gte  // Greater than or equal to
$lt   // Less than
$lte  // Less than or equal to
$in   // Matches any value in array
$nin  // Matches none of values in array

// Examples
db.products.find({ price: { $gte: 100, $lte: 500 } })
db.users.find({ status: { $in: ["active", "pending"] } })
```

**Logical Operators:**
```javascript
$and  // Joins query clauses with logical AND
$or   // Joins query clauses with logical OR
$not  // Inverts effect of query expression
$nor  // Joins query clauses with logical NOR

// Examples
db.users.find({
  $and: [
    { age: { $gte: 18 } },
    { country: "USA" }
  ]
})
```

**Element Operators:**
```javascript
$exists  // Matches documents with specified field
$type    // Matches documents if field is of specified type

// Examples
db.users.find({ phone: { $exists: true } })
db.users.find({ age: { $type: "number" } })
```

**Array Operators:**
```javascript
$all       // Matches arrays containing all elements
$elemMatch // Matches documents with array element matching conditions
$size      // Matches arrays with specific size

// Examples
db.users.find({ hobbies: { $all: ["reading", "gaming"] } })
db.orders.find({ 
  items: { 
    $elemMatch: { price: { $gt: 100 }, quantity: { $gte: 2 } }
  }
})
db.users.find({ hobbies: { $size: 3 } })
```

#### Sorting, Limiting, and Skipping
```javascript
// Sort ascending (1) or descending (-1)
db.users.find().sort({ age: 1, name: -1 })

// Limit results
db.users.find().limit(10)

// Skip documents (useful for pagination)
db.users.find().skip(20).limit(10)

// Combine multiple operations
db.users.find({ age: { $gte: 18 } })
  .sort({ createdAt: -1 })
  .skip(0)
  .limit(20)
```

#### Counting Documents
```javascript
// Count all documents
db.users.countDocuments()

// Count with filter
db.users.countDocuments({ age: { $gte: 18 } })

// Estimate count (faster, less accurate)
db.users.estimatedDocumentCount()
```

---

### 3. UPDATE Operations

#### Update One Document
```javascript
db.users.updateOne(
  { email: "john@example.com" },  // Filter
  { 
    $set: { 
      age: 31,
      "address.city": "Boston"
    } 
  }  // Update
)
```

#### Update Multiple Documents
```javascript
db.users.updateMany(
  { status: "inactive" },
  { 
    $set: { status: "archived" },
    $currentDate: { lastModified: true }
  }
)
```

#### Replace One Document
```javascript
// Replaces entire document (except _id)
db.users.replaceOne(
  { email: "john@example.com" },
  {
    name: "John Doe",
    email: "john@example.com",
    age: 31,
    status: "active"
  }
)
```

#### Update Operators

**Field Update Operators:**
```javascript
$set          // Sets value of field
$unset        // Removes field from document
$rename       // Renames field
$inc          // Increments field by specified value
$mul          // Multiplies field by specified value
$min          // Updates if new value is less than current
$max          // Updates if new value is greater than current
$currentDate  // Sets field to current date

// Examples
db.users.updateOne(
  { _id: userId },
  {
    $set: { status: "active" },
    $unset: { tempField: "" },
    $inc: { loginCount: 1 },
    $currentDate: { lastLogin: true }
  }
)
```

**Array Update Operators:**
```javascript
$push      // Adds element to array
$pop       // Removes first or last element
$pull      // Removes all elements matching condition
$pullAll   // Removes all matching values
$addToSet  // Adds element only if it doesn't exist

// Examples
db.users.updateOne(
  { _id: userId },
  { $push: { hobbies: "photography" } }
)

db.users.updateOne(
  { _id: userId },
  { $push: { 
      scores: { 
        $each: [85, 90, 92],
        $sort: -1,
        $slice: 5  // Keep only top 5 scores
      }
    }
  }
)

db.users.updateOne(
  { _id: userId },
  { $pull: { hobbies: "gaming" } }
)

db.users.updateOne(
  { _id: userId },
  { $addToSet: { tags: "premium" } }
)
```

#### Upsert (Update or Insert)
```javascript
db.users.updateOne(
  { email: "newuser@example.com" },
  { 
    $set: { 
      name: "New User",
      age: 25
    } 
  },
  { upsert: true }  // Creates document if not found
)
```

#### Find and Modify
```javascript
// Returns the document (before or after update)
db.users.findOneAndUpdate(
  { email: "john@example.com" },
  { $inc: { visits: 1 } },
  { 
    returnDocument: "after",  // or "before"
    upsert: false
  }
)
```

---

### 4. DELETE Operations

#### Delete One Document
```javascript
db.users.deleteOne({ email: "john@example.com" })
```

#### Delete Multiple Documents
```javascript
db.users.deleteMany({ status: "inactive" })

// Delete all documents in collection
db.users.deleteMany({})
```

#### Find and Delete
```javascript
// Returns the deleted document
db.users.findOneAndDelete({ email: "john@example.com" })
```

---

## Aggregation Framework

The aggregation framework processes data records and returns computed results. It's similar to SQL's GROUP BY, JOIN, and complex queries.

### Basic Aggregation Pipeline

```javascript
db.collection.aggregate([
  { $match: { ... } },      // Filter documents (like WHERE)
  { $group: { ... } },      // Group documents (like GROUP BY)
  { $sort: { ... } },       // Sort results (like ORDER BY)
  { $project: { ... } },    // Select/reshape fields (like SELECT)
  { $limit: 10 }            // Limit results (like LIMIT)
])
```

### Aggregation Stages

#### $match - Filter Documents
```javascript
// Similar to find() - filters documents
db.orders.aggregate([
  { 
    $match: { 
      status: "completed",
      total: { $gte: 100 }
    } 
  }
])
```

#### $project - Reshape Documents
```javascript
db.users.aggregate([
  {
    $project: {
      name: 1,
      email: 1,
      // Create computed fields
      fullName: { $concat: ["$firstName", " ", "$lastName"] },
      isAdult: { $gte: ["$age", 18] },
      // Exclude fields
      password: 0
    }
  }
])
```

#### $group - Group and Aggregate
```javascript
// Count users by country
db.users.aggregate([
  {
    $group: {
      _id: "$country",  // Group by field
      count: { $sum: 1 },
      avgAge: { $avg: "$age" },
      maxAge: { $max: "$age" },
      minAge: { $min: "$age" }
    }
  }
])

// Total sales by product
db.orders.aggregate([
  {
    $group: {
      _id: "$productId",
      totalSales: { $sum: "$amount" },
      orderCount: { $sum: 1 },
      avgOrderValue: { $avg: "$amount" }
    }
  }
])

// Group by multiple fields
db.sales.aggregate([
  {
    $group: {
      _id: {
        year: { $year: "$date" },
        month: { $month: "$date" }
      },
      totalRevenue: { $sum: "$amount" }
    }
  }
])
```

#### Accumulator Operators in $group
```javascript
$sum      // Calculates sum
$avg      // Calculates average
$max      // Returns maximum value
$min      // Returns minimum value
$first    // Returns first value in group
$last     // Returns last value in group
$push     // Returns array of all values
$addToSet // Returns array of unique values

// Example with multiple accumulators
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      totalSpent: { $sum: "$amount" },
      orderCount: { $sum: 1 },
      avgOrder: { $avg: "$amount" },
      allProducts: { $push: "$productName" },
      uniqueCategories: { $addToSet: "$category" },
      lastOrderDate: { $max: "$orderDate" }
    }
  }
])
```

#### $sort - Sort Documents
```javascript
db.users.aggregate([
  { $match: { status: "active" } },
  { $sort: { age: -1, name: 1 } }  // -1 desc, 1 asc
])
```

#### $limit and $skip - Pagination
```javascript
db.users.aggregate([
  { $match: { status: "active" } },
  { $sort: { createdAt: -1 } },
  { $skip: 20 },
  { $limit: 10 }
])
```

#### $lookup - Join Collections
```javascript
// Left outer join
db.orders.aggregate([
  {
    $lookup: {
      from: "users",           // Collection to join
      localField: "userId",    // Field from orders
      foreignField: "_id",     // Field from users
      as: "userDetails"        // Output array field
    }
  }
])

// With pipeline (more complex joins)
db.orders.aggregate([
  {
    $lookup: {
      from: "products",
      let: { productId: "$productId" },
      pipeline: [
        { $match: { $expr: { $eq: ["$_id", "$$productId"] } } },
        { $project: { name: 1, price: 1 } }
      ],
      as: "productInfo"
    }
  }
])
```

#### $unwind - Deconstruct Arrays
```javascript
// Splits array field into separate documents
db.users.aggregate([
  { $unwind: "$hobbies" }
])

// Example: User with hobbies ["reading", "gaming"]
// Becomes: Two documents, one with "reading", one with "gaming"

// With options
db.users.aggregate([
  {
    $unwind: {
      path: "$hobbies",
      preserveNullAndEmptyArrays: true  // Keep docs with no hobbies
    }
  }
])
```

#### $addFields - Add New Fields
```javascript
db.orders.aggregate([
  {
    $addFields: {
      totalWithTax: { 
        $multiply: ["$total", 1.1] 
      },
      orderYear: { $year: "$orderDate" }
    }
  }
])
```

#### $bucket - Categorize Documents
```javascript
// Group by age ranges
db.users.aggregate([
  {
    $bucket: {
      groupBy: "$age",
      boundaries: [0, 18, 30, 50, 100],
      default: "Other",
      output: {
        count: { $sum: 1 },
        users: { $push: "$name" }
      }
    }
  }
])
```

#### $facet - Multiple Aggregations
```javascript
// Run multiple pipelines in parallel
db.products.aggregate([
  {
    $facet: {
      "categorizedByPrice": [
        {
          $bucket: {
            groupBy: "$price",
            boundaries: [0, 50, 100, 500, 1000],
            default: "expensive"
          }
        }
      ],
      "topRated": [
        { $sort: { rating: -1 } },
        { $limit: 5 }
      ],
      "statistics": [
        {
          $group: {
            _id: null,
            avgPrice: { $avg: "$price" },
            maxPrice: { $max: "$price" }
          }
        }
      ]
    }
  }
])
```

### Complex Aggregation Examples

#### Example 1: E-commerce Sales Report
```javascript
db.orders.aggregate([
  // Filter orders from last 30 days
  {
    $match: {
      orderDate: { 
        $gte: new Date(new Date().setDate(new Date().getDate() - 30))
      },
      status: "completed"
    }
  },
  // Join with products
  {
    $lookup: {
      from: "products",
      localField: "productId",
      foreignField: "_id",
      as: "product"
    }
  },
  // Unwind product array
  { $unwind: "$product" },
  // Group by category
  {
    $group: {
      _id: "$product.category",
      totalRevenue: { $sum: "$total" },
      orderCount: { $sum: 1 },
      avgOrderValue: { $avg: "$total" },
      products: { $addToSet: "$product.name" }
    }
  },
  // Sort by revenue
  { $sort: { totalRevenue: -1 } },
  // Rename _id to category
  {
    $project: {
      _id: 0,
      category: "$_id",
      totalRevenue: 1,
      orderCount: 1,
      avgOrderValue: { $round: ["$avgOrderValue", 2] },
      productCount: { $size: "$products" }
    }
  }
])
```

#### Example 2: User Engagement Analysis
```javascript
db.users.aggregate([
  // Join with activity logs
  {
    $lookup: {
      from: "activities",
      localField: "_id",
      foreignField: "userId",
      as: "activities"
    }
  },
  // Add computed fields
  {
    $addFields: {
      activityCount: { $size: "$activities" },
      lastActivity: { $max: "$activities.timestamp" },
      daysSinceJoin: {
        $divide: [
          { $subtract: [new Date(), "$createdAt"] },
          1000 * 60 * 60 * 24
        ]
      }
    }
  },
  // Calculate engagement score
  {
    $addFields: {
      engagementScore: {
        $divide: ["$activityCount", "$daysSinceJoin"]
      }
    }
  },
  // Categorize users
  {
    $bucket: {
      groupBy: "$engagementScore",
      boundaries: [0, 1, 5, 10, 100],
      default: "highly_engaged",
      output: {
        count: { $sum: 1 },
        avgActivities: { $avg: "$activityCount" },
        users: { 
          $push: { 
            name: "$name", 
            score: "$engagementScore" 
          } 
        }
      }
    }
  }
])
```

#### Example 3: Time Series Analysis
```javascript
db.sales.aggregate([
  {
    $match: {
      date: { 
        $gte: new Date("2024-01-01"),
        $lt: new Date("2025-01-01")
      }
    }
  },
  {
    $group: {
      _id: {
        year: { $year: "$date" },
        month: { $month: "$date" },
        day: { $dayOfMonth: "$date" }
      },
      dailyTotal: { $sum: "$amount" },
      transactionCount: { $sum: 1 },
      avgTransaction: { $avg: "$amount" }
    }
  },
  {
    $sort: { "_id.year": 1, "_id.month": 1, "_id.day": 1 }
  },
  {
    $project: {
      _id: 0,
      date: {
        $dateFromParts: {
          year: "$_id.year",
          month: "$_id.month",
          day: "$_id.day"
        }
      },
      dailyTotal: { $round: ["$dailyTotal", 2] },
      transactionCount: 1,
      avgTransaction: { $round: ["$avgTransaction", 2] }
    }
  }
])
```

### Aggregation Expressions

#### Arithmetic Operators
```javascript
$add         // Addition
$subtract    // Subtraction
$multiply    // Multiplication
$divide      // Division
$mod         // Modulo
$pow         // Exponentiation

// Example
{
  $project: {
    totalPrice: { 
      $multiply: ["$price", "$quantity"] 
    },
    discount: {
      $divide: [
        { $multiply: ["$price", "$discountPercent"] },
        100
      ]
    }
  }
}
```

#### String Operators
```javascript
$concat      // Concatenate strings
$substr      // Extract substring
$toLower     // Convert to lowercase
$toUpper     // Convert to uppercase
$trim        // Remove whitespace
$split       // Split string into array

// Example
{
  $project: {
    fullName: { 
      $concat: ["$firstName", " ", "$lastName"] 
    },
    emailDomain: {
      $arrayElemAt: [
        { $split: ["$email", "@"] },
        1
      ]
    }
  }
}
```

#### Date Operators
```javascript
$year        // Extract year
$month       // Extract month
$dayOfMonth  // Extract day
$hour        // Extract hour
$dateToString // Format date as string
$dateDiff    // Calculate difference between dates

// Example
{
  $project: {
    orderYear: { $year: "$orderDate" },
    formattedDate: {
      $dateToString: {
        format: "%Y-%m-%d",
        date: "$orderDate"
      }
    },
    daysSinceOrder: {
      $dateDiff: {
        startDate: "$orderDate",
        endDate: "$$NOW",
        unit: "day"
      }
    }
  }
}
```

#### Conditional Operators
```javascript
$cond        // If-then-else
$ifNull      // Returns value if field is null
$switch      // Multi-branch conditional

// Example
{
  $project: {
    status: {
      $cond: {
        if: { $gte: ["$age", 18] },
        then: "adult",
        else: "minor"
      }
    },
    displayName: { 
      $ifNull: ["$nickname", "$fullName"] 
    },
    category: {
      $switch: {
        branches: [
          { case: { $lt: ["$price", 50] }, then: "budget" },
          { case: { $lt: ["$price", 200] }, then: "mid-range" },
          { case: { $gte: ["$price", 200] }, then: "premium" }
        ],
        default: "unknown"
      }
    }
  }
}
```

---

## Indexes and Performance

### Creating Indexes

#### Single Field Index
```javascript
// Ascending index
db.users.createIndex({ email: 1 })

// Descending index
db.users.createIndex({ createdAt: -1 })
```

#### Compound Index
```javascript
// Multi-field index
db.users.createIndex({ country: 1, age: -1 })

// Order matters for compound indexes!
```

#### Unique Index
```javascript
db.users.createIndex(
  { email: 1 },
  { unique: true }
)
```

#### Text Index (for text search)
```javascript
db.articles.createIndex({ 
  title: "text", 
  content: "text" 
})

// Search using text index
db.articles.find({ 
  $text: { $search: "mongodb tutorial" } 
})
```

#### TTL Index (Time To Live)
```javascript
// Documents auto-delete after expiration
db.sessions.createIndex(
  { createdAt: 1 },
  { expireAfterSeconds: 3600 }  // 1 hour
)
```

#### Partial Index
```javascript
// Index only documents matching condition
db.orders.createIndex(
  { customerId: 1, orderDate: -1 },
  { 
    partialFilterExpression: { 
      status: "pending" 
    } 
  }
)
```

### Managing Indexes

```javascript
// List all indexes
db.collection.getIndexes()

// Drop specific index
db.collection.dropIndex("indexName")
db.collection.dropIndex({ email: 1 })

// Drop all indexes (except _id)
db.collection.dropIndexes()

// Rebuild indexes
db.collection.reIndex()
```

### Query Performance

#### Explain Query
```javascript
// See query execution plan
db.users.find({ age: { $gt: 25 } }).explain("executionStats")

// Check if index is used
db.users.find({ email: "test@example.com" }).explain()
```

#### Optimization Tips
1. Create indexes on frequently queried fields
2. Use compound indexes for multi-field queries
3. Limit number of indexes (they slow down writes)
4. Use projection to return only needed fields
5. Use covered queries (query entirely from index)
6. Avoid $where and $regex without anchors
7. Use $in instead of multiple $or conditions

---

## Best Practices

### Data Modeling

#### Embedding vs Referencing
```javascript
// Embedding (one-to-few, data read together)
{
  _id: ObjectId("..."),
  name: "John Doe",
  address: {
    street: "123 Main St",
    city: "New York"
  },
  orders: [
    { orderId: 1, amount: 100 },
    { orderId: 2, amount: 150 }
  ]
}

// Referencing (one-to-many, large subdocuments)
// Users collection
{
  _id: ObjectId("user1"),
  name: "John Doe"
}

// Orders collection
{
  _id: ObjectId("order1"),
  userId: ObjectId("user1"),
  amount: 100
}
```

### Write Concerns
```javascript
// Control acknowledgment level
db.users.insertOne(
  { name: "John" },
  { writeConcern: { w: "majority", j: true, wtimeout: 5000 } }
)

// w: "majority" - Wait for majority of replica set
// j: true - Wait for journal commit
// wtimeout - Maximum time to wait
```

### Read Concerns
```javascript
// Control read isolation level
db.users.find().readConcern("majority")

// Levels: local, available, majority, linearizable, snapshot
```

### Transactions (Replica Sets/Sharded Clusters)
```javascript
const session = db.getMongo().startSession()
session.startTransaction()

try {
  const users = session.getDatabase("mydb").users
  const accounts = session.getDatabase("mydb").accounts
  
  users.updateOne(
    { _id: userId },
    { $inc: { balance: -100 } },
    { session }
  )
  
  accounts.insertOne(
    { userId: userId, amount: -100, type: "withdrawal" },
    { session }
  )
  
  session.commitTransaction()
} catch (error) {
  session.abortTransaction()
  throw error
} finally {
  session.endSession()
}
```

### Schema Validation
```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["name", "email", "age"],
      properties: {
        name: {
          bsonType: "string",
          description: "must be a string and is required"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+$",
          description: "must be a valid email"
        },
        age: {
          bsonType: "int",
          minimum: 0,
          maximum: 150,
          description: "must be an integer between 0 and 150"
        }
      }
    }
  }
})
```

### Performance Best Practices

1. **Use appropriate data types**: Store numbers as numbers, not strings
2. **Limit document size**: Keep documents under 16MB
3. **Use projection**: Only fetch needed fields
4. **Create selective indexes**: Index high-cardinality fields
5. **Use aggregation framework**: For complex queries instead of client-side processing
6. **Batch operations**: Use insertMany, updateMany, deleteMany when possible
7. **Monitor with profiler**: 
```javascript
// Enable profiler
db.setProfilingLevel(1, { slowms: 100 })

// View slow queries
db.system.profile.find().sort({ ts: -1 }).limit(5)
```

### Connection Pooling
```javascript
// Node.js example with connection pool
const { MongoClient } = require('mongodb')

const client = new MongoClient(uri, {
  maxPoolSize: 50,
  minPoolSize: 10,
  maxIdleTimeMS: 30000
})
```

### Backup and Recovery
```bash
# Backup database
mongodump --db=mydb --out=/backup/

# Restore database
mongorestore --db=mydb /backup/mydb/

# Export to JSON
mongoexport --db=mydb --collection=users --out=users.json

# Import from JSON
mongoimport --db=mydb --collection=users --file=users.json
```

---

## Common Patterns and Examples

### Pagination Pattern
```javascript
// Page 1
db.products.find()
  .sort({ _id: 1 })
  .limit(20)

// Page 2 (cursor-based)
db.products.find({ _id: { $gt: lastIdFromPage1 } })
  .sort({ _id: 1 })
  .limit(20)
```

### Audit Trail Pattern
```javascript
// Store changes with timestamps
{
  _id: ObjectId("..."),
  documentId: ObjectId("..."),
  changes: {
    field: "status",
    oldValue: "pending",
    newValue: "approved"
  },
  changedBy: "userId",
  changedAt: ISODate("2024-01-15T10:30:00Z")
}
```

### Soft Delete Pattern
```javascript
// Add deletedAt field instead of removing
db.users.updateOne(
  { _id: userId },
  { 
    $set: { 
      deletedAt: new Date(),
      isActive: false 
    } 
  }
)

// Query only active records
db.users.find({ deletedAt: { $exists: false } })
```

### Rate Limiting Pattern
```javascript
db.rateLimits.aggregate([
  {
    $match: {
      userId: userId,
      timestamp: { $gte: new Date(Date.now() - 60000) }
    }
  },
  {
    $group: {
      _id: "$userId",
      requestCount: { $sum: 1 }
    }
  }
])
```

---

## Conclusion

This guide covers the essential MongoDB operations:
- **CRUD Operations**: Create, Read, Update, and Delete data
- **Aggregation Framework**: Complex data processing and analysis
- **Indexes**: Optimize query performance
- **Best Practices**: Schema design, performance