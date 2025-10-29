# JavaScript Coding Questions: Real-World Scenarios with Solutions

## Table of Contents
1. [Set Questions](#set-questions)
2. [HashMap Questions](#hashmap-questions)
3. [Sorting Questions](#sorting-questions)
4. [Searching Questions](#searching-questions)

---

## Set Questions

### 1. Remove Duplicate Elements from Shopping Cart
**Scenario:** An e-commerce platform needs to remove duplicate product IDs from a user's shopping cart.

```javascript
function removeDuplicates(productIds) {
  return { store: nearest, distance: minDistance };
}

// Example
const userLoc = { lat: 40.7128, lng: -74.0060 }; // New York
const stores = [
  { name: 'Store A', location: { lat: 40.7580, lng: -73.9855 } },
  { name: 'Store B', location: { lat: 40.6782, lng: -73.9442 } },
  { name: 'Store C', location: { lat: 40.7489, lng: -73.9680 } }
];
console.log(findNearestLocation(userLoc, stores));
```

### 35. Search in Rotated Sorted Array
**Scenario:** Search for a product ID in a catalog that has been rotated during a system migration.

```javascript
function searchRotatedArray(arr, target) {
  let left = 0;
  let right = arr.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] === target) {
      return mid;
    }
    
    // Determine which half is sorted
    if (arr[left] <= arr[mid]) {
      // Left half is sorted
      if (target >= arr[left] && target < arr[mid]) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    } else {
      // Right half is sorted
      if (target > arr[mid] && target <= arr[right]) {
        left = mid + 1;
      } else {
        right = mid - 1;
      }
    }
  }
  
  return -1;
}

// Example
const rotatedArray = [45, 67, 89, 12, 23, 34];
console.log(searchRotatedArray(rotatedArray, 23)); // 4
```

### 36. Find All Matching Records
**Scenario:** Search database records that match partial text across multiple fields.

```javascript
function findMatchingRecords(records, searchTerm) {
  const lowerSearch = searchTerm.toLowerCase();
  
  return records.filter(record => {
    return Object.values(record).some(value => {
      if (typeof value === 'string') {
        return value.toLowerCase().includes(lowerSearch);
      }
      if (typeof value === 'number') {
        return value.toString().includes(searchTerm);
      }
      return false;
    });
  });
}

// Example
const employees = [
  { id: 1, name: 'John Doe', department: 'Engineering', email: 'john@example.com' },
  { id: 2, name: 'Jane Smith', department: 'Marketing', email: 'jane@example.com' },
  { id: 3, name: 'Bob Johnson', department: 'Engineering', email: 'bob@example.com' }
];
console.log(findMatchingRecords(employees, 'engineering'));
// Returns John Doe and Bob Johnson
```

### 37. Search with Ranking/Scoring
**Scenario:** Search and rank results based on relevance score.

```javascript
function searchWithRanking(items, query) {
  const lowerQuery = query.toLowerCase();
  
  const scored = items.map(item => {
    let score = 0;
    const lowerItem = item.toLowerCase();
    
    // Exact match
    if (lowerItem === lowerQuery) {
      score += 100;
    }
    // Starts with query
    else if (lowerItem.startsWith(lowerQuery)) {
      score += 50;
    }
    // Contains query
    else if (lowerItem.includes(lowerQuery)) {
      score += 25;
    }
    
    // Bonus for shorter items (more specific)
    score += (100 - item.length) * 0.1;
    
    return { item, score };
  }).filter(result => result.score > 0);
  
  return scored
    .sort((a, b) => b.score - a.score)
    .map(result => result.item);
}

// Example
const products = ['iPhone 14', 'iPhone 14 Pro', 'iPad', 'iPhone 13', 'iPhone'];
console.log(searchWithRanking(products, 'iphone'));
// ['iPhone', 'iPhone 14', 'iPhone 13', 'iPhone 14 Pro']
```

### 38. Deep Search in Nested Objects
**Scenario:** Search for a value in nested configuration objects.

```javascript
function deepSearch(obj, searchValue, path = '') {
  const results = [];
  
  function search(current, currentPath) {
    if (typeof current === 'object' && current !== null) {
      for (const [key, value] of Object.entries(current)) {
        const newPath = currentPath ? `${currentPath}.${key}` : key;
        
        if (value === searchValue) {
          results.push({ path: newPath, value });
        }
        
        if (typeof value === 'object') {
          search(value, newPath);
        }
      }
    }
  }
  
  search(obj, path);
  return results;
}

// Example
const config = {
  server: {
    host: 'localhost',
    port: 3000,
    database: {
      host: 'localhost',
      port: 5432
    }
  },
  cache: {
    host: 'localhost',
    port: 6379
  }
};
console.log(deepSearch(config, 'localhost'));
// [{ path: 'server.host', value: 'localhost' }, ...]
```

### 39. Find Peak Element
**Scenario:** Find a trending product (peak in sales data) in an e-commerce analytics dashboard.

```javascript
function findPeakElement(arr) {
  if (arr.length === 0) return -1;
  if (arr.length === 1) return 0;
  
  let left = 0;
  let right = arr.length - 1;
  
  while (left < right) {
    const mid = Math.floor((left + right) / 2);
    
    if (arr[mid] < arr[mid + 1]) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }
  
  return left;
}

// Example
const salesData = [10, 20, 30, 40, 35, 25, 15];
const peakIndex = findPeakElement(salesData);
console.log(`Peak sales at index ${peakIndex}: ${salesData[peakIndex]}`);
// Peak sales at index 3: 40
```

### 40. Search with Pagination
**Scenario:** Implement paginated search results for a large product catalog.

```javascript
function paginatedSearch(items, query, page = 1, pageSize = 10) {
  const lowerQuery = query.toLowerCase();
  
  // Filter matching items
  const filtered = items.filter(item => 
    item.name.toLowerCase().includes(lowerQuery) ||
    item.description.toLowerCase().includes(lowerQuery)
  );
  
  // Calculate pagination
  const totalItems = filtered.length;
  const totalPages = Math.ceil(totalItems / pageSize);
  const startIndex = (page - 1) * pageSize;
  const endIndex = startIndex + pageSize;
  
  // Get page items
  const pageItems = filtered.slice(startIndex, endIndex);
  
  return {
    items: pageItems,
    pagination: {
      currentPage: page,
      pageSize,
      totalItems,
      totalPages,
      hasNextPage: page < totalPages,
      hasPrevPage: page > 1
    }
  };
}

// Example
const catalog = [
  { id: 1, name: 'Laptop Pro', description: 'High-performance laptop' },
  { id: 2, name: 'Laptop Air', description: 'Lightweight laptop' },
  { id: 3, name: 'Desktop PC', description: 'Powerful desktop computer' },
  { id: 4, name: 'Laptop Gaming', description: 'Gaming laptop with RTX' },
  { id: 5, name: 'Tablet', description: 'Portable tablet device' }
];
console.log(paginatedSearch(catalog, 'laptop', 1, 2));
// Returns first 2 laptop results with pagination info
```

---

## Practice Tips

### For Set Questions:
- Use Sets for **uniqueness** and **fast lookups** (O(1))
- Remember Set methods: `add()`, `has()`, `delete()`, `size`
- Convert between Sets and Arrays: `[...set]` and `new Set(array)`

### For HashMap Questions:
- Use Map for **key-value pairs** with any key type
- Remember Map methods: `set()`, `get()`, `has()`, `delete()`, `size`
- Use `Object.fromEntries()` to convert Map to plain object
- Maps maintain insertion order

### For Sorting Questions:
- Default `sort()` converts to strings - use compare function for numbers
- Return negative for ascending, positive for descending
- Consider **stable sort** when order matters for equal elements
- Time complexity: O(n log n) for most sorting algorithms

### For Searching Questions:
- Linear search: O(n) - checks every element
- Binary search: O(log n) - requires sorted array
- Consider using **hash maps** for O(1) lookups
- Fuzzy search improves user experience for autocomplete

---

## Additional Resources

- [MDN Web Docs - Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
- [MDN Web Docs - Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
- [MDN Web Docs - Array.sort()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
- [Big O Cheat Sheet](https://www.bigocheatsheet.com/)

---

**Created: October 2025**  
**Total Questions: 40** (10 per category) [...new Set(productIds)];
}

// Example
const cart = [101, 205, 101, 340, 205, 450];
console.log(removeDuplicates(cart)); // [101, 205, 340, 450]
```

### 2. Find Common Interests Between Users
**Scenario:** A social media app needs to find common interests between two users for friend suggestions.

```javascript
function findCommonInterests(user1Interests, user2Interests) {
  const set1 = new Set(user1Interests);
  const set2 = new Set(user2Interests);
  return [...set1].filter(interest => set2.has(interest));
}

// Example
const alice = ['coding', 'music', 'travel', 'photography'];
const bob = ['music', 'gaming', 'photography', 'cooking'];
console.log(findCommonInterests(alice, bob)); // ['music', 'photography']
```

### 3. Check if All Required Skills are Present
**Scenario:** A job portal needs to verify if a candidate has all required skills for a position.

```javascript
function hasAllRequiredSkills(candidateSkills, requiredSkills) {
  const skillSet = new Set(candidateSkills);
  return requiredSkills.every(skill => skillSet.has(skill));
}

// Example
const candidate = ['JavaScript', 'React', 'Node.js', 'MongoDB', 'CSS'];
const required = ['JavaScript', 'React', 'Node.js'];
console.log(hasAllRequiredSkills(candidate, required)); // true
```

### 4. Find Unique Visitors to Website
**Scenario:** Analytics platform needs to count unique visitors from an array of IP addresses.

```javascript
function countUniqueVisitors(ipAddresses) {
  return new Set(ipAddresses).size;
}

// Example
const visits = ['192.168.1.1', '10.0.0.1', '192.168.1.1', '172.16.0.1', '10.0.0.1'];
console.log(countUniqueVisitors(visits)); // 3
```

### 5. Find Exclusive Features Between Products
**Scenario:** Compare two product versions and find features exclusive to each version.

```javascript
function findExclusiveFeatures(product1, product2) {
  const set1 = new Set(product1);
  const set2 = new Set(product2);
  
  const exclusiveToProduct1 = [...set1].filter(f => !set2.has(f));
  const exclusiveToProduct2 = [...set2].filter(f => !set1.has(f));
  
  return { product1: exclusiveToProduct1, product2: exclusiveToProduct2 };
}

// Example
const basic = ['wifi', 'bluetooth', '1080p'];
const premium = ['wifi', 'bluetooth', '4k', '5G'];
console.log(findExclusiveFeatures(basic, premium));
// { product1: ['1080p'], product2: ['4k', '5G'] }
```

### 6. Detect Duplicate Email Submissions
**Scenario:** A newsletter signup form needs to detect if an email has already been submitted.

```javascript
class NewsletterManager {
  constructor() {
    this.subscribers = new Set();
  }
  
  subscribe(email) {
    if (this.subscribers.has(email)) {
      return { success: false, message: 'Email already subscribed' };
    }
    this.subscribers.add(email);
    return { success: true, message: 'Successfully subscribed' };
  }
  
  getSubscriberCount() {
    return this.subscribers.size;
  }
}

// Example
const newsletter = new NewsletterManager();
console.log(newsletter.subscribe('user@example.com')); // { success: true, ... }
console.log(newsletter.subscribe('user@example.com')); // { success: false, ... }
```

### 7. Find All Unique Tags in Blog Posts
**Scenario:** Extract all unique tags from multiple blog posts.

```javascript
function getAllUniqueTags(blogPosts) {
  const allTags = blogPosts.flatMap(post => post.tags);
  return [...new Set(allTags)];
}

// Example
const posts = [
  { title: 'Post 1', tags: ['javascript', 'web', 'frontend'] },
  { title: 'Post 2', tags: ['python', 'backend', 'web'] },
  { title: 'Post 3', tags: ['javascript', 'react', 'frontend'] }
];
console.log(getAllUniqueTags(posts));
// ['javascript', 'web', 'frontend', 'python', 'backend', 'react']
```

### 8. Validate Unique Usernames
**Scenario:** Check if a username is unique before registration.

```javascript
class UserRegistry {
  constructor() {
    this.usernames = new Set(['admin', 'moderator', 'guest']);
  }
  
  isUsernameAvailable(username) {
    return !this.usernames.has(username.toLowerCase());
  }
  
  registerUsername(username) {
    const lowerUsername = username.toLowerCase();
    if (!this.isUsernameAvailable(lowerUsername)) {
      return false;
    }
    this.usernames.add(lowerUsername);
    return true;
  }
}

// Example
const registry = new UserRegistry();
console.log(registry.registerUsername('JohnDoe')); // true
console.log(registry.registerUsername('Admin')); // false
```

### 9. Track Active User Sessions
**Scenario:** Monitor currently active user sessions and prevent duplicate logins.

```javascript
class SessionManager {
  constructor() {
    this.activeSessions = new Set();
  }
  
  login(userId) {
    if (this.activeSessions.has(userId)) {
      return { success: false, message: 'User already logged in' };
    }
    this.activeSessions.add(userId);
    return { success: true, message: 'Login successful' };
  }
  
  logout(userId) {
    return this.activeSessions.delete(userId);
  }
  
  getActiveUserCount() {
    return this.activeSessions.size;
  }
}

// Example
const sessions = new SessionManager();
sessions.login('user123');
console.log(sessions.getActiveUserCount()); // 1
```

### 10. Find Products Available in All Stores
**Scenario:** E-commerce chain needs to find products available in all their stores.

```javascript
function findProductsInAllStores(storeInventories) {
  if (storeInventories.length === 0) return [];
  
  const firstStore = new Set(storeInventories[0]);
  
  return [...firstStore].filter(product => 
    storeInventories.every(inventory => inventory.includes(product))
  );
}

// Example
const inventories = [
  ['laptop', 'mouse', 'keyboard', 'monitor'],
  ['laptop', 'keyboard', 'headphones', 'monitor'],
  ['laptop', 'monitor', 'keyboard', 'webcam']
];
console.log(findProductsInAllStores(inventories));
// ['laptop', 'keyboard', 'monitor']
```

---

## HashMap Questions

### 11. Count Word Frequency in Document
**Scenario:** Analyze a document and count how many times each word appears.

```javascript
function countWordFrequency(text) {
  const words = text.toLowerCase().match(/\b\w+\b/g) || [];
  const frequency = new Map();
  
  for (const word of words) {
    frequency.set(word, (frequency.get(word) || 0) + 1);
  }
  
  return Object.fromEntries(frequency);
}

// Example
const text = "the quick brown fox jumps over the lazy dog the fox";
console.log(countWordFrequency(text));
// { the: 3, quick: 1, brown: 1, fox: 2, jumps: 1, over: 1, lazy: 1, dog: 1 }
```

### 12. Group Students by Grade
**Scenario:** A school management system needs to group students by their grades.

```javascript
function groupStudentsByGrade(students) {
  const gradeMap = new Map();
  
  for (const student of students) {
    if (!gradeMap.has(student.grade)) {
      gradeMap.set(student.grade, []);
    }
    gradeMap.get(student.grade).push(student.name);
  }
  
  return Object.fromEntries(gradeMap);
}

// Example
const students = [
  { name: 'Alice', grade: 'A' },
  { name: 'Bob', grade: 'B' },
  { name: 'Charlie', grade: 'A' },
  { name: 'David', grade: 'C' },
  { name: 'Eve', grade: 'B' }
];
console.log(groupStudentsByGrade(students));
// { A: ['Alice', 'Charlie'], B: ['Bob', 'Eve'], C: ['David'] }
```

### 13. Cache API Responses
**Scenario:** Implement a simple cache for API responses to reduce server load.

```javascript
class APICache {
  constructor(ttl = 60000) { // default 1 minute TTL
    this.cache = new Map();
    this.ttl = ttl;
  }
  
  set(key, value) {
    const expiry = Date.now() + this.ttl;
    this.cache.set(key, { value, expiry });
  }
  
  get(key) {
    const cached = this.cache.get(key);
    if (!cached) return null;
    
    if (Date.now() > cached.expiry) {
      this.cache.delete(key);
      return null;
    }
    
    return cached.value;
  }
  
  clear() {
    this.cache.clear();
  }
}

// Example
const cache = new APICache(5000); // 5 seconds TTL
cache.set('/api/users', [{ id: 1, name: 'John' }]);
console.log(cache.get('/api/users')); // Returns cached data
```

### 14. Track Product Inventory
**Scenario:** Manage product inventory with real-time stock updates.

```javascript
class InventoryManager {
  constructor() {
    this.inventory = new Map();
  }
  
  addProduct(productId, quantity) {
    const current = this.inventory.get(productId) || 0;
    this.inventory.set(productId, current + quantity);
  }
  
  removeProduct(productId, quantity) {
    const current = this.inventory.get(productId) || 0;
    if (current < quantity) {
      return { success: false, message: 'Insufficient stock' };
    }
    this.inventory.set(productId, current - quantity);
    return { success: true, available: current - quantity };
  }
  
  getStock(productId) {
    return this.inventory.get(productId) || 0;
  }
}

// Example
const inventory = new InventoryManager();
inventory.addProduct('PROD-001', 100);
console.log(inventory.removeProduct('PROD-001', 25));
// { success: true, available: 75 }
```

### 15. Find First Non-Repeating Character
**Scenario:** In a streaming service, find the first unique character in a movie title.

```javascript
function firstNonRepeatingChar(str) {
  const charCount = new Map();
  
  for (const char of str) {
    charCount.set(char, (charCount.get(char) || 0) + 1);
  }
  
  for (const char of str) {
    if (charCount.get(char) === 1) {
      return char;
    }
  }
  
  return null;
}

// Example
console.log(firstNonRepeatingChar('aabbcdeff')); // 'c'
console.log(firstNonRepeatingChar('aabbcc')); // null
```

### 16. Group Orders by Customer
**Scenario:** An e-commerce platform needs to group all orders by customer ID.

```javascript
function groupOrdersByCustomer(orders) {
  const customerOrders = new Map();
  
  for (const order of orders) {
    if (!customerOrders.has(order.customerId)) {
      customerOrders.set(order.customerId, []);
    }
    customerOrders.get(order.customerId).push(order);
  }
  
  return customerOrders;
}

// Example
const orders = [
  { orderId: 1, customerId: 'C001', total: 100 },
  { orderId: 2, customerId: 'C002', total: 200 },
  { orderId: 3, customerId: 'C001', total: 150 },
];
console.log(groupOrdersByCustomer(orders));
// Map { 'C001' => [...], 'C002' => [...] }
```

### 17. Calculate User Activity Streaks
**Scenario:** Track consecutive days a user has logged into the app.

```javascript
function calculateUserStreaks(loginDates) {
  const dateCount = new Map();
  
  for (const date of loginDates) {
    const dateStr = date.toISOString().split('T')[0];
    dateCount.set(dateStr, (dateCount.get(dateStr) || 0) + 1);
  }
  
  const uniqueDates = [...dateCount.keys()].sort();
  let maxStreak = 0;
  let currentStreak = 1;
  
  for (let i = 1; i < uniqueDates.length; i++) {
    const prevDate = new Date(uniqueDates[i - 1]);
    const currDate = new Date(uniqueDates[i]);
    const dayDiff = (currDate - prevDate) / (1000 * 60 * 60 * 24);
    
    if (dayDiff === 1) {
      currentStreak++;
    } else {
      maxStreak = Math.max(maxStreak, currentStreak);
      currentStreak = 1;
    }
  }
  
  return Math.max(maxStreak, currentStreak);
}

// Example
const logins = [
  new Date('2025-01-01'),
  new Date('2025-01-02'),
  new Date('2025-01-03'),
  new Date('2025-01-05')
];
console.log(calculateUserStreaks(logins)); // 3
```

### 18. Implement LRU Cache
**Scenario:** Create a Least Recently Used cache for a web application.

```javascript
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity;
    this.cache = new Map();
  }
  
  get(key) {
    if (!this.cache.has(key)) return -1;
    
    const value = this.cache.get(key);
    this.cache.delete(key);
    this.cache.set(key, value);
    return value;
  }
  
  put(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key);
    } else if (this.cache.size >= this.capacity) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    this.cache.set(key, value);
  }
}

// Example
const lru = new LRUCache(2);
lru.put(1, 'one');
lru.put(2, 'two');
console.log(lru.get(1)); // 'one'
lru.put(3, 'three'); // evicts key 2
console.log(lru.get(2)); // -1 (not found)
```

### 19. Track Page View Analytics
**Scenario:** Track which pages users visit most frequently on a website.

```javascript
class PageViewAnalytics {
  constructor() {
    this.pageViews = new Map();
  }
  
  recordView(page) {
    this.pageViews.set(page, (this.pageViews.get(page) || 0) + 1);
  }
  
  getTopPages(n = 5) {
    return [...this.pageViews.entries()]
      .sort((a, b) => b[1] - a[1])
      .slice(0, n)
      .map(([page, views]) => ({ page, views }));
  }
  
  getTotalViews() {
    return [...this.pageViews.values()].reduce((sum, v) => sum + v, 0);
  }
}

// Example
const analytics = new PageViewAnalytics();
analytics.recordView('/home');
analytics.recordView('/products');
analytics.recordView('/home');
analytics.recordView('/about');
analytics.recordView('/home');
console.log(analytics.getTopPages(2));
// [{ page: '/home', views: 3 }, { page: '/products', views: 1 }]
```

### 20. Two Sum Problem with HashMap
**Scenario:** In a payment system, find two transaction amounts that sum to a target amount.

```javascript
function findTwoTransactions(transactions, target) {
  const seen = new Map();
  
  for (let i = 0; i < transactions.length; i++) {
    const complement = target - transactions[i];
    
    if (seen.has(complement)) {
      return [seen.get(complement), i];
    }
    
    seen.set(transactions[i], i);
  }
  
  return null;
}

// Example
const transactions = [10, 25, 30, 45, 50];
console.log(findTwoTransactions(transactions, 75)); // [2, 3] (30 + 45)
```

---

## Sorting Questions

### 21. Sort Products by Price
**Scenario:** An e-commerce site needs to sort products by price (low to high or high to low).

```javascript
function sortProductsByPrice(products, order = 'asc') {
  return [...products].sort((a, b) => {
    return order === 'asc' ? a.price - b.price : b.price - a.price;
  });
}

// Example
const products = [
  { name: 'Laptop', price: 999 },
  { name: 'Mouse', price: 25 },
  { name: 'Keyboard', price: 75 },
  { name: 'Monitor', price: 300 }
];
console.log(sortProductsByPrice(products, 'asc'));
// Sorted by price ascending
```

### 22. Sort Students by Multiple Criteria
**Scenario:** Sort students first by grade (descending), then by name (ascending).

```javascript
function sortStudents(students) {
  return [...students].sort((a, b) => {
    if (a.grade !== b.grade) {
      return b.grade - a.grade; // Higher grade first
    }
    return a.name.localeCompare(b.name); // Then alphabetically
  });
}

// Example
const students = [
  { name: 'Charlie', grade: 85 },
  { name: 'Alice', grade: 92 },
  { name: 'Bob', grade: 92 },
  { name: 'David', grade: 78 }
];
console.log(sortStudents(students));
// Bob, Alice, Charlie, David
```

### 23. Sort Events by Date
**Scenario:** A calendar app needs to display events in chronological order.

```javascript
function sortEventsByDate(events) {
  return [...events].sort((a, b) => new Date(a.date) - new Date(b.date));
}

// Example
const events = [
  { title: 'Meeting', date: '2025-11-15' },
  { title: 'Conference', date: '2025-10-30' },
  { title: 'Workshop', date: '2025-11-01' }
];
console.log(sortEventsByDate(events));
// Conference, Workshop, Meeting
```

### 24. Custom Sort by Priority Level
**Scenario:** Sort support tickets by priority (Critical > High > Medium > Low).

```javascript
function sortTicketsByPriority(tickets) {
  const priorityOrder = { critical: 0, high: 1, medium: 2, low: 3 };
  
  return [...tickets].sort((a, b) => {
    return priorityOrder[a.priority] - priorityOrder[b.priority];
  });
}

// Example
const tickets = [
  { id: 1, priority: 'low', title: 'UI bug' },
  { id: 2, priority: 'critical', title: 'Server down' },
  { id: 3, priority: 'high', title: 'Payment issue' },
  { id: 4, priority: 'medium', title: 'Feature request' }
];
console.log(sortTicketsByPriority(tickets));
// Critical, High, Medium, Low
```

### 25. Sort String Array by Length
**Scenario:** Sort a list of product names by their length for display purposes.

```javascript
function sortByLength(strings, order = 'asc') {
  return [...strings].sort((a, b) => {
    const diff = a.length - b.length;
    return order === 'asc' ? diff : -diff;
  });
}

// Example
const products = ['Laptop', 'Mouse', 'Keyboard', 'USB Cable', 'Monitor'];
console.log(sortByLength(products));
// ['Mouse', 'Laptop', 'Monitor', 'Keyboard', 'USB Cable']
```

### 26. Merge and Sort Multiple Lists
**Scenario:** Merge product lists from multiple suppliers and sort by price.

```javascript
function mergeAndSortSupplierLists(supplierLists) {
  const merged = supplierLists.flat();
  return merged.sort((a, b) => a.price - b.price);
}

// Example
const supplier1 = [{ name: 'Item A', price: 50 }, { name: 'Item B', price: 30 }];
const supplier2 = [{ name: 'Item C', price: 40 }, { name: 'Item D', price: 20 }];
console.log(mergeAndSortSupplierLists([supplier1, supplier2]));
// Sorted by price: Item D, Item B, Item C, Item A
```

### 27. Sort Leaderboard Scores
**Scenario:** A gaming platform needs to display player scores in descending order.

```javascript
function sortLeaderboard(players) {
  return [...players].sort((a, b) => {
    if (b.score !== a.score) {
      return b.score - a.score; // Higher score first
    }
    return new Date(a.timestamp) - new Date(b.timestamp); // Earlier time first
  });
}

// Example
const players = [
  { name: 'Alice', score: 1500, timestamp: '2025-10-28T10:00:00' },
  { name: 'Bob', score: 1500, timestamp: '2025-10-28T09:00:00' },
  { name: 'Charlie', score: 1800, timestamp: '2025-10-28T11:00:00' }
];
console.log(sortLeaderboard(players));
// Charlie, Bob, Alice
```

### 28. Sort Tasks by Deadline and Priority
**Scenario:** A task management app sorts tasks by approaching deadline and priority.

```javascript
function sortTasks(tasks) {
  const now = new Date();
  
  return [...tasks].sort((a, b) => {
    const daysUntilA = (new Date(a.deadline) - now) / (1000 * 60 * 60 * 24);
    const daysUntilB = (new Date(b.deadline) - now) / (1000 * 60 * 60 * 24);
    
    if (Math.abs(daysUntilA - daysUntilB) > 1) {
      return daysUntilA - daysUntilB;
    }
    
    const priorityOrder = { high: 0, medium: 1, low: 2 };
    return priorityOrder[a.priority] - priorityOrder[b.priority];
  });
}

// Example
const tasks = [
  { title: 'Task 1', deadline: '2025-10-30', priority: 'low' },
  { title: 'Task 2', deadline: '2025-10-29', priority: 'high' },
  { title: 'Task 3', deadline: '2025-10-29', priority: 'medium' }
];
console.log(sortTasks(tasks));
```

### 29. Sort Files by Size
**Scenario:** A file manager needs to sort files by size for storage management.

```javascript
function sortFilesBySize(files, order = 'desc') {
  return [...files].sort((a, b) => {
    const diff = a.sizeInBytes - b.sizeInBytes;
    return order === 'desc' ? -diff : diff;
  });
}

// Example
const files = [
  { name: 'document.pdf', sizeInBytes: 2048000 },
  { name: 'image.jpg', sizeInBytes: 512000 },
  { name: 'video.mp4', sizeInBytes: 10240000 },
  { name: 'audio.mp3', sizeInBytes: 3072000 }
];
console.log(sortFilesBySize(files, 'desc'));
// video.mp4, audio.mp3, document.pdf, image.jpg
```

### 30. Sort Items with Stability (Stable Sort)
**Scenario:** Sort orders while maintaining the original order for items with the same value.

```javascript
function stableSort(array, compareFunc) {
  return array
    .map((item, index) => ({ item, index }))
    .sort((a, b) => {
      const result = compareFunc(a.item, b.item);
      return result !== 0 ? result : a.index - b.index;
    })
    .map(({ item }) => item);
}

// Example
const orders = [
  { id: 1, total: 100, customer: 'Alice' },
  { id: 2, total: 200, customer: 'Bob' },
  { id: 3, total: 100, customer: 'Charlie' },
  { id: 4, total: 200, customer: 'David' }
];
console.log(stableSort(orders, (a, b) => a.total - b.total));
// Maintains original order for items with same total
```

---

## Searching Questions

### 31. Binary Search in Sorted Product List
**Scenario:** Quickly find a product by ID in a sorted catalog.

```javascript
function binarySearchProduct(products, targetId) {
  let left = 0;
  let right = products.length - 1;
  
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    
    if (products[mid].id === targetId) {
      return products[mid];
    }
    
    if (products[mid].id < targetId) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }
  
  return null;
}

// Example
const products = [
  { id: 101, name: 'Laptop' },
  { id: 205, name: 'Mouse' },
  { id: 340, name: 'Keyboard' },
  { id: 450, name: 'Monitor' }
];
console.log(binarySearchProduct(products, 340)); // { id: 340, name: 'Keyboard' }
```

### 32. Search Products by Multiple Filters
**Scenario:** Filter products based on category, price range, and availability.

```javascript
function searchProducts(products, filters) {
  return products.filter(product => {
    if (filters.category && product.category !== filters.category) {
      return false;
    }
    
    if (filters.minPrice && product.price < filters.minPrice) {
      return false;
    }
    
    if (filters.maxPrice && product.price > filters.maxPrice) {
      return false;
    }
    
    if (filters.inStock !== undefined && product.inStock !== filters.inStock) {
      return false;
    }
    
    return true;
  });
}

// Example
const products = [
  { name: 'Laptop', category: 'electronics', price: 999, inStock: true },
  { name: 'Desk', category: 'furniture', price: 299, inStock: false },
  { name: 'Mouse', category: 'electronics', price: 25, inStock: true }
];
const filters = { category: 'electronics', minPrice: 20, maxPrice: 100, inStock: true };
console.log(searchProducts(products, filters)); // [Mouse]
```

### 33. Fuzzy Search for Autocomplete
**Scenario:** Implement autocomplete that finds matches even with typos.

```javascript
function fuzzySearch(items, query) {
  const lowerQuery = query.toLowerCase();
  
  return items.filter(item => {
    const lowerItem = item.toLowerCase();
    let queryIndex = 0;
    
    for (let i = 0; i < lowerItem.length && queryIndex < lowerQuery.length; i++) {
      if (lowerItem[i] === lowerQuery[queryIndex]) {
        queryIndex++;
      }
    }
    
    return queryIndex === lowerQuery.length;
  }).sort((a, b) => {
    const aIndex = a.toLowerCase().indexOf(lowerQuery);
    const bIndex = b.toLowerCase().indexOf(lowerQuery);
    
    if (aIndex !== -1 && bIndex === -1) return -1;
    if (aIndex === -1 && bIndex !== -1) return 1;
    
    return a.length - b.length;
  });
}

// Example
const cities = ['New York', 'Los Angeles', 'Chicago', 'Houston', 'Phoenix'];
console.log(fuzzySearch(cities, 'hix')); // ['Phoenix']
console.log(fuzzySearch(cities, 'nw')); // ['New York']
```

### 34. Find Nearest Location
**Scenario:** Find the nearest store location based on user coordinates.

```javascript
function findNearestLocation(userLocation, storeLocations) {
  function calculateDistance(loc1, loc2) {
    const dx = loc1.lat - loc2.lat;
    const dy = loc1.lng - loc2.lng;
    return Math.sqrt(dx * dx + dy * dy);
  }
  
  let nearest = null;
  let minDistance = Infinity;
  
  for (const store of storeLocations) {
    const distance = calculateDistance(userLocation, store.location);
    
    if (distance < minDistance) {
      minDistance = distance;
      nearest = store;
    }
  }
  
  return