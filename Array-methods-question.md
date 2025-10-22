# JavaScript Array Methods Practice Guide

## 30 Scenario-Based Questions

Master JavaScript array methods through real-world scenarios covering: `map()`, `filter()`, `reduce()`, `find()`, `findIndex()`, `some()`, `every()`, `forEach()`, `sort()`, `slice()`, `splice()`, `concat()`, `includes()`, `indexOf()`, and more!

---

## 🟢 Easy Level (Questions 1-10)

### **SCENARIO 1: Price Tag Generator**
**Method Focus:** `map()`

You're building an e-commerce site and need to add a dollar sign to all prices.

```javascript
const prices = [29.99, 49.99, 19.99, 99.99];
```

**Task:** Use `map()` to add "$" before each price

**Expected Output:** `["$29.99", "$49.99", "$19.99", "$99.99"]`

---

### **SCENARIO 2: Adult Filter**
**Method Focus:** `filter()`

A website needs to filter users who are 18 or older for age-restricted content.

```javascript
const users = [
  { name: "Alice", age: 17 },
  { name: "Bob", age: 22 },
  { name: "Charlie", age: 16 },
  { name: "Diana", age: 19 }
];
```

**Task:** Use `filter()` to get only users who are 18+

**Expected Output:** `[{ name: "Bob", age: 22 }, { name: "Diana", age: 19 }]`

---

### **SCENARIO 3: Shopping Cart Total**
**Method Focus:** `reduce()`

Calculate the total price of items in a shopping cart.

```javascript
const cartItems = [
  { item: "Laptop", price: 999 },
  { item: "Mouse", price: 25 },
  { item: "Keyboard", price: 75 }
];
```

**Task:** Use `reduce()` to calculate total price

**Expected Output:** `1099`

---

### **SCENARIO 4: Finding a User by Email**
**Method Focus:** `find()`

Search for a specific user in your database by their email.

```javascript
const users = [
  { id: 1, email: "alice@example.com" },
  { id: 2, email: "bob@example.com" },
  { id: 3, email: "charlie@example.com" }
];
```

**Task:** Use `find()` to locate the user with email "bob@example.com"

**Expected Output:** `{ id: 2, email: "bob@example.com" }`

---

### **SCENARIO 5: Email Verification Check**
**Method Focus:** `some()`

Check if at least one user has verified their email.

```javascript
const users = [
  { name: "Alice", emailVerified: false },
  { name: "Bob", emailVerified: true },
  { name: "Charlie", emailVerified: false }
];
```

**Task:** Use `some()` to check if any user is verified

**Expected Output:** `true`

---

### **SCENARIO 6: Product Availability**
**Method Focus:** `every()`

Verify if all products in an order are in stock before processing.

```javascript
const orderItems = [
  { product: "Laptop", inStock: true },
  { product: "Mouse", inStock: true },
  { product: "Monitor", inStock: true }
];
```

**Task:** Use `every()` to check if all items are in stock

**Expected Output:** `true`

---

### **SCENARIO 7: Wishlist Check**
**Method Focus:** `includes()`

Check if a specific product is in the user's wishlist.

```javascript
const wishlist = ["iPhone", "iPad", "MacBook", "AirPods"];
const searchProduct = "iPad";
```

**Task:** Use `includes()` to check if the product is in wishlist

**Expected Output:** `true`

---

### **SCENARIO 8: Student Ranking**
**Method Focus:** `sort()`

Sort students by their exam scores in descending order.

```javascript
const students = [
  { name: "Alice", score: 85 },
  { name: "Bob", score: 92 },
  { name: "Charlie", score: 78 },
  { name: "Diana", score: 95 }
];
```

**Task:** Use `sort()` to arrange students by score (highest first)

**Expected Output:** `[{ name: "Diana", score: 95 }, { name: "Bob", score: 92 }, ...]`

---

### **SCENARIO 9: Uppercase Converter**
**Method Focus:** `map()`

Convert all product names to uppercase for display.

```javascript
const products = ["laptop", "mouse", "keyboard", "monitor"];
```

**Task:** Use `map()` to convert all names to uppercase

**Expected Output:** `["LAPTOP", "MOUSE", "KEYBOARD", "MONITOR"]`

---

### **SCENARIO 10: Remove Inactive Users**
**Method Focus:** `filter()`

Filter out users who haven't logged in for over 90 days.

```javascript
const users = [
  { name: "Alice", daysSinceLogin: 5 },
  { name: "Bob", daysSinceLogin: 120 },
  { name: "Charlie", daysSinceLogin: 45 },
  { name: "Diana", daysSinceLogin: 100 }
];
```

**Task:** Use `filter()` to keep only active users (logged in within 90 days)

**Expected Output:** `[{ name: "Alice", daysSinceLogin: 5 }, { name: "Charlie", daysSinceLogin: 45 }]`

---

## 🟡 Medium Level (Questions 11-20)

### **SCENARIO 11: Calculate Average Rating**
**Method Focus:** `reduce()`

Calculate the average rating of a product from user reviews.

```javascript
const reviews = [
  { user: "Alice", rating: 5 },
  { user: "Bob", rating: 4 },
  { user: "Charlie", rating: 5 },
  { user: "Diana", rating: 3 }
];
```

**Task:** Use `reduce()` to calculate average rating

**Expected Output:** `4.25`

---

### **SCENARIO 12: Find Out-of-Stock Product Index**
**Method Focus:** `findIndex()`

Find the position of the first out-of-stock product.

```javascript
const inventory = [
  { item: "Apples", stock: 50 },
  { item: "Bananas", stock: 0 },
  { item: "Oranges", stock: 30 }
];
```

**Task:** Use `findIndex()` to find the first item with stock === 0

**Expected Output:** `1`

---

### **SCENARIO 13: Tag Products on Sale**
**Method Focus:** `map()`

Add a "discounted" property to products that have a discount greater than 20%.

```javascript
const products = [
  { name: "Laptop", price: 1000, discount: 25 },
  { name: "Mouse", price: 50, discount: 10 },
  { name: "Keyboard", price: 100, discount: 30 }
];
```

**Task:** Use `map()` to add a `discounted: true/false` property

**Expected Output:** `[{ name: "Laptop", price: 1000, discount: 25, discounted: true }, ...]`

---

### **SCENARIO 14: Multi-Condition Filter**
**Method Focus:** `filter()`

Find premium products (price > 500 and rating >= 4.5).

```javascript
const products = [
  { name: "Laptop", price: 999, rating: 4.7 },
  { name: "Phone", price: 799, rating: 4.3 },
  { name: "Tablet", price: 599, rating: 4.8 },
  { name: "Smartwatch", price: 299, rating: 4.9 }
];
```

**Task:** Use `filter()` to find premium products

**Expected Output:** `[{ name: "Laptop", ... }, { name: "Tablet", ... }]`

---

### **SCENARIO 15: Concatenate Categories**
**Method Focus:** `reduce()`

Combine all product categories into a single comma-separated string.

```javascript
const products = [
  { name: "Laptop", category: "Electronics" },
  { name: "Shirt", category: "Clothing" },
  { name: "Book", category: "Books" },
  { name: "Phone", category: "Electronics" }
];
```

**Task:** Use `reduce()` to create a unique, comma-separated category list

**Expected Output:** `"Electronics, Clothing, Books"`

---

### **SCENARIO 16: Pagination - Get Page Items**
**Method Focus:** `slice()`

Implement pagination by getting items for a specific page.

```javascript
const items = ["item1", "item2", "item3", "item4", "item5", "item6", "item7", "item8"];
const itemsPerPage = 3;
const currentPage = 2; // Pages start at 1
```

**Task:** Use `slice()` to get items for page 2

**Expected Output:** `["item4", "item5", "item6"]`

---

### **SCENARIO 17: Permission Validation**
**Method Focus:** `every()`

Check if a user has all required permissions to access a resource.

```javascript
const userPermissions = ["read", "write", "delete", "admin"];
const requiredPermissions = ["read", "write"];
```

**Task:** Use `every()` to verify user has all required permissions

**Expected Output:** `true`

---

### **SCENARIO 18: Contains Spam Keywords**
**Method Focus:** `some()`

Check if a comment contains any spam keywords.

```javascript
const comment = "Buy now! Click here for amazing deals!";
const spamKeywords = ["buy now", "click here", "amazing deals", "free money"];
```

**Task:** Use `some()` to check if comment contains spam

**Expected Output:** `true`

---

### **SCENARIO 19: Sort by Multiple Criteria**
**Method Focus:** `sort()`

Sort employees by department, then by salary within each department.

```javascript
const employees = [
  { name: "Alice", dept: "Engineering", salary: 90000 },
  { name: "Bob", dept: "Marketing", salary: 70000 },
  { name: "Charlie", dept: "Engineering", salary: 95000 },
  { name: "Diana", dept: "Marketing", salary: 75000 }
];
```

**Task:** Use `sort()` to sort by department, then by salary (descending)

**Expected Output:** Engineering dept first (Charlie then Alice), then Marketing (Diana then Bob)

---

### **SCENARIO 20: Remove Item from Cart**
**Method Focus:** `splice()` or `filter()`

Remove a specific item from the shopping cart by product ID.

```javascript
const cart = [
  { id: 1, product: "Laptop" },
  { id: 2, product: "Mouse" },
  { id: 3, product: "Keyboard" }
];
const removeId = 2;
```

**Task:** Remove the item with id = 2 from the cart

**Expected Output:** `[{ id: 1, product: "Laptop" }, { id: 3, product: "Keyboard" }]`

---

## 🔴 Hard Level (Questions 21-30)

### **SCENARIO 21: Group Products by Category**
**Method Focus:** `reduce()`

Group products into an object where keys are categories.

```javascript
const products = [
  { name: "Laptop", category: "Electronics" },
  { name: "Shirt", category: "Clothing" },
  { name: "Phone", category: "Electronics" },
  { name: "Jeans", category: "Clothing" }
];
```

**Task:** Use `reduce()` to group by category

**Expected Output:** 
```javascript
{
  Electronics: [{ name: "Laptop", ... }, { name: "Phone", ... }],
  Clothing: [{ name: "Shirt", ... }, { name: "Jeans", ... }]
}
```

---

### **SCENARIO 22: Flatten Nested Orders**
**Method Focus:** `flatMap()` or `reduce()`

Flatten an array of orders to get all individual items.

```javascript
const orders = [
  { orderId: 1, items: ["Laptop", "Mouse"] },
  { orderId: 2, items: ["Keyboard"] },
  { orderId: 3, items: ["Monitor", "Cable", "Stand"] }
];
```

**Task:** Get a flat array of all items ordered

**Expected Output:** `["Laptop", "Mouse", "Keyboard", "Monitor", "Cable", "Stand"]`

---

### **SCENARIO 23: Calculate Tax Summary**
**Method Focus:** `reduce()`

Calculate total sales, total tax, and grand total from transactions.

```javascript
const transactions = [
  { amount: 100, taxRate: 0.1 },
  { amount: 200, taxRate: 0.1 },
  { amount: 150, taxRate: 0.08 }
];
```

**Task:** Use `reduce()` to calculate { totalSales, totalTax, grandTotal }

**Expected Output:** `{ totalSales: 450, totalTax: 42, grandTotal: 492 }`

---

### **SCENARIO 24: Find Missing Numbers**
**Method Focus:** `filter()` and logic

Find missing order IDs in a sequence.

```javascript
const orders = [
  { orderId: 1 },
  { orderId: 2 },
  { orderId: 4 },
  { orderId: 6 }
];
// Expected sequence: 1, 2, 3, 4, 5, 6
```

**Task:** Find missing order IDs

**Expected Output:** `[3, 5]`

---

### **SCENARIO 25: Merge User Data**
**Method Focus:** `map()` with object lookup

Merge user profiles with their latest activity from two arrays.

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" }
];

const activities = [
  { userId: 1, lastActive: "2024-01-15" },
  { userId: 2, lastActive: "2024-01-20" },
  { userId: 3, lastActive: "2024-01-10" }
];
```

**Task:** Merge data to create array with user info + lastActive

**Expected Output:** `[{ id: 1, name: "Alice", lastActive: "2024-01-15" }, ...]`

---

### **SCENARIO 26: Top N Products**
**Method Focus:** `sort()` and `slice()`

Get the top 3 best-selling products.

```javascript
const products = [
  { name: "Laptop", sales: 150 },
  { name: "Mouse", sales: 320 },
  { name: "Keyboard", sales: 280 },
  { name: "Monitor", sales: 190 },
  { name: "Webcam", sales: 410 }
];
```

**Task:** Get top 3 products by sales

**Expected Output:** `[{ name: "Webcam", sales: 410 }, { name: "Mouse", sales: 320 }, { name: "Keyboard", sales: 280 }]`

---

### **SCENARIO 27: Running Total**
**Method Focus:** `reduce()` with accumulator array

Create an array showing cumulative sales over months.

```javascript
const monthlySales = [1000, 1500, 1200, 1800, 2000];
```

**Task:** Create array of running totals

**Expected Output:** `[1000, 2500, 3700, 5500, 7500]`

---

### **SCENARIO 28: Remove Duplicates**
**Method Focus:** `filter()` with `indexOf()`

Remove duplicate emails from a mailing list.

```javascript
const emails = [
  "alice@example.com",
  "bob@example.com",
  "alice@example.com",
  "charlie@example.com",
  "bob@example.com"
];
```

**Task:** Remove duplicates while preserving order

**Expected Output:** `["alice@example.com", "bob@example.com", "charlie@example.com"]`

---

### **SCENARIO 29: Inventory Restock Alert**
**Method Focus:** Multiple methods

Find products that need restocking (stock < 10) and create restock orders.

```javascript
const inventory = [
  { id: 1, name: "Laptop", stock: 5, reorderQty: 20 },
  { id: 2, name: "Mouse", stock: 50, reorderQty: 30 },
  { id: 3, name: "Keyboard", stock: 8, reorderQty: 25 },
  { id: 4, name: "Monitor", stock: 15, reorderQty: 10 }
];
```

**Task:** Filter low stock items and map to restock orders: { id, name, orderQty }

**Expected Output:** `[{ id: 1, name: "Laptop", orderQty: 20 }, { id: 3, name: "Keyboard", orderQty: 25 }]`

---

### **SCENARIO 30: Complex Search and Filter**
**Method Focus:** Multiple methods

Search products by keyword and filter by price range, then sort by relevance.

```javascript
const products = [
  { name: "Gaming Laptop", price: 1200, description: "High-performance laptop" },
  { name: "Office Laptop", price: 800, description: "Business laptop" },
  { name: "Laptop Stand", price: 50, description: "Ergonomic stand" },
  { name: "Laptop Bag", price: 60, description: "Protective bag" }
];

const searchTerm = "laptop";
const minPrice = 100;
const maxPrice = 1000;
```

**Task:** Find products with "laptop" in name/description, price between 100-1000, sort by price

**Expected Output:** `[{ name: "Office Laptop", price: 800, ... }]`

---

## 💡 Solutions

### Solution 1: Price Tag Generator
```javascript
prices.map(price => `$${price}`);
```

### Solution 2: Adult Filter
```javascript
users.filter(user => user.age >= 18);
```

### Solution 3: Shopping Cart Total
```javascript
cartItems.reduce((total, item) => total + item.price, 0);
```

### Solution 4: Finding User by Email
```javascript
users.find(user => user.email === "bob@example.com");
```

### Solution 5: Email Verification Check
```javascript
users.some(user => user.emailVerified === true);
```

### Solution 6: Product Availability
```javascript
orderItems.every(item => item.inStock === true);
```

### Solution 7: Wishlist Check
```javascript
wishlist.includes(searchProduct);
```

### Solution 8: Student Ranking
```javascript
students.sort((a, b) => b.score - a.score);
```

### Solution 9: Uppercase Converter
```javascript
products.map(product => product.toUpperCase());
```

### Solution 10: Remove Inactive Users
```javascript
users.filter(user => user.daysSinceLogin <= 90);
```

### Solution 11: Average Rating
```javascript
const total = reviews.reduce((sum, review) => sum + review.rating, 0);
const average = total / reviews.length;
// Result: 4.25
```

### Solution 12: Out-of-Stock Index
```javascript
inventory.findIndex(item => item.stock === 0);
```

### Solution 13: Tag Products on Sale
```javascript
products.map(product => ({
  ...product,
  discounted: product.discount > 20
}));
```

### Solution 14: Multi-Condition Filter
```javascript
products.filter(p => p.price > 500 && p.rating >= 4.5);
```

### Solution 15: Concatenate Categories
```javascript
const uniqueCategories = [...new Set(products.map(p => p.category))];
uniqueCategories.join(", ");
```

### Solution 16: Pagination
```javascript
const startIndex = (currentPage - 1) * itemsPerPage;
items.slice(startIndex, startIndex + itemsPerPage);
```

### Solution 17: Permission Validation
```javascript
requiredPermissions.every(perm => userPermissions.includes(perm));
```

### Solution 18: Spam Detection
```javascript
spamKeywords.some(keyword => comment.toLowerCase().includes(keyword.toLowerCase()));
```

### Solution 19: Multi-Criteria Sort
```javascript
employees.sort((a, b) => {
  if (a.dept !== b.dept) return a.dept.localeCompare(b.dept);
  return b.salary - a.salary;
});
```

### Solution 20: Remove from Cart
```javascript
cart.filter(item => item.id !== removeId);
// Or using splice:
// const index = cart.findIndex(item => item.id === removeId);
// cart.splice(index, 1);
```

### Solution 21: Group by Category
```javascript
products.reduce((acc, product) => {
  if (!acc[product.category]) {
    acc[product.category] = [];
  }
  acc[product.category].push(product);
  return acc;
}, {});
```

### Solution 22: Flatten Orders
```javascript
orders.flatMap(order => order.items);
// Or: orders.reduce((acc, order) => acc.concat(order.items), []);
```

### Solution 23: Tax Summary
```javascript
transactions.reduce((acc, t) => {
  const tax = t.amount * t.taxRate;
  return {
    totalSales: acc.totalSales + t.amount,
    totalTax: acc.totalTax + tax,
    grandTotal: acc.grandTotal + t.amount + tax
  };
}, { totalSales: 0, totalTax: 0, grandTotal: 0 });
```

### Solution 24: Missing Numbers
```javascript
const ids = orders.map(o => o.orderId);
const max = Math.max(...ids);
const missing = [];
for (let i = 1; i <= max; i++) {
  if (!ids.includes(i)) missing.push(i);
}
// Result: missing
```

### Solution 25: Merge User Data
```javascript
users.map(user => {
  const activity = activities.find(a => a.userId === user.id);
  return { ...user, lastActive: activity.lastActive };
});
```

### Solution 26: Top N Products
```javascript
products.sort((a, b) => b.sales - a.sales).slice(0, 3);
```

### Solution 27: Running Total
```javascript
monthlySales.reduce((acc, sale, i) => {
  const runningTotal = i === 0 ? sale : acc[i - 1] + sale;
  return [...acc, runningTotal];
}, []);
```

### Solution 28: Remove Duplicates
```javascript
emails.filter((email, index) => emails.indexOf(email) === index);
// Or: [...new Set(emails)]
```

### Solution 29: Restock Alert
```javascript
inventory
  .filter(item => item.stock < 10)
  .map(item => ({
    id: item.id,
    name: item.name,
    orderQty: item.reorderQty
  }));
```

### Solution 30: Complex Search
```javascript
products
  .filter(p => 
    (p.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
     p.description.toLowerCase().includes(searchTerm.toLowerCase())) &&
    p.price >= minPrice &&
    p.price <= maxPrice
  )
  .sort((a, b) => a.price - b.price);
```

---

## 📊 Methods Quick Reference

| Method | Returns | Mutates Original? | Use Case |
|--------|---------|------------------|----------|
| `map()` | New array | No | Transform each element |
| `filter()` | New array | No | Select elements by condition |
| `reduce()` | Single value | No | Accumulate/aggregate data |
| `find()` | Single element | No | Find first match |
| `findIndex()` | Number | No | Find position of element |
| `some()` | Boolean | No | Check if any match |
| `every()` | Boolean | No | Check if all match |
| `forEach()` | undefined | No | Execute for each element |
| `sort()` | Sorted array | **YES** | Order elements |
| `splice()` | Removed items | **YES** | Add/remove elements |
| `slice()` | New array | No | Extract portion |
| `concat()` | New array | No | Join arrays |
| `includes()` | Boolean | No | Check membership |
| `indexOf()` | Number | No | Find position |
| `flatMap()` | Flattened array | No | Map + flatten |

---

## 🎯 Practice Strategy

1. **Start with Easy (1-10)** - Build confidence with single-method solutions
2. **Progress to Medium (11-20)** - Combine methods and add logic
3. **Challenge Hard (21-30)** - Real-world complex scenarios
4. **Code without looking** - Try solving before checking solutions
5. **Experiment** - Modify scenarios with your own data
6. **Performance matters** - Consider efficiency for large datasets

---

## 🚀 Pro Tips

- Chain methods for cleaner code: `arr.filter(...).map(...).sort(...)`
- Use destructuring in callbacks: `map(({ name, price }) => ...)`
- Remember: `sort()` and `splice()` mutate the original array
- Use `slice()` before `sort()` if you need the original: `[...arr].sort()`
- Modern alternative: `Set` for unique values, `Object.groupBy()` for grouping

Happy coding! 💻