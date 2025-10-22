# Object Methods Practice Guide

## Overview
Practice scenarios for mastering JavaScript's Object methods:
- `Object.keys(obj)` – returns an array of keys
- `Object.values(obj)` – returns an array of values
- `Object.entries(obj)` – returns an array of `[key, value]` pairs

---

## 📚 Scenario-Based Questions

### **SCENARIO 1: E-commerce Product Count**
**Difficulty:** Easy

You have a shopping cart object. Find how many different products are in the cart.

```javascript
const cart = {
  laptop: 1,
  mouse: 2,
  keyboard: 1,
  monitor: 1
};
```

**Task:** Use `Object.keys()` to count the number of products

**Expected Output:** `4`

---

### **SCENARIO 2: Restaurant Order Total**
**Difficulty:** Easy

Calculate the total price of all items ordered.

```javascript
const order = {
  burger: 12.99,
  fries: 3.99,
  drink: 2.50,
  dessert: 5.99
};
```

**Task:** Use `Object.values()` to calculate the total

**Expected Output:** `25.47`

---

### **SCENARIO 3: Student Grade Report**
**Difficulty:** Easy

Convert student grades into a readable format: "subject: grade"

```javascript
const grades = {
  math: 85,
  science: 92,
  english: 88,
  history: 90
};
```

**Task:** Use `Object.entries()` to create an array of "subject: grade" strings

**Expected Output:** `["math: 85", "science: 92", "english: 88", "history: 90"]`

---

### **SCENARIO 4: Inventory Alert**
**Difficulty:** Medium

Find which products are in stock (quantity > 0).

```javascript
const inventory = {
  apples: 50,
  bananas: 0,
  oranges: 30,
  grapes: 0,
  mangoes: 15
};
```

**Task:** Use `Object.entries()` to filter and return only items with quantity > 0

**Expected Output:** `{ apples: 50, oranges: 30, mangoes: 15 }`

---

### **SCENARIO 5: User Profile Validation**
**Difficulty:** Medium

Check if a user has filled all required profile fields.

```javascript
const userProfile = {
  username: "john_doe",
  email: "john@example.com",
  age: 25,
  bio: "Developer"
};
const requiredFields = ["username", "email", "age"];
```

**Task:** Use `Object.keys()` to check if all required fields exist

**Expected Output:** `true`

---

### **SCENARIO 6: Social Media Stats**
**Difficulty:** Medium

Find the social media platform with the most followers.

```javascript
const socialMedia = {
  instagram: 15000,
  twitter: 8000,
  facebook: 12000,
  youtube: 20000
};
```

**Task:** Use `Object.entries()` to find the platform with max followers

**Expected Output:** `"youtube"`

---

### **SCENARIO 7: Temperature Converter**
**Difficulty:** Medium

Convert all temperatures from Celsius to Fahrenheit.

```javascript
const citiesTemp = {
  london: 15,
  paris: 18,
  tokyo: 22,
  sydney: 25
};
```

**Task:** Use `Object.entries()` to create a new object with converted temperatures

**Formula:** `(C * 9/5) + 32`

**Expected Output:** `{ london: 59, paris: 64.4, tokyo: 71.6, sydney: 77 }`

---

### **SCENARIO 8: Survey Response Count**
**Difficulty:** Easy

Count how many people selected each option in a survey.

```javascript
const surveyResponses = {
  excellent: 45,
  good: 30,
  average: 15,
  poor: 5
};
```

**Task:** Use `Object.values()` to find the total number of responses

**Expected Output:** `95`

---

### **SCENARIO 9: Movie Ratings Filter**
**Difficulty:** Medium

Get only movies with ratings above 4.0.

```javascript
const movieRatings = {
  inception: 4.5,
  avatar: 4.2,
  titanic: 3.8,
  interstellar: 4.7,
  tenet: 3.5
};
```

**Task:** Use `Object.entries()` to filter and return movies with rating > 4.0

**Expected Output:** `{ inception: 4.5, avatar: 4.2, interstellar: 4.7 }`

---

### **SCENARIO 10: Configuration Merger**
**Difficulty:** Medium

Create a display string showing all config settings.

```javascript
const config = {
  theme: "dark",
  language: "en",
  notifications: true,
  autoSave: false
};
```

**Task:** Use `Object.entries()` to create strings like "theme=dark"

**Expected Output:** `["theme=dark", "language=en", "notifications=true", "autoSave=false"]`

---

### **SCENARIO 11: Product Price Increase**
**Difficulty:** Medium

Increase all product prices by 10%.

```javascript
const prices = {
  shirt: 20,
  jeans: 50,
  shoes: 80,
  hat: 15
};
```

**Task:** Use `Object.entries()` to create a new object with increased prices

**Expected Output:** `{ shirt: 22, jeans: 55, shoes: 88, hat: 16.5 }`

---

### **SCENARIO 12: Employee Bonus Eligibility**
**Difficulty:** Medium

Find employees who worked more than 40 hours (eligible for bonus).

```javascript
const hoursWorked = {
  alice: 45,
  bob: 38,
  charlie: 42,
  diana: 35,
  eve: 48
};
```

**Task:** Use `Object.entries()` to filter employees with hours > 40

**Expected Output:** `["alice", "charlie", "eve"]`

---

### **SCENARIO 13: Form Field Emptiness Check**
**Difficulty:** Easy

Check if any form field is empty.

```javascript
const formData = {
  firstName: "John",
  lastName: "Doe",
  email: "",
  phone: "1234567890"
};
```

**Task:** Use `Object.values()` to check if any field is empty

**Expected Output:** `true`

---

### **SCENARIO 14: Game Leaderboard**
**Difficulty:** Hard

Sort players by score and return as an array.

```javascript
const scores = {
  player1: 850,
  player2: 1200,
  player3: 950,
  player4: 1100
};
```

**Task:** Use `Object.entries()` to sort by score descending

**Expected Output:** `[["player2", 1200], ["player4", 1100], ["player3", 950], ["player1", 850]]`

---

### **SCENARIO 15: API Response Transformer**
**Difficulty:** Hard

Transform API data to a format suitable for a dropdown menu.

```javascript
const countries = {
  us: "United States",
  uk: "United Kingdom",
  ca: "Canada",
  au: "Australia"
};
```

**Task:** Use `Object.entries()` to create array of objects with code and name properties

**Expected Output:**
```javascript
[
  { code: "us", name: "United States" },
  { code: "uk", name: "United Kingdom" },
  { code: "ca", name: "Canada" },
  { code: "au", name: "Australia" }
]
```

---

## 💡 Solutions

### Solution 1: Product Count
```javascript
Object.keys(cart).length;
// Output: 4
```

### Solution 2: Order Total
```javascript
Object.values(order).reduce((sum, price) => sum + price, 0);
// Output: 25.47
```

### Solution 3: Grade Report
```javascript
Object.entries(grades).map(([subject, grade]) => `${subject}: ${grade}`);
// Output: ["math: 85", "science: 92", "english: 88", "history: 90"]
```

### Solution 4: In Stock Items
```javascript
Object.fromEntries(Object.entries(inventory).filter(([key, qty]) => qty > 0));
// Output: { apples: 50, oranges: 30, mangoes: 15 }
```

### Solution 5: Profile Validation
```javascript
requiredFields.every(field => Object.keys(userProfile).includes(field));
// Output: true
```

### Solution 6: Most Followers
```javascript
const maxFollowers = Object.entries(socialMedia).reduce((max, [platform, followers]) => 
  followers > max[1] ? [platform, followers] : max
);
maxFollowers[0];
// Output: "youtube"
```

### Solution 7: Temperature Converter
```javascript
Object.fromEntries(
  Object.entries(citiesTemp).map(([city, temp]) => [city, (temp * 9/5) + 32])
);
// Output: { london: 59, paris: 64.4, tokyo: 71.6, sydney: 77 }
```

### Solution 8: Total Responses
```javascript
Object.values(surveyResponses).reduce((sum, count) => sum + count, 0);
// Output: 95
```

### Solution 9: High-Rated Movies
```javascript
Object.fromEntries(
  Object.entries(movieRatings).filter(([movie, rating]) => rating > 4.0)
);
// Output: { inception: 4.5, avatar: 4.2, interstellar: 4.7 }
```

### Solution 10: Config Display
```javascript
Object.entries(config).map(([key, value]) => `${key}=${value}`);
// Output: ["theme=dark", "language=en", "notifications=true", "autoSave=false"]
```

### Solution 11: Price Increase
```javascript
Object.fromEntries(
  Object.entries(prices).map(([item, price]) => [item, price * 1.1])
);
// Output: { shirt: 22, jeans: 55, shoes: 88, hat: 16.5 }
```

### Solution 12: Bonus Eligible
```javascript
Object.entries(hoursWorked)
  .filter(([name, hours]) => hours > 40)
  .map(([name]) => name);
// Output: ["alice", "charlie", "eve"]
```

### Solution 13: Empty Field Check
```javascript
Object.values(formData).some(value => value === "");
// Output: true
```

### Solution 14: Sorted Leaderboard
```javascript
Object.entries(scores).sort((a, b) => b[1] - a[1]);
// Output: [["player2", 1200], ["player4", 1100], ["player3", 950], ["player1", 850]]
```

### Solution 15: Dropdown Format
```javascript
Object.entries(countries).map(([code, name]) => ({ code, name }));
// Output: [
//   { code: "us", name: "United States" },
//   { code: "uk", name: "United Kingdom" },
//   { code: "ca", name: "Canada" },
//   { code: "au", name: "Australia" }
// ]
```

---

## 🎯 Practice Tips

1. **Try solving each problem before looking at solutions**
2. **Run the code in your browser console or Node.js**
3. **Experiment with different approaches**
4. **Remember `Object.fromEntries()` to convert arrays back to objects**
5. **Combine these methods with array methods like `map()`, `filter()`, and `reduce()`**

---

## 📖 Quick Reference

| Method | Returns | Use Case |
|--------|---------|----------|
| `Object.keys(obj)` | Array of keys | Get property names, count properties |
| `Object.values(obj)` | Array of values | Sum values, check all values |
| `Object.entries(obj)` | Array of [key, value] pairs | Transform, filter, or sort objects |
| `Object.fromEntries(arr)` | Object | Convert array back to object |

Happy practicing! 🚀