# JavaScript Array.reduce() Method - 30 Real-World Industry Questions

## E-Commerce & Retail

### 1. Calculate Total Cart Value
**Question:** Given an array of cart items with `price` and `quantity`, calculate the total cart value.
```javascript
const cart = [
  { name: 'Laptop', price: 999, quantity: 1 },
  { name: 'Mouse', price: 25, quantity: 2 },
  { name: 'Keyboard', price: 75, quantity: 1 }
];
// Expected output: 1124
```

### 2. Apply Discount Tiers
**Question:** Calculate total revenue after applying tiered discounts (5% for orders >$500, 10% for >$1000).
```javascript
const orders = [
  { orderId: 1, amount: 450 },
  { orderId: 2, amount: 800 },
  { orderId: 3, amount: 1200 }
];
```

### 3. Group Products by Category
**Question:** Transform a flat array of products into an object grouped by category.
```javascript
const products = [
  { id: 1, name: 'iPhone', category: 'Electronics' },
  { id: 2, name: 'Shirt', category: 'Clothing' },
  { id: 3, name: 'iPad', category: 'Electronics' }
];
// Expected: { Electronics: [...], Clothing: [...] }
```

## Finance & Banking

### 4. Calculate Investment Portfolio Value
**Question:** Calculate total portfolio value with stocks having `shares` and `currentPrice`.
```javascript
const portfolio = [
  { symbol: 'AAPL', shares: 10, currentPrice: 150 },
  { symbol: 'GOOGL', shares: 5, currentPrice: 2800 },
  { symbol: 'MSFT', shares: 15, currentPrice: 300 }
];
```

### 5. Transaction Balance Calculator
**Question:** Calculate final account balance from transactions (credits and debits).
```javascript
const transactions = [
  { type: 'credit', amount: 1000 },
  { type: 'debit', amount: 250 },
  { type: 'credit', amount: 500 },
  { type: 'debit', amount: 100 }
];
```

### 6. Compound Interest Over Time
**Question:** Calculate compound interest for multiple investment periods.
```javascript
const investments = [
  { principal: 1000, rate: 0.05, years: 1 },
  { principal: 2000, rate: 0.07, years: 2 }
];
// Formula: principal * (1 + rate)^years
```

## Healthcare & Medical

### 7. Calculate Average Patient Wait Time
**Question:** Find average wait time across multiple hospital departments.
```javascript
const departments = [
  { name: 'Emergency', waitTime: 15, patients: 20 },
  { name: 'Cardiology', waitTime: 30, patients: 10 },
  { name: 'Orthopedics', waitTime: 45, patients: 5 }
];
// Weight by number of patients
```

### 8. Aggregate Medical Test Results
**Question:** Summarize test results showing how many are normal, abnormal, and critical.
```javascript
const testResults = [
  { patientId: 1, result: 'normal' },
  { patientId: 2, result: 'abnormal' },
  { patientId: 3, result: 'critical' },
  { patientId: 4, result: 'normal' }
];
```

### 9. Calculate Total Medication Dosage
**Question:** Sum total daily dosage across multiple medications.
```javascript
const medications = [
  { name: 'Med A', dosage: 500, frequency: 2 },
  { name: 'Med B', dosage: 250, frequency: 3 }
];
// Total = dosage * frequency per day
```

## Education & E-Learning

### 10. Calculate Student GPA
**Question:** Calculate GPA from courses with credits and grades (A=4, B=3, C=2, D=1, F=0).
```javascript
const courses = [
  { name: 'Math', credits: 3, grade: 'A' },
  { name: 'Physics', credits: 4, grade: 'B' },
  { name: 'Chemistry', credits: 3, grade: 'A' }
];
```

### 11. Aggregate Quiz Scores by Topic
**Question:** Group quiz scores by subject and calculate average per subject.
```javascript
const quizzes = [
  { subject: 'Math', score: 85 },
  { subject: 'Science', score: 90 },
  { subject: 'Math', score: 78 },
  { subject: 'Science', score: 88 }
];
```

### 12. Calculate Course Completion Rate
**Question:** Determine overall course completion percentage across all students.
```javascript
const students = [
  { name: 'Alice', completed: 8, total: 10 },
  { name: 'Bob', completed: 6, total: 10 },
  { name: 'Charlie', completed: 10, total: 10 }
];
```

## Data Analytics & Business Intelligence

### 13. Flatten Nested Data Structure
**Question:** Flatten a nested array of arrays into a single array.
```javascript
const nestedData = [[1, 2], [3, 4], [5, 6, 7]];
// Expected: [1, 2, 3, 4, 5, 6, 7]
```

### 14. Count Word Frequency
**Question:** Count frequency of each word in an array.
```javascript
const words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple'];
// Expected: { apple: 3, banana: 2, cherry: 1 }
```

### 15. Calculate Moving Average
**Question:** Calculate a 3-period moving average for sales data.
```javascript
const sales = [100, 150, 120, 200, 180, 160];
// Moving avg of every 3 consecutive values
```

## Logistics & Supply Chain

### 16. Calculate Total Shipping Weight
**Question:** Sum total weight of all packages in a shipment.
```javascript
const packages = [
  { id: 1, weight: 2.5, quantity: 3 },
  { id: 2, weight: 1.2, quantity: 5 },
  { id: 3, weight: 3.0, quantity: 2 }
];
```

### 17. Track Inventory by Warehouse
**Question:** Aggregate inventory quantities grouped by warehouse location.
```javascript
const inventory = [
  { product: 'A', warehouse: 'East', quantity: 100 },
  { product: 'B', warehouse: 'West', quantity: 150 },
  { product: 'C', warehouse: 'East', quantity: 200 }
];
```

### 18. Calculate Delivery Time Statistics
**Question:** Find total, average, min, and max delivery times in one pass.
```javascript
const deliveries = [
  { orderId: 1, deliveryTime: 2.5 },
  { orderId: 2, deliveryTime: 1.8 },
  { orderId: 3, deliveryTime: 3.2 },
  { orderId: 4, deliveryTime: 2.1 }
];
```

## Human Resources & Payroll

### 19. Calculate Total Payroll
**Question:** Calculate total monthly payroll including base salary and bonuses.
```javascript
const employees = [
  { name: 'John', salary: 5000, bonus: 500 },
  { name: 'Jane', salary: 6000, bonus: 800 },
  { name: 'Bob', salary: 4500, bonus: 300 }
];
```

### 20. Group Employees by Department
**Question:** Create an object with departments as keys and employee arrays as values.
```javascript
const staff = [
  { name: 'Alice', department: 'Engineering' },
  { name: 'Bob', department: 'Marketing' },
  { name: 'Charlie', department: 'Engineering' }
];
```

### 21. Calculate Average Years of Experience
**Question:** Find average years of experience across all employees.
```javascript
const team = [
  { name: 'Alice', experience: 5 },
  { name: 'Bob', experience: 3 },
  { name: 'Charlie', experience: 8 }
];
```

## Social Media & Content Management

### 22. Calculate Total Engagement Rate
**Question:** Sum likes, shares, and comments across all posts.
```javascript
const posts = [
  { id: 1, likes: 150, shares: 20, comments: 30 },
  { id: 2, likes: 200, shares: 35, comments: 45 },
  { id: 3, likes: 180, shares: 25, comments: 40 }
];
```

### 23. Build Hashtag Frequency Map
**Question:** Count how many times each hashtag appears.
```javascript
const posts = [
  { content: 'Post 1', hashtags: ['javascript', 'coding'] },
  { content: 'Post 2', hashtags: ['javascript', 'webdev'] },
  { content: 'Post 3', hashtags: ['coding', 'webdev'] }
];
```

### 24. Aggregate User Activity by Hour
**Question:** Group activities by hour of the day.
```javascript
const activities = [
  { user: 'Alice', timestamp: '2024-01-01T08:30:00Z' },
  { user: 'Bob', timestamp: '2024-01-01T08:45:00Z' },
  { user: 'Charlie', timestamp: '2024-01-01T14:20:00Z' }
];
```

## Gaming & Entertainment

### 25. Calculate Total Game Score
**Question:** Sum scores across multiple game levels with multipliers.
```javascript
const levels = [
  { level: 1, score: 1000, multiplier: 1 },
  { level: 2, score: 1500, multiplier: 1.5 },
  { level: 3, score: 2000, multiplier: 2 }
];
```

### 26. Build Player Leaderboard
**Question:** Transform player scores into a sorted leaderboard object.
```javascript
const matches = [
  { player: 'Alice', score: 100 },
  { player: 'Bob', score: 150 },
  { player: 'Alice', score: 120 }
];
// Sum scores per player
```

## IoT & Smart Devices

### 27. Calculate Average Sensor Readings
**Question:** Average temperature readings from multiple sensors.
```javascript
const sensors = [
  { id: 'S1', readings: [22, 23, 21, 24] },
  { id: 'S2', readings: [20, 21, 22, 23] },
  { id: 'S3', readings: [25, 26, 24, 25] }
];
// Average all readings across all sensors
```

### 28. Aggregate Energy Consumption
**Question:** Calculate total energy consumption per room from smart meters.
```javascript
const meters = [
  { room: 'Living Room', device: 'TV', kwh: 2.5 },
  { room: 'Kitchen', device: 'Fridge', kwh: 3.2 },
  { room: 'Living Room', device: 'AC', kwh: 4.1 }
];
```

## Travel & Hospitality

### 29. Calculate Trip Duration
**Question:** Sum total travel hours from multiple flight segments.
```javascript
const flights = [
  { from: 'NYC', to: 'LAX', duration: 6 },
  { from: 'LAX', to: 'Tokyo', duration: 11 },
  { from: 'Tokyo', to: 'Seoul', duration: 2 }
];
// Include 2-hour layover between each flight
```

### 30. Aggregate Hotel Ratings
**Question:** Calculate overall hotel rating from multiple review categories.
```javascript
const reviews = [
  { category: 'Cleanliness', rating: 4.5, weight: 0.3 },
  { category: 'Service', rating: 4.0, weight: 0.3 },
  { category: 'Location', rating: 5.0, weight: 0.2 },
  { category: 'Value', rating: 3.5, weight: 0.2 }
];
// Weighted average
```

---

## Tips for Using reduce()

1. **Always provide an initial value** to avoid unexpected behavior with empty arrays
2. **Use descriptive accumulator names** (e.g., `totalPrice`, `groupedData`) instead of generic `acc`
3. **Remember to return the accumulator** in each iteration
4. **Consider readability** - sometimes `map()` + `filter()` is clearer than complex `reduce()`
5. **Use reduce for multiple transformations** in a single pass for better performance

---

*Practice these questions to master the `reduce()` method and become proficient in data transformation tasks across various industries!*