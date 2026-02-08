# MongoDB Aggregation Framework - 20 Comprehensive Practice Questions

This document contains 20 MongoDB aggregation questions covering all major stages and operators from basics to advanced concepts. Each question includes detailed solutions with explanations.

---

## Question 1: $match - Basic Filtering

**Topic:** $match Stage

**Question:** Given a `products` collection, write an aggregation pipeline to find all products with a price greater than $50 and in stock.

**Sample Data:**
```javascript
db.products.insertMany([
  { name: "Laptop", price: 999, category: "Electronics", inStock: true, quantity: 15 },
  { name: "Mouse", price: 25, category: "Electronics", inStock: true, quantity: 100 },
  { name: "Desk", price: 299, category: "Furniture", inStock: false, quantity: 0 },
  { name: "Chair", price: 150, category: "Furniture", inStock: true, quantity: 30 }
]);
```

**Solution:**
```javascript
db.products.aggregate([
  {
    $match: {
      price: { $gt: 50 },
      inStock: true
    }
  }
]);
```

**Output:**
```javascript
[
  { name: "Laptop", price: 999, category: "Electronics", inStock: true, quantity: 15 },
  { name: "Chair", price: 150, category: "Furniture", inStock: true, quantity: 30 }
]
```

**Explanation:** The `$match` stage filters documents based on specified conditions. It should be used early in the pipeline for better performance as it reduces the number of documents to process.

---

## Question 2: $group - Basic Grouping and Aggregation

**Topic:** $group Stage

**Question:** Group products by category and calculate the total count, average price, and total quantity for each category.

**Solution:**
```javascript
db.products.aggregate([
  {
    $group: {
      _id: "$category",
      totalProducts: { $sum: 1 },
      averagePrice: { $avg: "$price" },
      totalQuantity: { $sum: "$quantity" },
      minPrice: { $min: "$price" },
      maxPrice: { $max: "$price" }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    _id: "Electronics",
    totalProducts: 2,
    averagePrice: 512,
    totalQuantity: 115,
    minPrice: 25,
    maxPrice: 999
  },
  {
    _id: "Furniture",
    totalProducts: 2,
    averagePrice: 224.5,
    totalQuantity: 30,
    minPrice: 150,
    maxPrice: 299
  }
]
```

**Explanation:** `$group` groups documents by a specified field (_id). Accumulator operators like `$sum`, `$avg`, `$min`, `$max` perform calculations on grouped data.

---

## Question 3: $project - Reshaping Documents

**Topic:** $project Stage

**Question:** Create a projection that shows product name, price, a discounted price (10% off), and whether it's expensive (price > $100).

**Solution:**
```javascript
db.products.aggregate([
  {
    $project: {
      _id: 0,
      name: 1,
      originalPrice: "$price",
      discountedPrice: { $multiply: ["$price", 0.9] },
      isExpensive: { $gt: ["$price", 100] },
      priceCategory: {
        $switch: {
          branches: [
            { case: { $lt: ["$price", 50] }, then: "Budget" },
            { case: { $lt: ["$price", 200] }, then: "Mid-range" },
            { case: { $gte: ["$price", 200] }, then: "Premium" }
          ],
          default: "Unknown"
        }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    name: "Laptop",
    originalPrice: 999,
    discountedPrice: 899.1,
    isExpensive: true,
    priceCategory: "Premium"
  },
  {
    name: "Mouse",
    originalPrice: 25,
    discountedPrice: 22.5,
    isExpensive: false,
    priceCategory: "Budget"
  }
  // ... more results
]
```

**Explanation:** `$project` reshapes documents by including, excluding, or adding computed fields. It can use expressions like `$multiply`, `$gt`, `$switch` for calculations and conditional logic.

---

## Question 4: $sort and $limit

**Topic:** $sort, $limit, $skip Stages

**Question:** Find the top 3 most expensive products, sorted by price in descending order.

**Solution:**
```javascript
db.products.aggregate([
  {
    $sort: { price: -1 }
  },
  {
    $limit: 3
  },
  {
    $project: {
      name: 1,
      price: 1,
      category: 1,
      _id: 0
    }
  }
]);
```

**With Pagination (Skip):**
```javascript
// Get products 4-6 (second page, 3 per page)
db.products.aggregate([
  { $sort: { price: -1 } },
  { $skip: 3 },
  { $limit: 3 }
]);
```

**Output:**
```javascript
[
  { name: "Laptop", price: 999, category: "Electronics" },
  { name: "Desk", price: 299, category: "Furniture" },
  { name: "Chair", price: 150, category: "Furniture" }
]
```

**Explanation:** `$sort` orders documents (1 for ascending, -1 for descending). `$limit` restricts the number of documents. `$skip` skips a specified number of documents, useful for pagination.

---

## Question 5: $unwind - Deconstructing Arrays

**Topic:** $unwind Stage

**Question:** Given orders with multiple items, unwind the items array to create a separate document for each item.

**Sample Data:**
```javascript
db.orders.insertMany([
  {
    orderId: 1,
    customer: "John",
    items: [
      { product: "Laptop", quantity: 1, price: 999 },
      { product: "Mouse", quantity: 2, price: 25 }
    ]
  },
  {
    orderId: 2,
    customer: "Jane",
    items: [
      { product: "Desk", quantity: 1, price: 299 }
    ]
  }
]);
```

**Solution:**
```javascript
db.orders.aggregate([
  {
    $unwind: "$items"
  },
  {
    $project: {
      _id: 0,
      orderId: 1,
      customer: 1,
      product: "$items.product",
      quantity: "$items.quantity",
      price: "$items.price",
      totalPrice: { $multiply: ["$items.quantity", "$items.price"] }
    }
  }
]);
```

**Output:**
```javascript
[
  { orderId: 1, customer: "John", product: "Laptop", quantity: 1, price: 999, totalPrice: 999 },
  { orderId: 1, customer: "John", product: "Mouse", quantity: 2, price: 25, totalPrice: 50 },
  { orderId: 2, customer: "Jane", product: "Desk", quantity: 1, price: 299, totalPrice: 299 }
]
```

**Explanation:** `$unwind` deconstructs an array field, creating a separate document for each array element. Essential for working with embedded arrays.

---

## Question 6: $lookup - Left Outer Join

**Topic:** $lookup Stage (Join)

**Question:** Join orders with customer information from a separate customers collection.

**Sample Data:**
```javascript
db.customers.insertMany([
  { _id: 1, name: "John Doe", email: "john@example.com", city: "New York" },
  { _id: 2, name: "Jane Smith", email: "jane@example.com", city: "Los Angeles" }
]);

db.orders.insertMany([
  { orderId: 101, customerId: 1, amount: 1049, date: new Date("2024-01-15") },
  { orderId: 102, customerId: 2, amount: 299, date: new Date("2024-01-16") },
  { orderId: 103, customerId: 1, amount: 50, date: new Date("2024-01-17") }
]);
```

**Solution:**
```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customerDetails"
    }
  },
  {
    $unwind: "$customerDetails"
  },
  {
    $project: {
      _id: 0,
      orderId: 1,
      amount: 1,
      customerName: "$customerDetails.name",
      customerEmail: "$customerDetails.email",
      customerCity: "$customerDetails.city"
    }
  }
]);
```

**Output:**
```javascript
[
  {
    orderId: 101,
    amount: 1049,
    customerName: "John Doe",
    customerEmail: "john@example.com",
    customerCity: "New York"
  },
  {
    orderId: 102,
    amount: 299,
    customerName: "Jane Smith",
    customerEmail: "jane@example.com",
    customerCity: "Los Angeles"
  }
  // ... more results
]
```

**Explanation:** `$lookup` performs a left outer join with another collection. The result is added as an array field, which can be unwound for easier access.

---

## Question 7: $addFields - Adding Computed Fields

**Topic:** $addFields Stage

**Question:** Add calculated fields to documents without removing existing fields.

**Solution:**
```javascript
db.orders.aggregate([
  {
    $addFields: {
      tax: { $multiply: ["$amount", 0.08] },
      totalWithTax: { $multiply: ["$amount", 1.08] },
      year: { $year: "$date" },
      month: { $month: "$date" }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    orderId: 101,
    customerId: 1,
    amount: 1049,
    date: ISODate("2024-01-15T00:00:00.000Z"),
    tax: 83.92,
    totalWithTax: 1132.92,
    year: 2024,
    month: 1
  }
  // ... more results
]
```

**Explanation:** `$addFields` adds new fields to documents while keeping all existing fields. It's similar to `$project` but doesn't require specifying all fields to keep.

---

## Question 8: $bucket - Categorizing Documents

**Topic:** $bucket Stage

**Question:** Group products into price buckets: Budget (0-100), Mid-range (100-500), Premium (500+).

**Solution:**
```javascript
db.products.aggregate([
  {
    $bucket: {
      groupBy: "$price",
      boundaries: [0, 100, 500, 1000, Infinity],
      default: "Other",
      output: {
        count: { $sum: 1 },
        products: { $push: "$name" },
        avgPrice: { $avg: "$price" },
        minPrice: { $min: "$price" },
        maxPrice: { $max: "$price" }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    _id: 0,
    count: 1,
    products: ["Mouse"],
    avgPrice: 25,
    minPrice: 25,
    maxPrice: 25
  },
  {
    _id: 100,
    count: 2,
    products: ["Desk", "Chair"],
    avgPrice: 224.5,
    minPrice: 150,
    maxPrice: 299
  },
  {
    _id: 500,
    count: 1,
    products: ["Laptop"],
    avgPrice: 999,
    minPrice: 999,
    maxPrice: 999
  }
]
```

**Explanation:** `$bucket` categorizes documents into user-defined buckets based on a field value. The `boundaries` array defines bucket ranges.

---

## Question 9: $facet - Multiple Aggregations

**Topic:** $facet Stage

**Question:** Perform multiple aggregations simultaneously: get statistics, category breakdown, and top products.

**Solution:**
```javascript
db.products.aggregate([
  {
    $facet: {
      // Statistics facet
      statistics: [
        {
          $group: {
            _id: null,
            totalProducts: { $sum: 1 },
            avgPrice: { $avg: "$price" },
            totalInventory: { $sum: "$quantity" }
          }
        }
      ],
      
      // Category breakdown facet
      byCategory: [
        {
          $group: {
            _id: "$category",
            count: { $sum: 1 },
            avgPrice: { $avg: "$price" }
          }
        },
        { $sort: { count: -1 } }
      ],
      
      // Top 3 expensive products
      topProducts: [
        { $sort: { price: -1 } },
        { $limit: 3 },
        {
          $project: {
            _id: 0,
            name: 1,
            price: 1
          }
        }
      ],
      
      // Price ranges
      priceRanges: [
        {
          $bucket: {
            groupBy: "$price",
            boundaries: [0, 100, 500, 1000],
            default: "1000+",
            output: {
              count: { $sum: 1 }
            }
          }
        }
      ]
    }
  }
]);
```

**Output:**
```javascript
[
  {
    statistics: [
      { _id: null, totalProducts: 4, avgPrice: 368.25, totalInventory: 145 }
    ],
    byCategory: [
      { _id: "Electronics", count: 2, avgPrice: 512 },
      { _id: "Furniture", count: 2, avgPrice: 224.5 }
    ],
    topProducts: [
      { name: "Laptop", price: 999 },
      { name: "Desk", price: 299 },
      { name: "Chair", price: 150 }
    ],
    priceRanges: [
      { _id: 0, count: 1 },
      { _id: 100, count: 2 },
      { _id: 500, count: 1 }
    ]
  }
]
```

**Explanation:** `$facet` allows processing multiple aggregation pipelines within a single stage, returning results in separate sub-documents.

---

## Question 10: $redact - Conditional Document Filtering

**Topic:** $redact Stage

**Question:** Implement document-level access control based on user clearance level.

**Sample Data:**
```javascript
db.documents.insertMany([
  {
    title: "Public Notice",
    content: "This is public information",
    securityLevel: 0
  },
  {
    title: "Internal Memo",
    content: "For employees only",
    securityLevel: 1
  },
  {
    title: "Confidential Report",
    content: "Highly sensitive data",
    securityLevel: 2
  }
]);
```

**Solution:**
```javascript
// User with clearance level 1
const userClearance = 1;

db.documents.aggregate([
  {
    $redact: {
      $cond: {
        if: { $lte: ["$securityLevel", userClearance] },
        then: "$$DESCEND",
        else: "$$PRUNE"
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    title: "Public Notice",
    content: "This is public information",
    securityLevel: 0
  },
  {
    title: "Internal Memo",
    content: "For employees only",
    securityLevel: 1
  }
]
```

**Explanation:** `$redact` restricts document content based on conditions. `$$DESCEND` includes the document, `$$PRUNE` excludes it, and `$$KEEP` includes without checking subdocuments.

---

## Question 11: $graphLookup - Recursive Lookup

**Topic:** $graphLookup Stage

**Question:** Find all employees in a reporting hierarchy starting from a manager.

**Sample Data:**
```javascript
db.employees.insertMany([
  { _id: 1, name: "CEO", reportsTo: null },
  { _id: 2, name: "CTO", reportsTo: 1 },
  { _id: 3, name: "CFO", reportsTo: 1 },
  { _id: 4, name: "Dev Lead", reportsTo: 2 },
  { _id: 5, name: "Dev 1", reportsTo: 4 },
  { _id: 6, name: "Dev 2", reportsTo: 4 },
  { _id: 7, name: "Accountant", reportsTo: 3 }
]);
```

**Solution:**
```javascript
db.employees.aggregate([
  {
    $match: { _id: 2 } // Start with CTO
  },
  {
    $graphLookup: {
      from: "employees",
      startWith: "$_id",
      connectFromField: "_id",
      connectToField: "reportsTo",
      as: "subordinates",
      maxDepth: 10,
      depthField: "level"
    }
  },
  {
    $project: {
      name: 1,
      subordinates: {
        $map: {
          input: "$subordinates",
          as: "sub",
          in: {
            name: "$$sub.name",
            level: "$$sub.level"
          }
        }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    _id: 2,
    name: "CTO",
    subordinates: [
      { name: "Dev Lead", level: 0 },
      { name: "Dev 1", level: 1 },
      { name: "Dev 2", level: 1 }
    ]
  }
]
```

**Explanation:** `$graphLookup` performs recursive search on a collection, following connections between documents. Useful for hierarchies, social networks, and graph-like data.

---

## Question 12: $out and $merge - Writing Results

**Topic:** $out, $merge Stages

**Question:** Create a materialized view of aggregated sales data.

**Solution:**
```javascript
// $out - Replaces entire collection
db.orders.aggregate([
  {
    $group: {
      _id: {
        customerId: "$customerId",
        year: { $year: "$date" }
      },
      totalSpent: { $sum: "$amount" },
      orderCount: { $sum: 1 }
    }
  },
  {
    $project: {
      _id: 0,
      customerId: "$_id.customerId",
      year: "$_id.year",
      totalSpent: 1,
      orderCount: 1
    }
  },
  {
    $out: "customerYearlySummary"
  }
]);

// $merge - More flexible, can update existing documents
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      totalOrders: { $sum: 1 },
      totalAmount: { $sum: "$amount" }
    }
  },
  {
    $merge: {
      into: "customerSummary",
      on: "_id",
      whenMatched: "merge",
      whenNotMatched: "insert"
    }
  }
]);
```

**Explanation:** `$out` writes aggregation results to a new collection (replacing if exists). `$merge` is more flexible, allowing updates to existing documents and insertions of new ones.

---

## Question 13: Array Operators - $push, $addToSet, $arrayElemAt

**Topic:** Array Accumulator Operators

**Question:** Group orders by customer and create arrays of order details.

**Solution:**
```javascript
db.orders.aggregate([
  {
    $group: {
      _id: "$customerId",
      // $push - adds all values (including duplicates)
      allOrderIds: { $push: "$orderId" },
      
      // $addToSet - adds unique values only
      uniqueAmounts: { $addToSet: "$amount" },
      
      // Get first and last order
      firstOrder: { $first: "$orderId" },
      lastOrder: { $last: "$orderId" },
      
      // Array of order objects
      orderDetails: {
        $push: {
          orderId: "$orderId",
          amount: "$amount",
          date: "$date"
        }
      }
    }
  },
  {
    $project: {
      customerId: "$_id",
      _id: 0,
      orderCount: { $size: "$allOrderIds" },
      allOrderIds: 1,
      uniqueAmounts: 1,
      firstOrder: 1,
      lastOrder: 1,
      // Get second order if exists
      secondOrder: {
        $arrayElemAt: ["$orderDetails", 1]
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    customerId: 1,
    orderCount: 2,
    allOrderIds: [101, 103],
    uniqueAmounts: [1049, 50],
    firstOrder: 101,
    lastOrder: 103,
    secondOrder: { orderId: 103, amount: 50, date: ISODate("...") }
  }
]
```

**Explanation:** Array operators manipulate arrays during aggregation. `$push` adds all values, `$addToSet` adds unique values, `$arrayElemAt` accesses array elements by index.

---

## Question 14: String Operators

**Topic:** String Manipulation Operators

**Question:** Clean and format customer data using string operators.

**Sample Data:**
```javascript
db.customers.insertMany([
  { firstName: "john", lastName: "DOE", email: "JOHN.DOE@EXAMPLE.COM" },
  { firstName: "jane", lastName: "smith", email: "jane@example.com" }
]);
```

**Solution:**
```javascript
db.customers.aggregate([
  {
    $project: {
      // Capitalize first name
      firstName: {
        $concat: [
          { $toUpper: { $substrCP: ["$firstName", 0, 1] } },
          { $toLower: { $substrCP: ["$firstName", 1, { $strLenCP: "$firstName" }] } }
        ]
      },
      
      // Capitalize last name
      lastName: {
        $concat: [
          { $toUpper: { $substrCP: ["$lastName", 0, 1] } },
          { $toLower: { $substrCP: ["$lastName", 1, { $strLenCP: "$lastName" }] } }
        ]
      },
      
      // Lowercase email
      email: { $toLower: "$email" },
      
      // Full name
      fullName: {
        $concat: [
          { $toUpper: { $substrCP: ["$firstName", 0, 1] } },
          { $toLower: { $substrCP: ["$firstName", 1, { $strLenCP: "$firstName" }] } },
          " ",
          { $toUpper: { $substrCP: ["$lastName", 0, 1] } },
          { $toLower: { $substrCP: ["$lastName", 1, { $strLenCP: "$lastName" }] } }
        ]
      },
      
      // Extract domain from email
      emailDomain: {
        $arrayElemAt: [
          { $split: ["$email", "@"] },
          1
        ]
      },
      
      // Check if email contains specific domain
      isExampleDomain: {
        $regexMatch: {
          input: "$email",
          regex: /@example\.com$/i
        }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    firstName: "John",
    lastName: "Doe",
    email: "john.doe@example.com",
    fullName: "John Doe",
    emailDomain: "EXAMPLE.COM",
    isExampleDomain: true
  }
]
```

**Explanation:** String operators include `$concat`, `$toUpper`, `$toLower`, `$substrCP`, `$strLenCP`, `$split`, and `$regexMatch` for comprehensive string manipulation.

---

## Question 15: Date Operators

**Topic:** Date Manipulation Operators

**Question:** Analyze orders by date components and calculate time differences.

**Solution:**
```javascript
db.orders.aggregate([
  {
    $addFields: {
      // Extract date components
      year: { $year: "$date" },
      month: { $month: "$date" },
      dayOfMonth: { $dayOfMonth: "$date" },
      dayOfWeek: { $dayOfWeek: "$date" }, // 1=Sunday, 7=Saturday
      dayName: {
        $switch: {
          branches: [
            { case: { $eq: [{ $dayOfWeek: "$date" }, 1] }, then: "Sunday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 2] }, then: "Monday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 3] }, then: "Tuesday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 4] }, then: "Wednesday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 5] }, then: "Thursday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 6] }, then: "Friday" },
            { case: { $eq: [{ $dayOfWeek: "$date" }, 7] }, then: "Saturday" }
          ]
        }
      },
      
      // Week of year
      week: { $week: "$date" },
      
      // ISO week
      isoWeek: { $isoWeek: "$date" },
      
      // Days since epoch
      daysSinceEpoch: {
        $divide: [
          { $subtract: ["$date", new Date("1970-01-01")] },
          1000 * 60 * 60 * 24
        ]
      },
      
      // Format date
      formattedDate: {
        $dateToString: {
          format: "%Y-%m-%d %H:%M:%S",
          date: "$date"
        }
      },
      
      // Days since order
      daysSinceOrder: {
        $divide: [
          { $subtract: [new Date(), "$date"] },
          1000 * 60 * 60 * 24
        ]
      }
    }
  },
  {
    $project: {
      orderId: 1,
      amount: 1,
      year: 1,
      month: 1,
      dayOfMonth: 1,
      dayName: 1,
      week: 1,
      formattedDate: 1,
      daysSinceOrder: { $floor: "$daysSinceOrder" }
    }
  }
]);
```

**Explanation:** Date operators extract components (`$year`, `$month`, `$dayOfWeek`), perform calculations (`$subtract`), and format dates (`$dateToString`).

---

## Question 16: Conditional Operators - $cond, $ifNull, $switch

**Topic:** Conditional Expressions

**Question:** Apply business logic using conditional operators.

**Solution:**
```javascript
db.products.aggregate([
  {
    $project: {
      name: 1,
      price: 1,
      quantity: 1,
      
      // Simple condition
      availability: {
        $cond: {
          if: { $gt: ["$quantity", 0] },
          then: "In Stock",
          else: "Out of Stock"
        }
      },
      
      // Ternary-style condition
      stockLevel: {
        $cond: [
          { $gte: ["$quantity", 50] },
          "High",
          { $cond: [
            { $gte: ["$quantity", 10] },
            "Medium",
            "Low"
          ]}
        ]
      },
      
      // Handle null values
      displayPrice: {
        $ifNull: ["$salePrice", "$price"]
      },
      
      // Multiple conditions with $switch
      priceCategory: {
        $switch: {
          branches: [
            { case: { $lt: ["$price", 50] }, then: "Budget" },
            { case: { $lt: ["$price", 200] }, then: "Mid-range" },
            { case: { $lt: ["$price", 500] }, then: "Premium" },
            { case: { $gte: ["$price", 500] }, then: "Luxury" }
          ],
          default: "Uncategorized"
        }
      },
      
      // Discount logic
      discount: {
        $switch: {
          branches: [
            { 
              case: { $and: [
                { $gt: ["$quantity", 100] },
                { $lt: ["$price", 50] }
              ]},
              then: 0.15
            },
            { 
              case: { $gt: ["$quantity", 50] },
              then: 0.10
            },
            { 
              case: { $gt: ["$quantity", 10] },
              then: 0.05
            }
          ],
          default: 0
        }
      }
    }
  },
  {
    $addFields: {
      finalPrice: {
        $multiply: [
          "$price",
          { $subtract: [1, "$discount"] }
        ]
      }
    }
  }
]);
```

**Explanation:** Conditional operators enable business logic: `$cond` for simple if/then/else, `$switch` for multiple conditions, `$ifNull` for default values.

---

## Question 17: $setWindowFields - Window Functions

**Topic:** Window Functions

**Question:** Calculate running totals, moving averages, and rankings.

**Sample Data:**
```javascript
db.sales.insertMany([
  { month: "2024-01", revenue: 10000, region: "North" },
  { month: "2024-02", revenue: 12000, region: "North" },
  { month: "2024-03", revenue: 15000, region: "North" },
  { month: "2024-01", revenue: 8000, region: "South" },
  { month: "2024-02", revenue: 9000, region: "South" },
  { month: "2024-03", revenue: 11000, region: "South" }
]);
```

**Solution:**
```javascript
db.sales.aggregate([
  {
    $setWindowFields: {
      partitionBy: "$region",
      sortBy: { month: 1 },
      output: {
        // Running total within each region
        runningTotal: {
          $sum: "$revenue",
          window: {
            documents: ["unbounded", "current"]
          }
        },
        
        // Moving average (current + previous 1 month)
        movingAvg: {
          $avg: "$revenue",
          window: {
            documents: [-1, 0]
          }
        },
        
        // Rank within region
        rank: {
          $rank: {}
        },
        
        // Dense rank
        denseRank: {
          $denseRank: {}
        },
        
        // Previous month revenue
        previousRevenue: {
          $shift: {
            output: "$revenue",
            by: -1,
            default: 0
          }
        },
        
        // Next month revenue
        nextRevenue: {
          $shift: {
            output: "$revenue",
            by: 1,
            default: 0
          }
        },
        
        // Growth rate
        growthRate: {
          $let: {
            vars: {
              prev: {
                $shift: {
                  output: "$revenue",
                  by: -1,
                  default: "$revenue"
                }
              }
            },
            in: {
              $multiply: [
                {
                  $divide: [
                    { $subtract: ["$revenue", "$$prev"] },
                    "$$prev"
                  ]
                },
                100
              ]
            }
          }
        }
      }
    }
  },
  {
    $project: {
      _id: 0,
      month: 1,
      region: 1,
      revenue: 1,
      runningTotal: 1,
      movingAvg: { $round: ["$movingAvg", 2] },
      rank: 1,
      previousRevenue: 1,
      growthRate: { $round: ["$growthRate", 2] }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    month: "2024-01",
    region: "North",
    revenue: 10000,
    runningTotal: 10000,
    movingAvg: 10000,
    rank: 1,
    previousRevenue: 0,
    growthRate: 0
  },
  {
    month: "2024-02",
    region: "North",
    revenue: 12000,
    runningTotal: 22000,
    movingAvg: 11000,
    rank: 2,
    previousRevenue: 10000,
    growthRate: 20
  }
  // ... more results
]
```

**Explanation:** `$setWindowFields` performs calculations across a set of documents (window), supporting running totals, moving averages, rankings, and accessing adjacent documents.

---

## Question 18: Text Search and $text

**Topic:** Text Search in Aggregation

**Question:** Perform full-text search on product descriptions.

**Setup:**
```javascript
// Create text index
db.products.createIndex({ name: "text", description: "text" });

db.products.insertMany([
  { name: "Laptop Pro", description: "High-performance laptop with SSD and 16GB RAM", price: 999 },
  { name: "Gaming Mouse", description: "RGB gaming mouse with high DPI sensor", price: 49 },
  { name: "Mechanical Keyboard", description: "RGB mechanical keyboard for gaming", price: 89 }
]);
```

**Solution:**
```javascript
db.products.aggregate([
  {
    $match: {
      $text: { $search: "gaming RGB" }
    }
  },
  {
    $addFields: {
      // Text search score
      score: { $meta: "textScore" }
    }
  },
  {
    $sort: { score: -1 }
  },
  {
    $project: {
      name: 1,
      description: 1,
      price: 1,
      score: 1,
      relevance: {
        $switch: {
          branches: [
            { case: { $gte: ["$score", 2] }, then: "High" },
            { case: { $gte: ["$score", 1] }, then: "Medium" }
          ],
          default: "Low"
        }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    name: "Gaming Mouse",
    description: "RGB gaming mouse with high DPI sensor",
    price: 49,
    score: 2.25,
    relevance: "High"
  },
  {
    name: "Mechanical Keyboard",
    description: "RGB mechanical keyboard for gaming",
    price: 89,
    score: 2.25,
    relevance: "High"
  }
]
```

**Explanation:** `$text` performs full-text search using text indexes. `$meta: "textScore"` retrieves relevance scores for sorting and filtering.

---

## Question 19: Complex Pipeline - E-commerce Analytics

**Topic:** Multi-Stage Complex Pipeline

**Question:** Create a comprehensive sales analytics report combining multiple aggregation techniques.

**Sample Data:**
```javascript
db.orders.insertMany([
  {
    orderId: 1,
    customerId: 101,
    products: [
      { productId: "P1", name: "Laptop", quantity: 1, price: 999 },
      { productId: "P2", name: "Mouse", quantity: 2, price: 25 }
    ],
    status: "completed",
    orderDate: new Date("2024-01-15"),
    shippingAddress: { city: "New York", state: "NY", country: "USA" }
  },
  {
    orderId: 2,
    customerId: 102,
    products: [
      { productId: "P3", name: "Keyboard", quantity: 1, price: 89 }
    ],
    status: "completed",
    orderDate: new Date("2024-01-20"),
    shippingAddress: { city: "Los Angeles", state: "CA", country: "USA" }
  },
  {
    orderId: 3,
    customerId: 101,
    products: [
      { productId: "P1", name: "Laptop", quantity: 1, price: 999 }
    ],
    status: "pending",
    orderDate: new Date("2024-02-01"),
    shippingAddress: { city: "New York", state: "NY", country: "USA" }
  }
]);
```

**Solution:**
```javascript
db.orders.aggregate([
  // Stage 1: Filter completed orders
  {
    $match: {
      status: "completed",
      orderDate: {
        $gte: new Date("2024-01-01"),
        $lt: new Date("2024-03-01")
      }
    }
  },
  
  // Stage 2: Unwind products
  {
    $unwind: "$products"
  },
  
  // Stage 3: Calculate product revenue
  {
    $addFields: {
      "products.revenue": {
        $multiply: ["$products.quantity", "$products.price"]
      },
      month: { $month: "$orderDate" },
      year: { $year: "$orderDate" }
    }
  },
  
  // Stage 4: Group by product and calculate metrics
  {
    $group: {
      _id: {
        productId: "$products.productId",
        productName: "$products.name"
      },
      totalQuantitySold: { $sum: "$products.quantity" },
      totalRevenue: { $sum: "$products.revenue" },
      averagePrice: { $avg: "$products.price" },
      orderCount: { $sum: 1 },
      uniqueCustomers: { $addToSet: "$customerId" },
      states: { $addToSet: "$shippingAddress.state" }
    }
  },
  
  // Stage 5: Add calculated fields
  {
    $addFields: {
      uniqueCustomerCount: { $size: "$uniqueCustomers" },
      stateCount: { $size: "$states" },
      avgRevenuePerOrder: {
        $divide: ["$totalRevenue", "$orderCount"]
      }
    }
  },
  
  // Stage 6: Sort by revenue
  {
    $sort: { totalRevenue: -1 }
  },
  
  // Stage 7: Project final shape
  {
    $project: {
      _id: 0,
      productId: "$_id.productId",
      productName: "$_id.productName",
      totalQuantitySold: 1,
      totalRevenue: { $round: ["$totalRevenue", 2] },
      averagePrice: { $round: ["$averagePrice", 2] },
      orderCount: 1,
      uniqueCustomerCount: 1,
      stateCount: 1,
      avgRevenuePerOrder: { $round: ["$avgRevenuePerOrder", 2] },
      performance: {
        $switch: {
          branches: [
            { case: { $gte: ["$totalRevenue", 1000] }, then: "Excellent" },
            { case: { $gte: ["$totalRevenue", 500] }, then: "Good" },
            { case: { $gte: ["$totalRevenue", 100] }, then: "Average" }
          ],
          default: "Poor"
        }
      }
    }
  },
  
  // Stage 8: Add ranking
  {
    $setWindowFields: {
      sortBy: { totalRevenue: -1 },
      output: {
        revenueRank: { $rank: {} }
      }
    }
  }
]);
```

**Output:**
```javascript
[
  {
    productId: "P1",
    productName: "Laptop",
    totalQuantitySold: 1,
    totalRevenue: 999,
    averagePrice: 999,
    orderCount: 1,
    uniqueCustomerCount: 1,
    stateCount: 1,
    avgRevenuePerOrder: 999,
    performance: "Good",
    revenueRank: 1
  }
  // ... more results
]
```

**Explanation:** This complex pipeline demonstrates combining multiple stages: filtering, unwinding, grouping, calculations, sorting, projecting, and window functions for comprehensive analytics.

---

## Question 20: Performance Optimization Techniques

**Topic:** Aggregation Pipeline Optimization

**Question:** Demonstrate best practices for optimizing aggregation pipeline performance.

**Solution:**
```javascript
// BAD: Inefficient pipeline
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",
      localField: "customerId",
      foreignField: "_id",
      as: "customer"
    }
  },
  { $unwind: "$customer" },
  {
    $match: {
      "customer.country": "USA",
      orderDate: { $gte: new Date("2024-01-01") }
    }
  },
  {
    $project: {
      orderId: 1,
      amount: 1,
      customerName: "$customer.name"
    }
  }
]);

// GOOD: Optimized pipeline
db.orders.aggregate([
  // 1. Match early to reduce documents
  {
    $match: {
      orderDate: { $gte: new Date("2024-01-01") }
    }
  },
  
  // 2. Project early to reduce data size
  {
    $project: {
      orderId: 1,
      customerId: 1,
      amount: 1
    }
  },
  
  // 3. Lookup after reducing documents
  {
    $lookup: {
      from: "customers",
      let: { custId: "$customerId" },
      pipeline: [
        {
          $match: {
            $expr: { $eq: ["$_id", "$$custId"] },
            country: "USA" // Filter in lookup pipeline
          }
        },
        {
          $project: { name: 1 } // Project only needed fields
        }
      ],
      as: "customer"
    }
  },
  
  // 4. Filter out non-matching customers
  {
    $match: {
      customer: { $ne: [] }
    }
  },
  
  { $unwind: "$customer" },
  
  {
    $project: {
      orderId: 1,
      amount: 1,
      customerName: "$customer.name"
    }
  }
]);

// Additional optimization tips:

// Use indexes
db.orders.createIndex({ orderDate: 1 });
db.orders.createIndex({ customerId: 1 });
db.customers.createIndex({ country: 1 });

// Use $limit early when possible
db.orders.aggregate([
  { $match: { status: "completed" } },
  { $sort: { orderDate: -1 } },
  { $limit: 100 }, // Limit early
  // ... rest of pipeline
]);

// Use allowDiskUse for large datasets
db.orders.aggregate(
  [/* pipeline stages */],
  { allowDiskUse: true }
);

// Use $sample for random sampling instead of $match + $limit
db.orders.aggregate([
  { $sample: { size: 1000 } } // Random 1000 documents
]);

// Avoid unnecessary $unwind and $group
// Instead of:
db.orders.aggregate([
  { $unwind: "$items" },
  { $group: { _id: "$_id", total: { $sum: "$items.price" } } }
]);

// Use $reduce:
db.orders.aggregate([
  {
    $addFields: {
      total: {
        $reduce: {
          input: "$items",
          initialValue: 0,
          in: { $add: ["$$value", "$$this.price"] }
        }
      }
    }
  }
]);
```

**Optimization Best Practices:**

1. **Use $match early** - Filter documents as early as possible
2. **Use $project early** - Reduce document size before expensive operations
3. **Create appropriate indexes** - Especially for $match and $sort stages
4. **Optimize $lookup** - Use pipeline syntax to filter on both sides
5. **Avoid unnecessary $unwind** - Use array operators when possible
6. **Use $limit** - Especially after $sort
7. **Use allowDiskUse** - For operations exceeding memory limits
8. **Use explain()** - Analyze pipeline performance
9. **Use $sample** - For random sampling instead of full collection scans
10. **Order stages efficiently** - Match → Sort → Limit → Lookup → Unwind

**Check Pipeline Performance:**
```javascript
db.orders.explain("executionStats").aggregate([
  // ... pipeline stages
]);
```

**Explanation:** Optimizing aggregation pipelines involves strategic ordering of stages, early filtering and projection, proper indexing, and using appropriate operators for the task.

---

## Summary

These 20 questions cover all major MongoDB aggregation topics:

1. **$match** - Basic filtering
2. **$group** - Grouping and aggregation
3. **$project** - Reshaping documents
4. **$sort, $limit, $skip** - Sorting and pagination
5. **$unwind** - Array deconstruction
6. **$lookup** - Joins
7. **$addFields** - Adding computed fields
8. **$bucket** - Categorization
9. **$facet** - Multiple aggregations
10. **$redact** - Document-level filtering
11. **$graphLookup** - Recursive lookups
12. **$out, $merge** - Writing results
13. **Array Operators** - Array manipulation
14. **String Operators** - String manipulation
15. **Date Operators** - Date operations
16. **Conditional Operators** - Business logic
17. **$setWindowFields** - Window functions
18. **Text Search** - Full-text search
19. **Complex Pipeline** - Real-world example
20. **Optimization** - Performance best practices

Master these concepts to become proficient in MongoDB aggregation framework!