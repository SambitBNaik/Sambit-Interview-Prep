# JavaScript Array.reduce() Method - Answers to 30 Real-World Questions

## E-Commerce & Retail

### 1. Calculate Total Cart Value
```javascript
const cart = [
  { name: 'Laptop', price: 999, quantity: 1 },
  { name: 'Mouse', price: 25, quantity: 2 },
  { name: 'Keyboard', price: 75, quantity: 1 }
];

const totalValue = cart.reduce((total, item) => {
  return total + (item.price * item.quantity);
}, 0);

console.log(totalValue); // 1124
```

### 2. Apply Discount Tiers
```javascript
const orders = [
  { orderId: 1, amount: 450 },
  { orderId: 2, amount: 800 },
  { orderId: 3, amount: 1200 }
];

const totalRevenue = orders.reduce((total, order) => {
  let discountedAmount = order.amount;
  
  if (order.amount > 1000) {
    discountedAmount = order.amount * 0.9; // 10% discount
  } else if (order.amount > 500) {
    discountedAmount = order.amount * 0.95; // 5% discount
  }
  
  return total + discountedAmount;
}, 0);

console.log(totalRevenue); // 2290 (450 + 760 + 1080)
```

### 3. Group Products by Category
```javascript
const products = [
  { id: 1, name: 'iPhone', category: 'Electronics' },
  { id: 2, name: 'Shirt', category: 'Clothing' },
  { id: 3, name: 'iPad', category: 'Electronics' }
];

const groupedProducts = products.reduce((grouped, product) => {
  const category = product.category;
  
  if (!grouped[category]) {
    grouped[category] = [];
  }
  
  grouped[category].push(product);
  return grouped;
}, {});

console.log(groupedProducts);
// {
//   Electronics: [{ id: 1, name: 'iPhone', category: 'Electronics' }, { id: 3, name: 'iPad', category: 'Electronics' }],
//   Clothing: [{ id: 2, name: 'Shirt', category: 'Clothing' }]
// }
```

## Finance & Banking

### 4. Calculate Investment Portfolio Value
```javascript
const portfolio = [
  { symbol: 'AAPL', shares: 10, currentPrice: 150 },
  { symbol: 'GOOGL', shares: 5, currentPrice: 2800 },
  { symbol: 'MSFT', shares: 15, currentPrice: 300 }
];

const totalValue = portfolio.reduce((total, stock) => {
  return total + (stock.shares * stock.currentPrice);
}, 0);

console.log(totalValue); // 20000
```

### 5. Transaction Balance Calculator
```javascript
const transactions = [
  { type: 'credit', amount: 1000 },
  { type: 'debit', amount: 250 },
  { type: 'credit', amount: 500 },
  { type: 'debit', amount: 100 }
];

const balance = transactions.reduce((total, transaction) => {
  if (transaction.type === 'credit') {
    return total + transaction.amount;
  } else {
    return total - transaction.amount;
  }
}, 0);

console.log(balance); // 1150
```

### 6. Compound Interest Over Time
```javascript
const investments = [
  { principal: 1000, rate: 0.05, years: 1 },
  { principal: 2000, rate: 0.07, years: 2 }
];

const totalValue = investments.reduce((total, investment) => {
  const finalAmount = investment.principal * Math.pow(1 + investment.rate, investment.years);
  return total + finalAmount;
}, 0);

console.log(totalValue.toFixed(2)); // 3347.98
```

## Healthcare & Medical

### 7. Calculate Average Patient Wait Time
```javascript
const departments = [
  { name: 'Emergency', waitTime: 15, patients: 20 },
  { name: 'Cardiology', waitTime: 30, patients: 10 },
  { name: 'Orthopedics', waitTime: 45, patients: 5 }
];

const { totalWaitTime, totalPatients } = departments.reduce((acc, dept) => {
  return {
    totalWaitTime: acc.totalWaitTime + (dept.waitTime * dept.patients),
    totalPatients: acc.totalPatients + dept.patients
  };
}, { totalWaitTime: 0, totalPatients: 0 });

const averageWaitTime = totalWaitTime / totalPatients;
console.log(averageWaitTime.toFixed(2)); // 22.14 minutes
```

### 8. Aggregate Medical Test Results
```javascript
const testResults = [
  { patientId: 1, result: 'normal' },
  { patientId: 2, result: 'abnormal' },
  { patientId: 3, result: 'critical' },
  { patientId: 4, result: 'normal' }
];

const summary = testResults.reduce((counts, test) => {
  counts[test.result] = (counts[test.result] || 0) + 1;
  return counts;
}, {});

console.log(summary); // { normal: 2, abnormal: 1, critical: 1 }
```

### 9. Calculate Total Medication Dosage
```javascript
const medications = [
  { name: 'Med A', dosage: 500, frequency: 2 },
  { name: 'Med B', dosage: 250, frequency: 3 }
];

const totalDailyDosage = medications.reduce((total, med) => {
  return total + (med.dosage * med.frequency);
}, 0);

console.log(totalDailyDosage); // 1750 mg
```

## Education & E-Learning

### 10. Calculate Student GPA
```javascript
const courses = [
  { name: 'Math', credits: 3, grade: 'A' },
  { name: 'Physics', credits: 4, grade: 'B' },
  { name: 'Chemistry', credits: 3, grade: 'A' }
];

const gradePoints = { A: 4, B: 3, C: 2, D: 1, F: 0 };

const { totalPoints, totalCredits } = courses.reduce((acc, course) => {
  const points = gradePoints[course.grade] * course.credits;
  return {
    totalPoints: acc.totalPoints + points,
    totalCredits: acc.totalCredits + course.credits
  };
}, { totalPoints: 0, totalCredits: 0 });

const gpa = totalPoints / totalCredits;
console.log(gpa.toFixed(2)); // 3.70
```

### 11. Aggregate Quiz Scores by Topic
```javascript
const quizzes = [
  { subject: 'Math', score: 85 },
  { subject: 'Science', score: 90 },
  { subject: 'Math', score: 78 },
  { subject: 'Science', score: 88 }
];

const averagesBySubject = quizzes.reduce((acc, quiz) => {
  if (!acc[quiz.subject]) {
    acc[quiz.subject] = { total: 0, count: 0 };
  }
  
  acc[quiz.subject].total += quiz.score;
  acc[quiz.subject].count += 1;
  
  return acc;
}, {});

// Calculate averages
const result = Object.keys(averagesBySubject).reduce((acc, subject) => {
  acc[subject] = averagesBySubject[subject].total / averagesBySubject[subject].count;
  return acc;
}, {});

console.log(result); // { Math: 81.5, Science: 89 }
```

### 12. Calculate Course Completion Rate
```javascript
const students = [
  { name: 'Alice', completed: 8, total: 10 },
  { name: 'Bob', completed: 6, total: 10 },
  { name: 'Charlie', completed: 10, total: 10 }
];

const { totalCompleted, totalCourses } = students.reduce((acc, student) => {
  return {
    totalCompleted: acc.totalCompleted + student.completed,
    totalCourses: acc.totalCourses + student.total
  };
}, { totalCompleted: 0, totalCourses: 0 });

const completionRate = (totalCompleted / totalCourses) * 100;
console.log(completionRate.toFixed(2) + '%'); // 80.00%
```

## Data Analytics & Business Intelligence

### 13. Flatten Nested Data Structure
```javascript
const nestedData = [[1, 2], [3, 4], [5, 6, 7]];

const flattened = nestedData.reduce((flat, arr) => {
  return flat.concat(arr);
}, []);

console.log(flattened); // [1, 2, 3, 4, 5, 6, 7]

// Alternative using spread operator
const flattened2 = nestedData.reduce((flat, arr) => [...flat, ...arr], []);
```

### 14. Count Word Frequency
```javascript
const words = ['apple', 'banana', 'apple', 'cherry', 'banana', 'apple'];

const frequency = words.reduce((counts, word) => {
  counts[word] = (counts[word] || 0) + 1;
  return counts;
}, {});

console.log(frequency); // { apple: 3, banana: 2, cherry: 1 }
```

### 15. Calculate Moving Average
```javascript
const sales = [100, 150, 120, 200, 180, 160];
const period = 3;

const movingAverages = sales.reduce((averages, value, index, arr) => {
  if (index >= period - 1) {
    const sum = arr.slice(index - period + 1, index + 1).reduce((a, b) => a + b, 0);
    averages.push(sum / period);
  }
  return averages;
}, []);

console.log(movingAverages); // [123.33, 156.67, 166.67, 180]
```

## Logistics & Supply Chain

### 16. Calculate Total Shipping Weight
```javascript
const packages = [
  { id: 1, weight: 2.5, quantity: 3 },
  { id: 2, weight: 1.2, quantity: 5 },
  { id: 3, weight: 3.0, quantity: 2 }
];

const totalWeight = packages.reduce((total, pkg) => {
  return total + (pkg.weight * pkg.quantity);
}, 0);

console.log(totalWeight); // 19.5 kg
```

### 17. Track Inventory by Warehouse
```javascript
const inventory = [
  { product: 'A', warehouse: 'East', quantity: 100 },
  { product: 'B', warehouse: 'West', quantity: 150 },
  { product: 'C', warehouse: 'East', quantity: 200 }
];

const inventoryByWarehouse = inventory.reduce((acc, item) => {
  if (!acc[item.warehouse]) {
    acc[item.warehouse] = [];
  }
  
  acc[item.warehouse].push({
    product: item.product,
    quantity: item.quantity
  });
  
  return acc;
}, {});

console.log(inventoryByWarehouse);
// {
//   East: [{ product: 'A', quantity: 100 }, { product: 'C', quantity: 200 }],
//   West: [{ product: 'B', quantity: 150 }]
// }
```

### 18. Calculate Delivery Time Statistics
```javascript
const deliveries = [
  { orderId: 1, deliveryTime: 2.5 },
  { orderId: 2, deliveryTime: 1.8 },
  { orderId: 3, deliveryTime: 3.2 },
  { orderId: 4, deliveryTime: 2.1 }
];

const stats = deliveries.reduce((acc, delivery) => {
  return {
    total: acc.total + delivery.deliveryTime,
    count: acc.count + 1,
    min: Math.min(acc.min, delivery.deliveryTime),
    max: Math.max(acc.max, delivery.deliveryTime)
  };
}, { total: 0, count: 0, min: Infinity, max: -Infinity });

stats.average = stats.total / stats.count;

console.log(stats);
// { total: 9.6, count: 4, min: 1.8, max: 3.2, average: 2.4 }
```

## Human Resources & Payroll

### 19. Calculate Total Payroll
```javascript
const employees = [
  { name: 'John', salary: 5000, bonus: 500 },
  { name: 'Jane', salary: 6000, bonus: 800 },
  { name: 'Bob', salary: 4500, bonus: 300 }
];

const totalPayroll = employees.reduce((total, employee) => {
  return total + employee.salary + employee.bonus;
}, 0);

console.log(totalPayroll); // 17100
```

### 20. Group Employees by Department
```javascript
const staff = [
  { name: 'Alice', department: 'Engineering' },
  { name: 'Bob', department: 'Marketing' },
  { name: 'Charlie', department: 'Engineering' }
];

const byDepartment = staff.reduce((groups, employee) => {
  const dept = employee.department;
  
  if (!groups[dept]) {
    groups[dept] = [];
  }
  
  groups[dept].push(employee);
  return groups;
}, {});

console.log(byDepartment);
// {
//   Engineering: [{ name: 'Alice', ... }, { name: 'Charlie', ... }],
//   Marketing: [{ name: 'Bob', ... }]
// }
```

### 21. Calculate Average Years of Experience
```javascript
const team = [
  { name: 'Alice', experience: 5 },
  { name: 'Bob', experience: 3 },
  { name: 'Charlie', experience: 8 }
];

const { totalExperience, count } = team.reduce((acc, employee) => {
  return {
    totalExperience: acc.totalExperience + employee.experience,
    count: acc.count + 1
  };
}, { totalExperience: 0, count: 0 });

const averageExperience = totalExperience / count;
console.log(averageExperience.toFixed(2)); // 5.33 years
```

## Social Media & Content Management

### 22. Calculate Total Engagement Rate
```javascript
const posts = [
  { id: 1, likes: 150, shares: 20, comments: 30 },
  { id: 2, likes: 200, shares: 35, comments: 45 },
  { id: 3, likes: 180, shares: 25, comments: 40 }
];

const totalEngagement = posts.reduce((totals, post) => {
  return {
    likes: totals.likes + post.likes,
    shares: totals.shares + post.shares,
    comments: totals.comments + post.comments,
    total: totals.total + post.likes + post.shares + post.comments
  };
}, { likes: 0, shares: 0, comments: 0, total: 0 });

console.log(totalEngagement);
// { likes: 530, shares: 80, comments: 115, total: 725 }
```

### 23. Build Hashtag Frequency Map
```javascript
const posts = [
  { content: 'Post 1', hashtags: ['javascript', 'coding'] },
  { content: 'Post 2', hashtags: ['javascript', 'webdev'] },
  { content: 'Post 3', hashtags: ['coding', 'webdev'] }
];

const hashtagFrequency = posts.reduce((frequency, post) => {
  post.hashtags.forEach(tag => {
    frequency[tag] = (frequency[tag] || 0) + 1;
  });
  return frequency;
}, {});

console.log(hashtagFrequency);
// { javascript: 2, coding: 2, webdev: 2 }
```

### 24. Aggregate User Activity by Hour
```javascript
const activities = [
  { user: 'Alice', timestamp: '2024-01-01T08:30:00Z' },
  { user: 'Bob', timestamp: '2024-01-01T08:45:00Z' },
  { user: 'Charlie', timestamp: '2024-01-01T14:20:00Z' }
];

const activityByHour = activities.reduce((groups, activity) => {
  const hour = new Date(activity.timestamp).getUTCHours();
  
  if (!groups[hour]) {
    groups[hour] = [];
  }
  
  groups[hour].push(activity);
  return groups;
}, {});

console.log(activityByHour);
// {
//   8: [{ user: 'Alice', ... }, { user: 'Bob', ... }],
//   14: [{ user: 'Charlie', ... }]
// }
```

## Gaming & Entertainment

### 25. Calculate Total Game Score
```javascript
const levels = [
  { level: 1, score: 1000, multiplier: 1 },
  { level: 2, score: 1500, multiplier: 1.5 },
  { level: 3, score: 2000, multiplier: 2 }
];

const totalScore = levels.reduce((total, level) => {
  return total + (level.score * level.multiplier);
}, 0);

console.log(totalScore); // 7250
```

### 26. Build Player Leaderboard
```javascript
const matches = [
  { player: 'Alice', score: 100 },
  { player: 'Bob', score: 150 },
  { player: 'Alice', score: 120 }
];

const leaderboard = matches.reduce((scores, match) => {
  scores[match.player] = (scores[match.player] || 0) + match.score;
  return scores;
}, {});

// Convert to sorted array
const sortedLeaderboard = Object.entries(leaderboard)
  .map(([player, score]) => ({ player, score }))
  .sort((a, b) => b.score - a.score);

console.log(leaderboard); // { Alice: 220, Bob: 150 }
console.log(sortedLeaderboard);
// [{ player: 'Alice', score: 220 }, { player: 'Bob', score: 150 }]
```

## IoT & Smart Devices

### 27. Calculate Average Sensor Readings
```javascript
const sensors = [
  { id: 'S1', readings: [22, 23, 21, 24] },
  { id: 'S2', readings: [20, 21, 22, 23] },
  { id: 'S3', readings: [25, 26, 24, 25] }
];

const { totalReadings, count } = sensors.reduce((acc, sensor) => {
  const sum = sensor.readings.reduce((a, b) => a + b, 0);
  return {
    totalReadings: acc.totalReadings + sum,
    count: acc.count + sensor.readings.length
  };
}, { totalReadings: 0, count: 0 });

const averageReading = totalReadings / count;
console.log(averageReading.toFixed(2)); // 23.00°C
```

### 28. Aggregate Energy Consumption
```javascript
const meters = [
  { room: 'Living Room', device: 'TV', kwh: 2.5 },
  { room: 'Kitchen', device: 'Fridge', kwh: 3.2 },
  { room: 'Living Room', device: 'AC', kwh: 4.1 }
];

const consumptionByRoom = meters.reduce((totals, meter) => {
  totals[meter.room] = (totals[meter.room] || 0) + meter.kwh;
  return totals;
}, {});

console.log(consumptionByRoom);
// { 'Living Room': 6.6, Kitchen: 3.2 }
```

## Travel & Hospitality

### 29. Calculate Trip Duration
```javascript
const flights = [
  { from: 'NYC', to: 'LAX', duration: 6 },
  { from: 'LAX', to: 'Tokyo', duration: 11 },
  { from: 'Tokyo', to: 'Seoul', duration: 2 }
];

const layoverTime = 2;

const totalDuration = flights.reduce((total, flight, index) => {
  const flightTime = flight.duration;
  const layover = index < flights.length - 1 ? layoverTime : 0;
  return total + flightTime + layover;
}, 0);

console.log(totalDuration); // 23 hours
```

### 30. Aggregate Hotel Ratings
```javascript
const reviews = [
  { category: 'Cleanliness', rating: 4.5, weight: 0.3 },
  { category: 'Service', rating: 4.0, weight: 0.3 },
  { category: 'Location', rating: 5.0, weight: 0.2 },
  { category: 'Value', rating: 3.5, weight: 0.2 }
];

const overallRating = reviews.reduce((total, review) => {
  return total + (review.rating * review.weight);
}, 0);

console.log(overallRating.toFixed(2)); // 4.25
```

---

## Key Takeaways

1. **Always use an initial value** - Prevents errors with empty arrays and ensures predictable behavior
2. **Return the accumulator** - Every iteration must return the accumulator for the next iteration
3. **Immutability matters** - When working with objects/arrays, create new references when needed
4. **Combine operations** - Use reduce to perform multiple calculations in a single pass
5. **Nested reduces** - Can be used for complex transformations (like flattening or grouping)

---

*Master these patterns and you'll be able to handle any data transformation challenge with confidence!*