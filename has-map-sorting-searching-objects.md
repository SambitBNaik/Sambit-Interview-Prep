# JavaScript Coding Questions: Real-World Scenarios with Solutions

## Table of Contents
1. [Set Questions](#set-questions)
2. [HashMap Questions](#hashmap-questions)
3. [Sorting Questions](#sorting-questions)
4. [Searching Questions](#searching-questions)

---

## Set Questions

### 1. Remove Duplicate Products from Shopping Cart
**Scenario**: E-commerce platform needs to remove duplicate products from user's cart
```javascript
// Real-world data
const cart = [
  {id: 1, name: "iPhone 14", price: 999},
  {id: 2, name: "MacBook Pro", price: 1999},
  {id: 1, name: "iPhone 14", price: 999},
  {id: 3, name: "AirPods", price: 179},
  {id: 2, name: "MacBook Pro", price: 1999}
];

// Solution
function removeDuplicateProducts(cart) {
  const seen = new Set();
  return cart.filter(product => {
    if (seen.has(product.id)) {
      return false;
    }
    seen.add(product.id);
    return true;
  });
}

console.log(removeDuplicateProducts(cart));
// Output: [{id: 1, name: "iPhone 14", price: 999}, {id: 2, name: "MacBook Pro", price: 1999}, {id: 3, name: "AirPods", price: 179}]
```

### 2. Find Common Skills Between Job Candidates
**Scenario**: HR system finding candidates with overlapping skills
```javascript
// Real-world data
const candidate1 = {
  name: "John Doe",
  skills: ["JavaScript", "React", "Node.js", "MongoDB", "Python"]
};
const candidate2 = {
  name: "Jane Smith", 
  skills: ["Python", "Django", "PostgreSQL", "React", "Docker"]
};

// Solution
function findCommonSkills(candidate1, candidate2) {
  const skillsSet1 = new Set(candidate1.skills);
  const commonSkills = candidate2.skills.filter(skill => skillsSet1.has(skill));
  return commonSkills;
}

console.log(findCommonSkills(candidate1, candidate2));
// Output: ["Python", "React"]
```

### 3. Find All Technologies Used in Company Projects
**Scenario**: Get unique technologies across all company projects
```javascript
// Real-world data
const projects = [
  {name: "E-commerce Site", technologies: ["React", "Node.js", "MongoDB", "AWS"]},
  {name: "Mobile App", technologies: ["React Native", "Firebase", "Node.js"]},
  {name: "Analytics Dashboard", technologies: ["Vue.js", "Python", "PostgreSQL", "AWS"]},
  {name: "API Gateway", technologies: ["Node.js", "Express", "Redis", "Docker"]}
];

// Solution
function getAllTechnologies(projects) {
  const allTech = new Set();
  projects.forEach(project => {
    project.technologies.forEach(tech => allTech.add(tech));
  });
  return Array.from(allTech).sort();
}

console.log(getAllTechnologies(projects));
// Output: ["AWS", "Docker", "Express", "Firebase", "MongoDB", "Node.js", "PostgreSQL", "Python", "React", "React Native", "Redis", "Vue.js"]
```

### 4. Find Missing Course Prerequisites
**Scenario**: Educational platform checking missing prerequisites for a course
```javascript
// Real-world data
const course = {
  name: "Advanced React",
  prerequisites: ["HTML", "CSS", "JavaScript", "React Basics", "ES6"]
};
const studentSkills = ["HTML", "CSS", "JavaScript", "jQuery"];

// Solution
function findMissingPrerequisites(course, studentSkills) {
  const skillsSet = new Set(studentSkills);
  return course.prerequisites.filter(prereq => !skillsSet.has(prereq));
}

console.log(findMissingPrerequisites(course, studentSkills));
// Output: ["React Basics", "ES6"]
```

### 5. Check if User Has Access to All Required Features
**Scenario**: SaaS platform checking user permissions
```javascript
// Real-world data
const userPermissions = new Set(["read_posts", "write_posts", "delete_own_posts", "manage_users"]);
const requiredFeatures = ["read_posts", "write_posts", "manage_billing"];

// Solution
function hasAllRequiredAccess(userPermissions, requiredFeatures) {
  return requiredFeatures.every(feature => userPermissions.has(feature));
}

console.log(hasAllRequiredAccess(userPermissions, requiredFeatures));
// Output: false (missing "manage_billing")
```

### 6. Count Unique Visitors to Website
**Scenario**: Analytics system counting unique daily visitors
```javascript
// Real-world data
const dailyVisits = [
  {userId: "user123", timestamp: "2024-01-15T10:30:00Z"},
  {userId: "user456", timestamp: "2024-01-15T11:45:00Z"},
  {userId: "user123", timestamp: "2024-01-15T14:20:00Z"},
  {userId: "user789", timestamp: "2024-01-15T16:10:00Z"},
  {userId: "user456", timestamp: "2024-01-15T18:30:00Z"}
];

// Solution
function countUniqueVisitors(visits) {
  const uniqueVisitors = new Set();
  visits.forEach(visit => uniqueVisitors.add(visit.userId));
  return {
    totalVisits: visits.length,
    uniqueVisitors: uniqueVisitors.size,
    visitors: Array.from(uniqueVisitors)
  };
}

console.log(countUniqueVisitors(dailyVisits));
// Output: {totalVisits: 5, uniqueVisitors: 3, visitors: ["user123", "user456", "user789"]}
```

### 7. Remove Duplicate Tags from Blog Post
**Scenario**: CMS removing duplicate tags while preserving order
```javascript
// Real-world data
const blogPost = {
  title: "Getting Started with JavaScript",
  content: "...",
  tags: ["javascript", "programming", "web", "tutorial", "javascript", "beginner", "web"]
};

// Solution
function removeDuplicateTags(post) {
  const seenTags = new Set();
  const uniqueTags = [];
  
  post.tags.forEach(tag => {
    if (!seenTags.has(tag)) {
      seenTags.add(tag);
      uniqueTags.push(tag);
    }
  });
  
  return { ...post, tags: uniqueTags };
}

console.log(removeDuplicateTags(blogPost));
// Output: {..., tags: ["javascript", "programming", "web", "tutorial", "beginner"]}
```

### 8. Find Available Meeting Rooms
**Scenario**: Office management system finding free meeting rooms
```javascript
// Real-world data
const allRooms = ["Room A", "Room B", "Room C", "Room D", "Room E"];
const bookedRooms = [
  {room: "Room A", time: "10:00-11:00"},
  {room: "Room C", time: "10:00-11:00"},
  {room: "Room A", time: "14:00-15:00"}
];

// Solution
function findAvailableRooms(allRooms, bookedRooms, timeSlot) {
  const bookedAtTime = new Set();
  bookedRooms.forEach(booking => {
    if (booking.time === timeSlot) {
      bookedAtTime.add(booking.room);
    }
  });
  
  return allRooms.filter(room => !bookedAtTime.has(room));
}

console.log(findAvailableRooms(allRooms, bookedRooms, "10:00-11:00"));
// Output: ["Room B", "Room D", "Room E"]
```

### 9. Check if Order Contains All Required Items
**Scenario**: Restaurant system checking if order has all items from a combo
```javascript
// Real-world data
const comboMeal = {
  name: "Big Burger Combo",
  requiredItems: ["burger", "fries", "drink"]
};
const customerOrder = ["burger", "fries", "drink", "extra_sauce"];

// Solution
function hasAllComboItems(requiredItems, orderItems) {
  const orderSet = new Set(orderItems);
  return requiredItems.every(item => orderSet.has(item));
}

console.log(hasAllComboItems(comboMeal.requiredItems, customerOrder));
// Output: true
```

### 10. Find Customers Who Bought Both Products
**Scenario**: E-commerce finding customers who bought specific product combinations
```javascript
// Real-world data
const purchases = [
  {customerId: "C001", products: ["laptop", "mouse", "keyboard"]},
  {customerId: "C002", products: ["laptop", "headphones"]},
  {customerId: "C003", products: ["mouse", "keyboard", "monitor"]},
  {customerId: "C004", products: ["laptop", "mouse", "headphones"]}
];

// Solution
function findCustomersWithBothProducts(purchases, product1, product2) {
  return purchases
    .filter(purchase => {
      const productSet = new Set(purchase.products);
      return productSet.has(product1) && productSet.has(product2);
    })
    .map(purchase => purchase.customerId);
}

console.log(findCustomersWithBothProducts(purchases, "laptop", "mouse"));
// Output: ["C001", "C004"]
```

### 11. Group Students by Common Subjects
**Scenario**: School system finding students with overlapping subjects
```javascript
// Real-world data
const students = [
  {name: "Alice", subjects: ["Math", "Physics", "Chemistry"]},
  {name: "Bob", subjects: ["Math", "Biology", "Chemistry"]},
  {name: "Charlie", subjects: ["Physics", "Chemistry", "Biology"]}
];

// Solution
function findStudentsWithCommonSubjects(students, targetSubjects) {
  const targetSet = new Set(targetSubjects);
  return students.filter(student => {
    const studentSet = new Set(student.subjects);
    return targetSubjects.some(subject => studentSet.has(subject));
  });
}

console.log(findStudentsWithCommonSubjects(students, ["Math", "Physics"]));
// Output: [{name: "Alice", ...}, {name: "Bob", ...}, {name: "Charlie", ...}]
```

### 12. Find Duplicate Email Addresses
**Scenario**: User management system finding duplicate registrations
```javascript
// Real-world data
const registrations = [
  {name: "John Doe", email: "john@example.com"},
  {name: "Jane Smith", email: "jane@example.com"},
  {name: "John Smith", email: "john@example.com"},
  {name: "Bob Wilson", email: "bob@example.com"}
];

// Solution
function findDuplicateEmails(registrations) {
  const emailCount = new Map();
  const duplicates = new Set();
  
  registrations.forEach(reg => {
    if (emailCount.has(reg.email)) {
      duplicates.add(reg.email);
    } else {
      emailCount.set(reg.email, 1);
    }
  });
  
  return Array.from(duplicates);
}

console.log(findDuplicateEmails(registrations));
// Output: ["john@example.com"]
```

### 13. Check Website Contains All Required Meta Tags
**Scenario**: SEO audit tool checking for required meta tags
```javascript
// Real-world data
const webpage = {
  url: "https://example.com",
  metaTags: ["title", "description", "keywords", "author", "viewport"]
};
const seoRequirements = ["title", "description", "keywords", "og:title", "og:description"];

// Solution
function checkSEOCompliance(webpage, requirements) {
  const pageTagsSet = new Set(webpage.metaTags);
  const missing = requirements.filter(req => !pageTagsSet.has(req));
  return {
    compliant: missing.length === 0,
    missingTags: missing,
    score: ((requirements.length - missing.length) / requirements.length * 100).toFixed(1)
  };
}

console.log(checkSEOCompliance(webpage, seoRequirements));
// Output: {compliant: false, missingTags: ["og:title", "og:description"], score: "60.0"}
```

### 14. Convert Shopping Categories to Unique Set
**Scenario**: E-commerce organizing product categories
```javascript
// Real-world data
const products = [
  {name: "iPhone", categories: ["Electronics", "Mobile", "Apple"]},
  {name: "MacBook", categories: ["Electronics", "Computers", "Apple"]},
  {name: "Nike Shoes", categories: ["Fashion", "Footwear", "Sports"]},
  {name: "iPad", categories: ["Electronics", "Tablets", "Apple"]}
];

// Solution
function getUniqueCategories(products) {
  const categorySet = new Set();
  products.forEach(product => {
    product.categories.forEach(category => categorySet.add(category));
  });
  return Array.from(categorySet).sort();
}

console.log(getUniqueCategories(products));
// Output: ["Apple", "Computers", "Electronics", "Fashion", "Footwear", "Mobile", "Sports", "Tablets"]
```

### 15. Find Longest Unique Session without Repeated Actions
**Scenario**: User analytics finding longest session without repeated actions
```javascript
// Real-world data
const userSession = {
  userId: "user123",
  actions: ["login", "view_profile", "edit_profile", "view_dashboard", "login", "logout"]
};

// Solution
function findLongestUniqueSession(actions) {
  let maxLength = 0;
  let currentStart = 0;
  const seen = new Set();
  
  for (let i = 0; i < actions.length; i++) {
    while (seen.has(actions[i])) {
      seen.delete(actions[currentStart]);
      currentStart++;
    }
    seen.add(actions[i]);
    maxLength = Math.max(maxLength, i - currentStart + 1);
  }
  
  return maxLength;
}

console.log(findLongestUniqueSession(userSession.actions));
// Output: 4 (sequence: "view_profile", "edit_profile", "view_dashboard", "login")
```

### 16. Validate Restaurant Order Items
**Scenario**: Restaurant POS system validating menu items
```javascript
// Real-world data
const menuItems = new Set(["burger", "pizza", "pasta", "salad", "fries", "soda", "beer"]);
const order = {
  orderId: "ORD123",
  items: ["burger", "fries", "soda", "ice_cream"]
};

// Solution
function validateOrder(menuItems, order) {
  const invalidItems = order.items.filter(item => !menuItems.has(item));
  return {
    orderId: order.orderId,
    valid: invalidItems.length === 0,
    invalidItems: invalidItems,
    validItems: order.items.filter(item => menuItems.has(item))
  };
}

console.log(validateOrder(menuItems, order));
// Output: {orderId: "ORD123", valid: false, invalidItems: ["ice_cream"], validItems: ["burger", "fries", "soda"]}
```

### 17. Find Common Interests Between Users
**Scenario**: Social media platform finding users with similar interests
```javascript
// Real-world data
const users = [
  {id: "user1", interests: ["photography", "travel", "cooking", "music"]},
  {id: "user2", interests: ["travel", "music", "sports", "reading"]},
  {id: "user3", interests: ["cooking", "photography", "art", "music"]}
];

// Solution
function findUsersWithCommonInterests(users, targetUserId) {
  const targetUser = users.find(u => u.id === targetUserId);
  if (!targetUser) return [];
  
  const targetInterests = new Set(targetUser.interests);
  
  return users
    .filter(user => user.id !== targetUserId)
    .map(user => ({
      userId: user.id,
      commonInterests: user.interests.filter(interest => targetInterests.has(interest)),
      matchScore: user.interests.filter(interest => targetInterests.has(interest)).length
    }))
    .filter(result => result.matchScore > 0)
    .sort((a, b) => b.matchScore - a.matchScore);
}

console.log(findUsersWithCommonInterests(users, "user1"));
// Output: [{userId: "user3", commonInterests: ["photography", "cooking", "music"], matchScore: 3}, {userId: "user2", commonInterests: ["travel", "music"], matchScore: 2}]
```

### 18. Remove Inactive Features from User Plan
**Scenario**: SaaS platform managing user feature access
```javascript
// Real-world data
const userPlan = {
  planName: "Premium",
  features: new Set(["advanced_analytics", "api_access", "priority_support", "custom_themes", "beta_features"])
};
const activeFeatures = ["advanced_analytics", "api_access", "priority_support"];

// Solution
function removeInactiveFeatures(userPlan, activeFeatures) {
  const activeSet = new Set(activeFeatures);
  const updatedFeatures = new Set();
  
  userPlan.features.forEach(feature => {
    if (activeSet.has(feature)) {
      updatedFeatures.add(feature);
    }
  });
  
  return {
    ...userPlan,
    features: updatedFeatures,
    removedFeatures: Array.from(userPlan.features).filter(f => !activeSet.has(f))
  };
}

console.log(removeInactiveFeatures(userPlan, activeFeatures));
// Output: {..., features: Set(["advanced_analytics", "api_access", "priority_support"]), removedFeatures: ["custom_themes", "beta_features"]}
```

### 19. Check if Resume Keywords Match Job Requirements
**Scenario**: Job matching system comparing resume with job posting
```javascript
// Real-world data
const jobRequirements = ["JavaScript", "React", "Node.js", "MongoDB", "REST API", "Git"];
const resumeKeywords = ["JavaScript", "React", "HTML", "CSS", "Git", "MySQL"];

// Solution
function analyzeJobMatch(requirements, keywords) {
  const reqSet = new Set(requirements);
  const keywordSet = new Set(keywords);
  
  const matches = requirements.filter(req => keywordSet.has(req));
  const missing = requirements.filter(req => !keywordSet.has(req));
  const matchPercentage = (matches.length / requirements.length * 100).toFixed(1);
  
  return {
    matchPercentage: parseFloat(matchPercentage),
    matchedSkills: matches,
    missingSkills: missing,
    additionalSkills: keywords.filter(skill => !reqSet.has(skill))
  };
}

console.log(analyzeJobMatch(jobRequirements, resumeKeywords));
// Output: {matchPercentage: 50.0, matchedSkills: ["JavaScript", "React", "Git"], missingSkills: ["Node.js", "MongoDB", "REST API"], additionalSkills: ["HTML", "CSS", "MySQL"]}
```

### 20. Find Non-Participating Students in Event
**Scenario**: School event management finding students who haven't registered
```javascript
// Real-world data
const allStudents = ["student1", "student2", "student3", "student4", "student5"];
const eventRegistrations = [
  {eventName: "Science Fair", participants: ["student1", "student3", "student5"]},
  {eventName: "Sports Day", participants: ["student2", "student4"]},
  {eventName: "Cultural Fest", participants: ["student1", "student2", "student3"]}
];

// Solution
function findNonParticipants(allStudents, eventName, registrations) {
  const event = registrations.find(reg => reg.eventName === eventName);
  if (!event) return allStudents;
  
  const participantSet = new Set(event.participants);
  return allStudents.filter(student => !participantSet.has(student));
}

console.log(findNonParticipants(allStudents, "Science Fair", eventRegistrations));
// Output: ["student2", "student4"]
```

---

## HashMap Questions

### 1. Customer Purchase Analysis
**Scenario**: E-commerce analyzing customer buying patterns
```javascript
// Real-world data
const orders = [
  {customerId: "C001", items: ["laptop", "mouse"], total: 1200},
  {customerId: "C002", items: ["phone", "case"], total: 800},
  {customerId: "C001", items: ["keyboard", "monitor"], total: 600},
  {customerId: "C003", items: ["tablet"], total: 500},
  {customerId: "C002", items: ["headphones"], total: 150}
];

// Solution
function analyzeCustomerPurchases(orders) {
  const customerMap = new Map();
  
  orders.forEach(order => {
    if (!customerMap.has(order.customerId)) {
      customerMap.set(order.customerId, {
        totalSpent: 0,
        orderCount: 0,
        items: new Set()
      });
    }
    
    const customer = customerMap.get(order.customerId);
    customer.totalSpent += order.total;
    customer.orderCount++;
    order.items.forEach(item => customer.items.add(item));
  });
  
  return customerMap;
}

console.log(analyzeCustomerPurchases(orders));
// Output: Map with customer analysis data
```

### 2. Employee Attendance Tracker
**Scenario**: HR system tracking employee attendance frequency
```javascript
// Real-world data
const attendanceRecords = [
  {employeeId: "EMP001", name: "John Doe", date: "2024-01-15", status: "present"},
  {employeeId: "EMP002", name: "Jane Smith", date: "2024-01-15", status: "absent"},
  {employeeId: "EMP001", name: "John Doe", date: "2024-01-16", status: "present"},
  {employeeId: "EMP003", name: "Bob Wilson", date: "2024-01-15", status: "present"},
  {employeeId: "EMP002", name: "Jane Smith", date: "2024-01-16", status: "present"}
];

// Solution
function calculateAttendanceStats(records) {
  const employeeStats = new Map();
  
  records.forEach(record => {
    if (!employeeStats.has(record.employeeId)) {
      employeeStats.set(record.employeeId, {
        name: record.name,
        totalDays: 0,
        presentDays: 0,
        absentDays: 0
      });
    }
    
    const stats = employeeStats.get(record.employeeId);
    stats.totalDays++;
    if (record.status === "present") {
      stats.presentDays++;
    } else {
      stats.absentDays++;
    }
  });
  
  // Calculate attendance percentage
  employeeStats.forEach(stats => {
    stats.attendancePercentage = ((stats.presentDays / stats.totalDays) * 100).toFixed(1);
  });
  
  return employeeStats;
}

console.log(calculateAttendanceStats(attendanceRecords));
```

### 3. Inventory Stock Management
**Scenario**: Warehouse tracking product stock levels
```javascript
// Real-world data
const stockMovements = [
  {productId: "P001", type: "in", quantity: 100, date: "2024-01-15"},
  {productId: "P002", type: "in", quantity: 50, date: "2024-01-15"},
  {productId: "P001", type: "out", quantity: 30, date: "2024-01-16"},
  {productId: "P003", type: "in", quantity: 75, date: "2024-01-16"},
  {productId: "P001", type: "out", quantity: 20, date: "2024-01-17"}
];

// Solution
function calculateCurrentStock(movements) {
  const inventory = new Map();
  
  movements.forEach(movement => {
    if (!inventory.has(movement.productId)) {
      inventory.set(movement.productId, {
        currentStock: 0,
        totalIn: 0,
        totalOut: 0,
        lastActivity: null
      });
    }
    
    const product = inventory.get(movement.productId);
    
    if (movement.type === "in") {
      product.currentStock += movement.quantity;
      product.totalIn += movement.quantity;
    } else {
      product.currentStock -= movement.quantity;
      product.totalOut += movement.quantity;
    }
    
    product.lastActivity = movement.date;
  });
  
  return inventory;
}

console.log(calculateCurrentStock(stockMovements));
```

### 4. Website Page View Analytics
**Scenario**: Web analytics tracking page popularity
```javascript
// Real-world data
const pageViews = [
  {url: "/home", userId: "user1", timestamp: "2024-01-15T10:00:00Z"},
  {url: "/products", userId: "user2", timestamp: "2024-01-15T10:05:00Z"},
  {url: "/home", userId: "user3", timestamp: "2024-01-15T10:10:00Z"},
  {url: "/about", userId: "user1", timestamp: "2024-01-15T10:15:00Z"},
  {url: "/products", userId: "user4", timestamp: "2024-01-15T10:20:00Z"},
  {url: "/home", userId: "user2", timestamp: "2024-01-15T10:25:00Z"}
];

// Solution
function analyzePageViews(pageViews) {
  const pageStats = new Map();
  
  pageViews.forEach(view => {
    if (!pageStats.has(view.url)) {
      pageStats.set(view.url, {
        totalViews: 0,
        uniqueUsers: new Set(),
        firstView: view.timestamp,
        lastView: view.timestamp
      });
    }
    
    const stats = pageStats.get(view.url);
    stats.totalViews++;
    stats.uniqueUsers.add(view.userId);
    stats.lastView = view.timestamp;
  });
  
  // Convert to array and add calculated fields
  return Array.from(pageStats.entries()).map(([url, stats]) => ({
    url,
    totalViews: stats.totalViews,
    uniqueUsers: stats.uniqueUsers.size,
    firstView: stats.firstView,
    lastView: stats.lastView
  })).sort((a, b) => b.totalViews - a.totalViews);
}

console.log(analyzePageViews(pageViews));
```

### 5. Student Grade Management
**Scenario**: School system managing student grades across subjects
```javascript
// Real-world data
const gradeRecords = [
  {studentId: "S001", name: "Alice", subject: "Math", grade: 85},
  {studentId: "S002", name: "Bob", subject: "Math", grade: 78},
  {studentId: "S001", name: "Alice", subject: "Science", grade: 92},
  {studentId: "S003", name: "Charlie", subject: "Math", grade: 88},
  {studentId: "S001", name: "Alice", subject: "English", grade: 79},
  {studentId: "S002", name: "Bob", subject: "Science", grade: 85}
];

// Solution
function calculateStudentAverages(records) {
  const studentMap = new Map();
  
  records.forEach(record => {
    if (!studentMap.has(record.studentId)) {
      studentMap.set(record.studentId, {
        name: record.name,
        subjects: new Map(),
        totalGrades: 0,
        subjectCount: 0
      });
    }
    
    const student = studentMap.get(record.studentId);
    student.subjects.set(record.subject, record.grade);
    student.totalGrades += record.grade;
    student.subjectCount++;
  });
  
  // Calculate averages
  const results = [];
  studentMap.forEach((student, studentId) => {
    const average = (student.totalGrades / student.subjectCount).toFixed(2);
    results.push({
      studentId,
      name: student.name,
      subjects: Object.fromEntries(student.subjects),
      average: parseFloat(average),
      grade: average >= 90 ? 'A' : average >= 80 ? 'B' : average >= 70 ? 'C' : average >= 60 ? 'D' : 'F'
    });
  });
  
  return results.sort((a, b) => b.average - a.average);
}

console.log(calculateStudentAverages(gradeRecords));
```

### 6. Social Media Hashtag Trends
**Scenario**: Social platform analyzing trending hashtags
```javascript
// Real-world data
const posts = [
  {postId: "P001", content: "Great day! #sunny #happy #weekend", userId: "user1"},
  {postId: "P002", content: "Working on new project #coding #javascript #weekend", userId: "user2"},
  {postId: "P003", content: "Beautiful sunset #photography #nature #happy", userId: "user3"},
  {postId: "P004", content: "Learning React #coding #react #javascript #learning", userId: "user4"},
  {postId: "P005", content: "Weekend vibes #weekend #relax #happy", userId: "user5"}
];

// Solution
function analyzeHashtagTrends(posts) {
  const hashtagMap = new Map();
  
  posts.forEach(post => {
    const hashtags = post.content.match(/#\w+/g) || [];
    hashtags.forEach(hashtag => {
      const tag = hashtag.toLowerCase();
      if (!hashtagMap.has(tag)) {
        hashtagMap.set(tag, {
          count: 0,
          users: new Set(),
          posts: []
        });
      }
      
      const tagData = hashtagMap.get(tag);
      tagData.count++;
      tagData.users.add(post.userId);
      tagData.posts.push(post.postId);
    });
  });
  
  return Array.from(hashtagMap.entries())
    .map(([tag, data]) => ({
      hashtag: tag,
      usage: data.count,
      uniqueUsers: data.users.size,
      engagement: (data.count / data.users.size).toFixed(2)
    }))
    .sort((a, b) => b.usage - a.usage);
}

console.log(analyzeHashtagTrends(posts));
```

### 7. Restaurant Order Frequency
**Scenario**: Restaurant analyzing popular dishes
```javascript
// Real-world data
const restaurantOrders = [
  {orderId: "O001", items: ["burger", "fries", "coke"], timestamp: "2024-01-15T12:00:00Z"},
  {orderId: "O002", items: ["pizza", "salad"], timestamp: "2024-01-15T12:30:00Z"},
  {orderId: "O003", items: ["burger", "onion_rings", "coke"], timestamp: "2024-01-15T13:00:00Z"},
  {orderId: "O004", items: ["pasta", "garlic_bread"], timestamp: "2024-01-15T13:15:00Z"},
  {orderId: "O005", items: ["burger", "fries"], timestamp: "2024-01-15T14:00:00Z"}
];

// Solution
function analyzePopularItems(orders) {
  const itemFrequency = new Map();
  const itemCombinations = new Map();
  
  orders.forEach(order => {
    // Count individual items
    order.items.forEach(item => {
      itemFrequency.set(item, (itemFrequency.get(item) || 0) + 1);
    });
    
    // Count item combinations
    if (order.items.length > 1) {
      const combo = order.items.sort().join(", ");
      itemCombinations.set(combo, (itemCombinations.get(combo) || 0) + 1);
    }
  });
  
  return {
    popularItems: Array.from(itemFrequency.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([item, count]) => ({item, orders: count})),
    popularCombos: Array.from(itemCombinations.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([combo, count]) => ({combination: combo, orders: count}))
  };
}

console.log(analyzePopularItems(restaurantOrders));
```

### 8. Employee Skill Assessment
**Scenario**: HR tracking employee skills and proficiency levels
```javascript
// Real-world data
const skillAssessments = [
  {employeeId: "E001", name: "John", skill: "JavaScript", level: "Expert"},
  {employeeId: "E001", name: "John", skill: "React", level: "Intermediate"},
  {employeeId: "E002", name: "Jane", skill: "JavaScript", level: "Intermediate"},
  {employeeId: "E002", name: "Jane", skill: "Python", level: "Expert"},
  {employeeId: "E003", name: "Bob", skill: "JavaScript", level: "Beginner"},
  {employeeId: "E001", name: "John", skill: "Node.js", level: "Advanced"}
];

// Solution
function createSkillMatrix(assessments) {
  const employeeSkills = new Map();
  const skillDistribution = new Map();
  
  const levelPoints = {"Beginner": 1, "Intermediate": 2, "Advanced": 3, "Expert": 4};
  
  assessments.forEach(assessment => {
    // Employee skill mapping
    if (!employeeSkills.has(assessment.employeeId)) {
      employeeSkills.set(assessment.employeeId, {
        name: assessment.name,
        skills: new Map(),
        totalScore: 0
      });
    }
    
    const employee = employeeSkills.get(assessment.employeeId);
    employee.skills.set(assessment.skill, {
      level: assessment.level,
      points: levelPoints[assessment.level]
    });
    employee.totalScore += levelPoints[assessment.level];
    
    // Skill distribution
    if (!skillDistribution.has(assessment.skill)) {
      skillDistribution.set(assessment.skill, {
        Beginner: 0, Intermediate: 0, Advanced: 0, Expert: 0
      });
    }
    skillDistribution.get(assessment.skill)[assessment.level]++;
  });
  
  return {
    employeeProfiles: Array.from(employeeSkills.entries()).map(([id, data]) => ({
      employeeId: id,
      name: data.name,
      skills: Object.fromEntries(data.skills),
      totalScore: data.totalScore
    })),
    skillDistribution: Object.fromEntries(skillDistribution)
  };
}

console.log(createSkillMatrix(skillAssessments));
```

### 9. Library Book Checkout System
**Scenario**: Library managing book borrowing patterns
```javascript
// Real-world data
const checkoutRecords = [
  {bookId: "B001", title: "JavaScript Guide", userId: "U001", checkoutDate: "2024-01-15", returnDate: "2024-01-25"},
  {bookId: "B002", title: "React Handbook", userId: "U002", checkoutDate: "2024-01-16", returnDate: null},
  {bookId: "B001", title: "JavaScript Guide", userId: "U003", checkoutDate: "2024-01-26", returnDate: "2024-02-05"},
  {bookId: "B003", title: "Python Basics", userId: "U001", checkoutDate: "2024-01-20", returnDate: "2024-01-30"},
  {bookId: "B002", title: "React Handbook", userId: "U004", checkoutDate: "2024-02-01", returnDate: null}
];

// Solution
function analyzeLibraryUsage(records) {
  const bookStats = new Map();
  const userStats = new Map();
  
  records.forEach(record => {
    // Book statistics
    if (!bookStats.has(record.bookId)) {
      bookStats.set(record.bookId, {
        title: record.title,
        totalCheckouts: 0,
        currentlyBorrowed: 0,
        uniqueBorrowers: new Set()
      });
    }
    
    const bookData = bookStats.get(record.bookId);
    bookData.totalCheckouts++;
    bookData.uniqueBorrowers.add(record.userId);
    if (!record.returnDate) {
      bookData.currentlyBorrowed++;
    }
    
    // User statistics
    if (!userStats.has(record.userId)) {
      userStats.set(record.userId, {
        totalBooks: 0,
        currentBooks: 0,
        returnedBooks: 0
      });
    }
    
    const userData = userStats.get(record.userId);
    userData.totalBooks++;
    if (record.returnDate) {
      userData.returnedBooks++;
    } else {
      userData.currentBooks++;
    }
  });
  
  return {
    popularBooks: Array.from(bookStats.entries())
      .map(([bookId, stats]) => ({
        bookId,
        title: stats.title,
        popularity: stats.totalCheckouts,
        currentlyBorrowed: stats.currentlyBorrowed,
        uniqueBorrowers: stats.uniqueBorrowers.size
      }))
      .sort((a, b) => b.popularity - a.popularity),
    
    activeUsers: Array.from(userStats.entries())
      .map(([userId, stats]) => ({
        userId,
        ...stats
      }))
      .sort((a, b) => b.totalBooks - a.totalBooks)
  };
}

console.log(analyzeLibraryUsage(checkoutRecords));
```

### 10. Product Review Sentiment Analysis
**Scenario**: E-commerce analyzing product reviews and ratings
```javascript
// Real-world data
const productReviews = [
  {productId: "P001", userId: "U001", rating: 5, review: "Excellent product!", date: "2024-01-15"},
  {productId: "P001", userId: "U002", rating: 4, review: "Good quality", date: "2024-01-16"},
  {productId: "P002", userId: "U001", rating: 2, review: "Poor quality", date: "2024-01-17"},
  {productId: "P001", userId: "U003", rating: 5, review: "Amazing!", date: "2024-01-18"},
  {productId: "P003", userId: "U002", rating: 3, review: "Average product", date: "2024-01-19"}
];

// Solution
function analyzeProductReviews(reviews) {
  const productMap = new Map();
  
  reviews.forEach(review => {
    if (!productMap.has(review.productId)) {
      productMap.set(review.productId, {
        ratings: [],
        reviewCount: 0,
        ratingDistribution: {1: 0, 2: 0, 3: 0, 4: 0, 5: 0}
      });
    }
    
    const product = productMap.get(review.productId);
    product.ratings.push(review.rating);
    product.reviewCount++;
    product.ratingDistribution[review.rating]++;
  });
  
  return Array.from(productMap.entries()).map(([productId, data]) => {
    const avgRating = data.ratings.reduce((sum, rating) => sum + rating, 0) / data.ratings.length;
    const sentiment = avgRating >= 4 ? 'Positive' : avgRating >= 3 ? 'Neutral' : 'Negative';
    
    return {
      productId,
      averageRating: parseFloat(avgRating.toFixed(2)),
      totalReviews: data.reviewCount,
      sentiment,
      ratingDistribution: data.ratingDistribution
    };
  }).sort((a, b) => b.averageRating - a.averageRating);
}

console.log(analyzeProductReviews(productReviews));
```

### 11. Sales Team Performance Tracking
**Scenario**: CRM tracking sales representative performance
```javascript
// Real-world data
const salesData = [
  {repId: "REP001", name: "Alice", deal: 5000, client: "Company A", date: "2024-01-15"},
  {repId: "REP002", name: "Bob", deal: 3000, client: "Company B", date: "2024-01-16"},
  {repId: "REP001", name: "Alice", deal: 7500, client: "Company C", date: "2024-01-18"},
  {repId: "REP003", name: "Charlie", deal: 2000, client: "Company D", date: "2024-01-20"},
  {repId: "REP002", name: "Bob", deal: 4500, client: "Company E", date: "2024-01-22"}
];

// Solution
function analyzeSalesPerformance(salesData) {
  const repPerformance = new Map();
  
  salesData.forEach(sale => {
    if (!repPerformance.has(sale.repId)) {
      repPerformance.set(sale.repId, {
        name: sale.name,
        totalSales: 0,
        dealCount: 0,
        clients: new Set(),
        deals: []
      });
    }
    
    const rep = repPerformance.get(sale.repId);
    rep.totalSales += sale.deal;
    rep.dealCount++;
    rep.clients.add(sale.client);
    rep.deals.push(sale.deal);
  });
  
  return Array.from(repPerformance.entries()).map(([repId, data]) => ({
    repId,
    name: data.name,
    totalSales: data.totalSales,
    dealCount: data.dealCount,
    avgDealSize: Math.round(data.totalSales / data.dealCount),
    uniqueClients: data.clients.size,
    performance: data.totalSales >= 10000 ? 'Excellent' : data.totalSales >= 5000 ? 'Good' : 'Needs Improvement'
  })).sort((a, b) => b.totalSales - a.totalSales);
}

console.log(analyzeSalesPerformance(salesData));
```

### 12. Course Enrollment Management
**Scenario**: Educational platform managing student course enrollments
```javascript
// Real-world data
const enrollments = [
  {studentId: "S001", courseId: "C001", courseName: "JavaScript Basics", enrollDate: "2024-01-15", status: "active"},
  {studentId: "S002", courseId: "C001", courseName: "JavaScript Basics", enrollDate: "2024-01-16", status: "completed"},
  {studentId: "S001", courseId: "C002", courseName: "React Fundamentals", enrollDate: "2024-01-20", status: "active"},
  {studentId: "S003", courseId: "C001", courseName: "JavaScript Basics", enrollDate: "2024-01-18", status: "dropped"},
  {studentId: "S002", courseId: "C003", courseName: "Node.js", enrollDate: "2024-01-25", status: "active"}
];

// Solution
function analyzeCourseEnrollments(enrollments) {
  const courseMap = new Map();
  const studentMap = new Map();
  
  enrollments.forEach(enrollment => {
    // Course analysis
    if (!courseMap.has(enrollment.courseId)) {
      courseMap.set(enrollment.courseId, {
        courseName: enrollment.courseName,
        totalEnrollments: 0,
        active: 0,
        completed: 0,
        dropped: 0
      });
    }
    
    const course = courseMap.get(enrollment.courseId);
    course.totalEnrollments++;
    course[enrollment.status]++;
    
    // Student analysis
    if (!studentMap.has(enrollment.studentId)) {
      studentMap.set(enrollment.studentId, {
        totalCourses: 0,
        activeCourses: 0,
        completedCourses: 0,
        courseList: []
      });
    }
    
    const student = studentMap.get(enrollment.studentId);
    student.totalCourses++;
    if (enrollment.status === 'active') student.activeCourses++;
    if (enrollment.status === 'completed') student.completedCourses++;
    student.courseList.push(enrollment.courseName);
  });
  
  return {
    courseStats: Array.from(courseMap.entries()).map(([courseId, stats]) => ({
      courseId,
      courseName: stats.courseName,
      enrollments: stats.totalEnrollments,
      completionRate: ((stats.completed / stats.totalEnrollments) * 100).toFixed(1),
      dropoutRate: ((stats.dropped / stats.totalEnrollments) * 100).toFixed(1),
      ...stats
    })),
    studentProgress: Array.from(studentMap.entries()).map(([studentId, data]) => ({
      studentId,
      ...data
    }))
  };
}

console.log(analyzeCourseEnrollments(enrollments));
```

### 13. Event Registration System
**Scenario**: Conference managing attendee registrations and preferences
```javascript
// Real-world data
const registrations = [
  {attendeeId: "A001", name: "John", event: "Tech Conference 2024", sessions: ["AI Workshop", "React Talk"], dietary: "vegetarian"},
  {attendeeId: "A002", name: "Jane", event: "Tech Conference 2024", sessions: ["AI Workshop", "Node.js Deep Dive"], dietary: "none"},
  {attendeeId: "A003", name: "Bob", event: "Tech Conference 2024", sessions: ["React Talk", "Database Design"], dietary: "vegan"},
  {attendeeId: "A004", name: "Alice", event: "Tech Conference 2024", sessions: ["AI Workshop"], dietary: "gluten-free"}
];

// Solution
function analyzeEventRegistrations(registrations) {
  const sessionPopularity = new Map();
  const dietaryRequirements = new Map();
  const attendeeInterests = new Map();
  
  registrations.forEach(reg => {
    // Session popularity
    reg.sessions.forEach(session => {
      sessionPopularity.set(session, (sessionPopularity.get(session) || 0) + 1);
    });
    
    // Dietary requirements
    dietaryRequirements.set(reg.dietary, (dietaryRequirements.get(reg.dietary) || 0) + 1);
    
    // Attendee interests
    attendeeInterests.set(reg.attendeeId, {
      name: reg.name,
      sessions: reg.sessions,
      dietary: reg.dietary
    });
  });
  
  return {
    popularSessions: Array.from(sessionPopularity.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([session, count]) => ({session, attendees: count})),
    
    dietaryBreakdown: Object.fromEntries(dietaryRequirements),
    
    totalAttendees: registrations.length,
    
    sessionRecommendations: Array.from(sessionPopularity.entries())
      .filter(([_, count]) => count >= 2)
      .map(([session, _]) => session)
  };
}

console.log(analyzeEventRegistrations(registrations));
```

### 14. Medical Appointment Scheduling
**Scenario**: Hospital managing doctor appointments and patient history
```javascript
// Real-world data
const appointments = [
  {patientId: "P001", doctorId: "D001", specialty: "Cardiology", date: "2024-01-15", status: "completed"},
  {patientId: "P002", doctorId: "D002", specialty: "Dermatology", date: "2024-01-16", status: "scheduled"},
  {patientId: "P001", doctorId: "D003", specialty: "General", date: "2024-01-20", status: "completed"},
  {patientId: "P003", doctorId: "D001", specialty: "Cardiology", date: "2024-01-18", status: "cancelled"},
  {patientId: "P002", doctorId: "D001", specialty: "Cardiology", date: "2024-01-25", status: "scheduled"}
];

// Solution
function analyzeAppointments(appointments) {
  const doctorWorkload = new Map();
  const patientHistory = new Map();
  const specialtyDemand = new Map();
  
  appointments.forEach(apt => {
    // Doctor workload
    if (!doctorWorkload.has(apt.doctorId)) {
      doctorWorkload.set(apt.doctorId, {
        totalAppointments: 0,
        completed: 0,
        scheduled: 0,
        cancelled: 0,
        specialties: new Set()
      });
    }
    
    const doctor = doctorWorkload.get(apt.doctorId);
    doctor.totalAppointments++;
    doctor[apt.status]++;
    doctor.specialties.add(apt.specialty);
    
    // Patient history
    if (!patientHistory.has(apt.patientId)) {
      patientHistory.set(apt.patientId, {
        totalVisits: 0,
        specialtiesSeen: new Set(),
        doctorsSeen: new Set()
      });
    }
    
    const patient = patientHistory.get(apt.patientId);
    patient.totalVisits++;
    patient.specialtiesSeen.add(apt.specialty);
    patient.doctorsSeen.add(apt.doctorId);
    
    // Specialty demand
    specialtyDemand.set(apt.specialty, (specialtyDemand.get(apt.specialty) || 0) + 1);
  });
  
  return {
    doctorUtilization: Array.from(doctorWorkload.entries()).map(([doctorId, data]) => ({
      doctorId,
      ...data,
      specialties: Array.from(data.specialties),
      utilizationRate: ((data.completed / data.totalAppointments) * 100).toFixed(1)
    })),
    
    patientAnalysis: Array.from(patientHistory.entries()).map(([patientId, data]) => ({
      patientId,
      totalVisits: data.totalVisits,
      specialtiesSeen: Array.from(data.specialtiesSeen),
      doctorsSeen: data.doctorsSeen.size
    })),
    
    specialtyPopularity: Array.from(specialtyDemand.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([specialty, demand]) => ({specialty, appointments: demand}))
  };
}

console.log(analyzeAppointments(appointments));
```

### 15. Fitness Tracker Data Analysis
**Scenario**: Health app analyzing user workout patterns
```javascript
// Real-world data
const workoutData = [
  {userId: "U001", activity: "running", duration: 30, calories: 300, date: "2024-01-15"},
  {userId: "U002", activity: "cycling", duration: 45, calories: 400, date: "2024-01-15"},
  {userId: "U001", activity: "swimming", duration: 60, calories: 500, date: "2024-01-16"},
  {userId: "U003", activity: "running", duration: 25, calories: 250, date: "2024-01-16"},
  {userId: "U001", activity: "running", duration: 35, calories: 350, date: "2024-01-17"}
];

// Solution
function analyzeFitnessData(workouts) {
  const userStats = new Map();
  const activityStats = new Map();
  
  workouts.forEach(workout => {
    // User statistics
    if (!userStats.has(workout.userId)) {
      userStats.set(workout.userId, {
        totalWorkouts: 0,
        totalDuration: 0,
        totalCalories: 0,
        activities: new Set(),
        workoutDays: new Set()
      });
    }
    
    const user = userStats.get(workout.userId);
    user.totalWorkouts++;
    user.totalDuration += workout.duration;
    user.totalCalories += workout.calories;
    user.activities.add(workout.activity);
    user.workoutDays.add(workout.date);
    
    // Activity statistics
    if (!activityStats.has(workout.activity)) {
      activityStats.set(workout.activity, {
        totalSessions: 0,
        totalDuration: 0,
        totalCalories: 0,
        participants: new Set()
      });
    }
    
    const activity = activityStats.get(workout.activity);
    activity.totalSessions++;
    activity.totalDuration += workout.duration;
    activity.totalCalories += workout.calories;
    activity.participants.add(workout.userId);
  });
  
  return {
    userProfiles: Array.from(userStats.entries()).map(([userId, stats]) => ({
      userId,
      totalWorkouts: stats.totalWorkouts,
      avgDuration: Math.round(stats.totalDuration / stats.totalWorkouts),
      avgCalories: Math.round(stats.totalCalories / stats.totalWorkouts),
      activeDays: stats.workoutDays.size,
      varietyScore: stats.activities.size,
      activities: Array.from(stats.activities)
    })),
    
    activityInsights: Array.from(activityStats.entries()).map(([activity, stats]) => ({
      activity,
      popularity: stats.totalSessions,
      avgDuration: Math.round(stats.totalDuration / stats.totalSessions),
      caloriesPerMinute: (stats.totalCalories / stats.totalDuration).toFixed(1),
      uniqueParticipants: stats.participants.size
    })).sort((a, b) => b.popularity - a.popularity)
  };
}

console.log(analyzeFitnessData(workoutData));
```

### 16. Customer Support Ticket Analysis
**Scenario**: Help desk analyzing support ticket patterns and resolution times
```javascript
// Real-world data
const supportTickets = [
  {ticketId: "T001", userId: "U001", category: "Technical", priority: "High", status: "Resolved", createDate: "2024-01-15", resolveDate: "2024-01-16"},
  {ticketId: "T002", userId: "U002", category: "Billing", priority: "Medium", status: "Open", createDate: "2024-01-16", resolveDate: null},
  {ticketId: "T003", userId: "U001", category: "Technical", priority: "Low", status: "Resolved", createDate: "2024-01-17", resolveDate: "2024-01-20"},
  {ticketId: "T004", userId: "U003", category: "General", priority: "Medium", status: "In Progress", createDate: "2024-01-18", resolveDate: null}
];

// Solution
function analyzeSupportTickets(tickets) {
  const categoryStats = new Map();
  const userTickets = new Map();
  const priorityDistribution = new Map();
  
  tickets.forEach(ticket => {
    // Category analysis
    if (!categoryStats.has(ticket.category)) {
      categoryStats.set(ticket.category, {
        total: 0,
        resolved: 0,
        open: 0,
        inProgress: 0,
        resolutionTimes: []
      });
    }
    
    const category = categoryStats.get(ticket.category);
    category.total++;
    category[ticket.status.toLowerCase().replace(' ', '')]++;
    
    if (ticket.resolveDate) {
      const createTime = new Date(ticket.createDate);
      const resolveTime = new Date(ticket.resolveDate);
      const resolutionDays = Math.ceil((resolveTime - createTime) / (1000 * 60 * 60 * 24));
      category.resolutionTimes.push(resolutionDays);
    }
    
    // User ticket count
    userTickets.set(ticket.userId, (userTickets.get(ticket.userId) || 0) + 1);
    
    // Priority distribution
    priorityDistribution.set(ticket.priority, (priorityDistribution.get(ticket.priority) || 0) + 1);
  });
  
  return {
    categoryAnalysis: Array.from(categoryStats.entries()).map(([category, stats]) => ({
      category,
      totalTickets: stats.total,
      resolutionRate: ((stats.resolved / stats.total) * 100).toFixed(1),
      avgResolutionTime: stats.resolutionTimes.length > 0 
        ? (stats.resolutionTimes.reduce((sum, time) => sum + time, 0) / stats.resolutionTimes.length).toFixed(1)
        : 'N/A',
      statusBreakdown: {
        resolved: stats.resolved,
        open: stats.open,
        inProgress: stats.inprogress || 0
      }
    })),
    
    frequentUsers: Array.from(userTickets.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([userId, count]) => ({userId, ticketCount: count})),
    
    priorityBreakdown: Object.fromEntries(priorityDistribution)
  };
}

console.log(analyzeSupportTickets(supportTickets));
```

### 17. Streaming Platform Content Analysis
**Scenario**: Video streaming service analyzing viewing patterns
```javascript
// Real-world data
const viewingHistory = [
  {userId: "U001", contentId: "C001", title: "Action Movie", genre: "Action", duration: 120, watchTime: 120, date: "2024-01-15"},
  {userId: "U002", contentId: "C002", title: "Comedy Show", genre: "Comedy", duration: 30, watchTime: 25, date: "2024-01-15"},
  {userId: "U001", contentId: "C003", title: "Drama Series", genre: "Drama", duration: 45, watchTime: 45, date: "2024-01-16"},
  {userId: "U003", contentId: "C001", title: "Action Movie", genre: "Action", duration: 120, watchTime: 60, date: "2024-01-16"},
  {userId: "U002", contentId: "C001", title: "Action Movie", genre: "Action", duration: 120, watchTime: 120, date: "2024-01-17"}
];

// Solution
function analyzeViewingPatterns(viewingData) {
  const contentStats = new Map();
  const userPreferences = new Map();
  const genrePopularity = new Map();
  
  viewingData.forEach(view => {
    const completionRate = (view.watchTime / view.duration) * 100;
    
    // Content analysis
    if (!contentStats.has(view.contentId)) {
      contentStats.set(view.contentId, {
        title: view.title,
        genre: view.genre,
        totalViews: 0,
        totalWatchTime: 0,
        completions: 0,
        viewers: new Set()
      });
    }
    
    const content = contentStats.get(view.contentId);
    content.totalViews++;
    content.totalWatchTime += view.watchTime;
    content.viewers.add(view.userId);
    if (completionRate >= 90) content.completions++;
    
    // User preferences
    if (!userPreferences.has(view.userId)) {
      userPreferences.set(view.userId, {
        totalWatchTime: 0,
        contentWatched: 0,
        genres: new Map(),
        avgCompletionRate: []
      });
    }
    
    const user = userPreferences.get(view.userId);
    user.totalWatchTime += view.watchTime;
    user.contentWatched++;
    user.avgCompletionRate.push(completionRate);
    
    const currentGenreCount = user.genres.get(view.genre) || 0;
    user.genres.set(view.genre, currentGenreCount + 1);
    
    // Genre popularity
    genrePopularity.set(view.genre, (genrePopularity.get(view.genre) || 0) + view.watchTime);
  });
  
  return {
    contentPerformance: Array.from(contentStats.entries()).map(([contentId, stats]) => ({
      contentId,
      title: stats.title,
      genre: stats.genre,
      views: stats.totalViews,
      uniqueViewers: stats.viewers.size,
      completionRate: ((stats.completions / stats.totalViews) * 100).toFixed(1),
      avgWatchTime: Math.round(stats.totalWatchTime / stats.totalViews)
    })).sort((a, b) => b.views - a.views),
    
    userInsights: Array.from(userPreferences.entries()).map(([userId, prefs]) => ({
      userId,
      totalWatchTime: prefs.totalWatchTime,
      contentWatched: prefs.contentWatched,
      avgCompletionRate: (prefs.avgCompletionRate.reduce((sum, rate) => sum + rate, 0) / prefs.avgCompletionRate.length).toFixed(1),
      favoriteGenre: Array.from(prefs.genres.entries()).sort((a, b) => b[1] - a[1])[0]?.[0] || 'None'
    })),
    
    genreRankings: Array.from(genrePopularity.entries())
      .sort((a, b) => b[1] - a[1])
      .map(([genre, totalWatchTime]) => ({genre, totalWatchTime}))
  };
}

console.log(analyzeViewingPatterns(viewingHistory));
```

### 18. Hotel Room Booking System
**Scenario**: Hotel managing room reservations and occupancy rates
```javascript
// Real-world data
const reservations = [
  {reservationId: "R001", guestId: "G001", roomType: "Deluxe", checkIn: "2024-01-15", checkOut: "2024-01-18", rate: 200},
  {reservationId: "R002", guestId: "G002", roomType: "Standard", checkIn: "2024-01-16", checkOut: "2024-01-19", rate: 150},
  {reservationId: "R003", guestId: "G001", roomType: "Suite", checkIn: "2024-01-20", checkOut: "2024-01-23", rate: 350},
  {reservationId: "R004", guestId: "G003", roomType: "Standard", checkIn: "2024-01-17", checkOut: "2024-01-20", rate: 150}
];

// Solution
function analyzeHotelBookings(reservations) {
  const roomTypeStats = new Map();
  const guestHistory = new Map();
  const revenueByDate = new Map();
  
  reservations.forEach(reservation => {
    const nights = (new Date(reservation.checkOut) - new Date(reservation.checkIn)) / (1000 * 60 * 60 * 24);
    const totalRevenue = reservation.rate * nights;
    
    // Room type analysis
    if (!roomTypeStats.has(reservation.roomType)) {
      roomTypeStats.set(reservation.roomType, {
        bookings: 0,
        totalNights: 0,
        totalRevenue: 0,
        avgRate: 0
      });
    }
    
    const roomStats = roomTypeStats.get(reservation.roomType);
    roomStats.bookings++;
    roomStats.totalNights += nights;
    roomStats.totalRevenue += totalRevenue;
    roomStats.avgRate = roomStats.totalRevenue / roomStats.totalNights;
    
    // Guest history
    if (!guestHistory.has(reservation.guestId)) {
      guestHistory.set(reservation.guestId, {
        totalBookings: 0,
        totalSpent: 0,
        totalNights: 0,
        roomTypesStayed: new Set()
      });
    }
    
    const guest = guestHistory.get(reservation.guestId);
    guest.totalBookings++;
    guest.totalSpent += totalRevenue;
    guest.totalNights += nights;
    guest.roomTypesStayed.add(reservation.roomType);
  });
  
  return {
    roomTypePerformance: Array.from(roomTypeStats.entries()).map(([roomType, stats]) => ({
      roomType,
      bookings: stats.bookings,
      totalNights: stats.totalNights,
      revenue: stats.totalRevenue,
      avgNightlyRate: Math.round(stats.avgRate)
    })).sort((a, b) => b.revenue - a.revenue),
    
    guestProfiles: Array.from(guestHistory.entries()).map(([guestId, history]) => ({
      guestId,
      bookings: history.totalBookings,
      totalSpent: history.totalSpent,
      avgStayLength: Math.round(history.totalNights / history.totalBookings),
      loyaltyLevel: history.totalSpent > 1000 ? 'VIP' : history.totalSpent > 500 ? 'Gold' : 'Standard',
      roomPreference: Array.from(history.roomTypesStayed)
    })).sort((a, b) => b.totalSpent - a.totalSpent)
  };
}

console.log(analyzeHotelBookings(reservations));
```

### 19. Online Learning Platform Progress Tracking
**Scenario**: EdTech platform tracking student learning progress and course completion
```javascript
// Real-world data
const learningProgress = [
  {studentId: "S001", courseId: "C001", lessonId: "L001", completed: true, timeSpent: 45, score: 85},
  {studentId: "S001", courseId: "C001", lessonId: "L002", completed: true, timeSpent: 60, score: 92},
  {studentId: "S002", courseId: "C001", lessonId: "L001", completed: true, timeSpent: 30, score: 78},
  {studentId: "S002", courseId: "C002", lessonId: "L001", completed: false, timeSpent: 15, score: null},
  {studentId: "S001", courseId: "C002", lessonId: "L001", completed: true, timeSpent: 50, score: 88}
];

// Solution
function trackLearningProgress(progressData) {
  const studentProgress = new Map();
  const courseStats = new Map();
  const lessonPerformance = new Map();
  
  progressData.forEach(record => {
    // Student progress tracking
    if (!studentProgress.has(record.studentId)) {
      studentProgress.set(record.studentId, {
        coursesEnrolled: new Set(),
        lessonsCompleted: 0,
        totalTimeSpent: 0,
        scores: [],
        courseProgress: new Map()
      });
    }
    
    const student = studentProgress.get(record.studentId);
    student.coursesEnrolled.add(record.courseId);
    student.totalTimeSpent += record.timeSpent;
    
    if (record.completed) {
      student.lessonsCompleted++;
      if (record.score) student.scores.push(record.score);
    }
    
    // Course-specific progress
    if (!student.courseProgress.has(record.courseId)) {
      student.courseProgress.set(record.courseId, {
        lessonsCompleted: 0,
        totalLessons: 0,
        avgScore: 0
      });
    }
    
    const courseProgress = student.courseProgress.get(record.courseId);
    courseProgress.totalLessons++;
    if (record.completed) {
      courseProgress.lessonsCompleted++;
    }
    
    // Course statistics
    if (!courseStats.has(record.courseId)) {
      courseStats.set(record.courseId, {
        enrolledStudents: new Set(),
        totalLessons: new Set(),
        completedLessons: 0,
        avgTimePerLesson: []
      });
    }
    
    const course = courseStats.get(record.courseId);
    course.enrolledStudents.add(record.studentId);
    course.totalLessons.add(record.lessonId);
    course.avgTimePerLesson.push(record.timeSpent);
    if (record.completed) course.completedLessons++;
  });
  
  return {
    studentAnalytics: Array.from(studentProgress.entries()).map(([studentId, data]) => ({
      studentId,
      coursesEnrolled: data.coursesEnrolled.size,
      lessonsCompleted: data.lessonsCompleted,
      totalStudyTime: data.totalTimeSpent,
      avgScore: data.scores.length > 0 ? Math.round(data.scores.reduce((sum, score) => sum + score, 0) / data.scores.length) : 'N/A',
      engagementLevel: data.totalTimeSpent > 200 ? 'High' : data.totalTimeSpent > 100 ? 'Medium' : 'Low'
    })),
    
    courseInsights: Array.from(courseStats.entries()).map(([courseId, stats]) => ({
      courseId,
      enrolledStudents: stats.enrolledStudents.size,
      totalLessons: stats.totalLessons.size,
      completionRate: ((stats.completedLessons / (stats.enrolledStudents.size * stats.totalLessons.size)) * 100).toFixed(1),
      avgTimePerLesson: Math.round(stats.avgTimePerLesson.reduce((sum, time) => sum + time, 0) / stats.avgTimePerLesson.length)
    }))
  };
}

console.log(trackLearningProgress(learningProgress));
```

### 20. Supply Chain Inventory Optimization
**Scenario**: Manufacturing company optimizing inventory levels across warehouses
```javascript
// Real-world data
const inventoryMovements = [
  {productId: "P001", warehouseId: "W001", type: "stock_in", quantity: 500, date: "2024-01-15"},
  {productId: "P001", warehouseId: "W001", type: "stock_out", quantity: 100, date: "2024-01-16"},
  {productId: "P002", warehouseId: "W002", type: "stock_in", quantity: 200, date: "2024-01-15"},
  {productId: "P001", warehouseId: "W002", type: "stock_in", quantity: 300, date: "2024-01-17"},
  {productId: "P002", warehouseId: "W001", type: "stock_out", quantity: 50, date: "2024-01-18"}
];

// Solution
function optimizeInventory(movements) {
  const warehouseInventory = new Map();
  const productDistribution = new Map();
  const movementHistory = new Map();
  
  movements.forEach(movement => {
    // Warehouse inventory tracking
    if (!warehouseInventory.has(movement.warehouseId)) {
      warehouseInventory.set(movement.warehouseId, {
        products: new Map(),
        totalValue: 0,
        lastActivity: null
      });
    }
    
    const warehouse = warehouseInventory.get(movement.warehouseId);
    
    if (!warehouse.products.has(movement.productId)) {
      warehouse.products.set(movement.productId, {
        currentStock: 0,
        stockIn: 0,
        stockOut: 0
      });
    }
    
    const product = warehouse.products.get(movement.productId);
    
    if (movement.type === 'stock_in') {
      product.currentStock += movement.quantity;
      product.stockIn += movement.quantity;
    } else {
      product.currentStock -= movement.quantity;
      product.stockOut += movement.quantity;
    }
    
    warehouse.lastActivity = movement.date;
    
    // Product distribution across warehouses
    if (!productDistribution.has(movement.productId)) {
      productDistribution.set(movement.productId, {
        warehouses: new Set(),
        totalStock: 0,
        totalMovements: 0
      });
    }
    
    const productDist = productDistribution.get(movement.productId);
    productDist.warehouses.add(movement.warehouseId);
    productDist.totalMovements++;
    
    if (movement.type === 'stock_in') {
      productDist.totalStock += movement.quantity;
    } else {
      productDist.totalStock -= movement.quantity;
    }
  });
  
  return {
    warehouseStatus: Array.from(warehouseInventory.entries()).map(([warehouseId, data]) => ({
      warehouseId,
      productCount: data.products.size,
      inventory: Array.from(data.products.entries()).map(([productId, stock]) => ({
        productId,
        currentStock: stock.currentStock,
        turnoverRate: stock.stockOut > 0 ? (stock.stockOut / (stock.stockIn || 1)).toFixed(2) : '0.00',
        status: stock.currentStock < 50 ? 'Low Stock' : stock.currentStock > 400 ? 'Overstocked' : 'Normal'
      })),
      lastActivity: data.lastActivity
    })),
    
    productAnalysis: Array.from(productDistribution.entries()).map(([productId, data]) => ({
      productId,
      warehouseCount: data.warehouses.size,
      totalStock: data.totalStock,
      distribution: data.totalStock > 0 ? 'Well Distributed' : 'Out of Stock',
      recommendedAction: data.totalStock < 100 ? 'Reorder' : data.totalStock > 800 ? 'Reduce Orders' : 'Maintain'
    }))
  };
}

console.log(optimizeInventory(inventoryMovements));
```

---

## Sorting Questions

### 1. Sort Employee Records by Multiple Criteria
**Scenario**: HR system sorting employees by department, then salary, then hire date
```javascript
// Real-world data
const employees = [
  {id: "E001", name: "John Doe", department: "Engineering", salary: 75000, hireDate: "2022-01-15"},
  {id: "E002", name: "Jane Smith", department: "Marketing", salary: 65000, hireDate: "2021-03-20"},
  {id: "E003", name: "Bob Wilson", department: "Engineering", salary: 80000, hireDate: "2021-07-10"},
  {id: "E004", name: "Alice Brown", department: "Marketing", salary: 70000, hireDate: "2022-05-01"},
  {id: "E005", name: "Charlie Davis", department: "Engineering", salary: 75000, hireDate: "2021-12-01"}
];

// Solution
function sortEmployees(employees) {
  return employees.sort((a, b) => {
    // First by department
    if (a.department !== b.department) {
      return a.department.localeCompare(b.department);
    }
    
    // Then by salary (descending)
    if (a.salary !== b.salary) {
      return b.salary - a.salary;
    }
    
    // Finally by hire date (earliest first)
    return new Date(a.hireDate) - new Date(b.hireDate);
  });
}

console.log(sortEmployees(employees));
// Output: Sorted by department, then salary desc, then hire date asc
```

### 2. E-commerce Product Ranking Algorithm
**Scenario**: Online store sorting products by relevance score, ratings, and price
```javascript
// Real-world data
const products = [
  {id: "P001", name: "iPhone 14", category: "Electronics", price: 999, rating: 4.5, relevanceScore: 0.9, reviews: 1200},
  {id: "P002", name: "Samsung Galaxy", category: "Electronics", price: 899, rating: 4.3, relevanceScore: 0.8, reviews: 800},
  {id: "P003", name: "MacBook Pro", category: "Electronics", price: 1999, rating: 4.7, relevanceScore: 0.95, reviews: 600},
  {id: "P004", name: "iPad Air", category: "Electronics", price: 599, rating: 4.4, relevanceScore: 0.85, reviews: 900}
];

// Solution
function rankProducts(products, sortBy = "relevance") {
  const sortingStrategies = {
    relevance: (a, b) => {
      // Primary: relevance score
      if (Math.abs(a.relevanceScore - b.relevanceScore) > 0.1) {
        return b.relevanceScore - a.relevanceScore;
      }
      // Secondary: rating
      if (Math.abs(a.rating - b.rating) > 0.2) {
        return b.rating - a.rating;
      }
      // Tertiary: review count
      return b.reviews - a.reviews;
    },
    
    price_low_high: (a, b) => a.price - b.price,
    price_high_low: (a, b) => b.price - a.price,
    
    rating: (a, b) => {
      if (Math.abs(a.rating - b.rating) > 0.1) {
        return b.rating - a.rating;
      }
      return b.reviews - a.reviews;
    },
    
    popularity: (a, b) => b.reviews - a.reviews
  };
  
  return products.sort(sortingStrategies[sortBy] || sortingStrategies.relevance);
}

console.log(rankProducts(products, "relevance"));
```

### 3. Meeting Room Schedule Optimization
**Scenario**: Office scheduling system organizing meetings by time and priority
```javascript
// Real-world data
const meetings = [
  {id: "M001", title: "Team Standup", startTime: "09:00", endTime: "09:30", priority: "High", attendees: 5},
  {id: "M002", title: "Client Presentation", startTime: "10:00", endTime: "11:00", priority: "Critical", attendees: 8},
  {id: "M003", title: "Code Review", startTime: "09:00", endTime: "10:00", priority: "Medium", attendees: 3},
  {id: "M004", title: "All Hands", startTime: "14:00", endTime: "15:00", priority: "High", attendees: 50},
  {id: "M005", title: "1:1 Meeting", startTime: "11:00", endTime: "11:30", priority: "Low", attendees: 2}
];

// Solution
function scheduleMeetings(meetings) {
  const priorityWeight = {"Critical": 4, "High": 3, "Medium": 2, "Low": 1};
  
  // Sort by start time, then by priority, then by attendee count
  return meetings.sort((a, b) => {
    // Primary: start time
    if (a.startTime !== b.startTime) {
      return a.startTime.localeCompare(b.startTime);
    }
    
    // Secondary: priority
    const priorityDiff = priorityWeight[b.priority] - priorityWeight[a.priority];
    if (priorityDiff !== 0) {
      return priorityDiff;
    }
    
    // Tertiary: attendee count (more attendees = higher priority)
    return b.attendees - a.attendees;
  });
}

// Also find conflicting meetings
function findConflicts(meetings) {
  const sorted = scheduleMeetings([...meetings]);
  const conflicts = [];
  
  for (let i = 0; i < sorted.length - 1; i++) {
    const current = sorted[i];
    const next = sorted[i + 1];
    
    if (current.startTime === next.startTime || 
        (current.endTime > next.startTime && current.startTime < next.endTime)) {
      conflicts.push({meeting1: current.id, meeting2: next.id, type: "Time Conflict"});
    }
  }
  
  return {scheduledMeetings: sorted, conflicts};
}

console.log(findConflicts(meetings));
```

### 4. Student Grade Ranking System
**Scenario**: School ranking students by GPA, then by extracurricular activities
```javascript
// Real-world data
const students = [
  {id: "S001", name: "Alice Johnson", gpa: 3.8, extracurriculars: 5, graduationYear: 2024, major: "Computer Science"},
  {id: "S002", name: "Bob Smith", gpa: 3.9, extracurriculars: 3, graduationYear: 2024, major: "Engineering"},
  {id: "S003", name: "Carol Davis", gpa: 3.8, extracurriculars: 7, graduationYear: 2025, major: "Computer Science"},
  {id: "S004", name: "David Wilson", gpa: 4.0, extracurriculars: 4, graduationYear: 2024, major: "Mathematics"},
  {id: "S005", name: "Eva Brown", gpa: 3.7, extracurriculars: 6, graduationYear: 2024, major: "Engineering"}
];

// Solution
function rankStudentsForScholarship(students) {
  return students.sort((a, b) => {
    // Primary: GPA (higher is better)
    if (Math.abs(a.gpa - b.gpa) > 0.05) {
      return b.gpa - a.gpa;
    }
    
    // Secondary: Extracurricular activities (more is better)
    if (a.extracurriculars !== b.extracurriculars) {
      return b.extracurriculars - a.extracurriculars;
    }
    
    // Tertiary: Graduation year (current year gets priority)
    if (a.graduationYear !== b.graduationYear) {
      return a.graduationYear - b.graduationYear;
    }
    
    // Final: Alphabetical by name
    return a.name.localeCompare(b.name);
  }).map((student, index) => ({
    rank: index + 1,
    ...student,
    scholarshipEligibility: index < 3 ? "Eligible" : "Not Eligible"
  }));
}

console.log(rankStudentsForScholarship(students));
```

### 5. Restaurant Order Queue Management
**Scenario**: Restaurant kitchen prioritizing orders by time, special requests, and customer tier
```javascript
// Real-world data
const orders = [
  {orderId: "O001", customerTier: "VIP", items: 3, specialRequests: 1, orderTime: "12:00", estimatedTime: 15},
  {orderId: "O002", customerTier: "Regular", items: 2, specialRequests: 0, orderTime: "12:02", estimatedTime: 10},
  {orderId: "O003", customerTier: "Premium", items: 4, specialRequests: 2, orderTime: "12:01", estimatedTime: 20},
  {orderId: "O004", customerTier: "Regular", items: 1, specialRequests: 0, orderTime: "11:58", estimatedTime: 5},
  {orderId: "O005", customerTier: "VIP", items: 2, specialRequests: 0, orderTime: "12:03", estimatedTime: 12}
];

// Solution
function prioritizeKitchenOrders(orders) {
  const tierPriority = {"VIP": 3, "Premium": 2, "Regular": 1};
  const currentTime = new Date(`2024-01-15T12:05:00`); // 12:05 PM
  
  return orders.sort((a, b) => {
    // Calculate wait time
    const waitTimeA = (currentTime - new Date(`2024-01-15T${a.orderTime}:00`)) / (1000 * 60);
    const waitTimeB = (currentTime - new Date(`2024-01-15T${b.orderTime}:00`)) / (1000 * 60);
    
    // Primary: Customer tier
    const tierDiff = tierPriority[b.customerTier] - tierPriority[a.customerTier];
    if (tierDiff !== 0) {
      return tierDiff;
    }
    
    // Secondary: Wait time (longer wait = higher priority)
    if (Math.abs(waitTimeA - waitTimeB) > 2) { // 2 minute threshold
      return waitTimeB - waitTimeA;
    }
    
    // Tertiary: Fewer special requests get priority (easier to prepare)
    if (a.specialRequests !== b.specialRequests) {
      return a.specialRequests - b.specialRequests;
    }
    
    // Final: Shorter estimated time gets priority
    return a.estimatedTime - b.estimatedTime;
  }).map((order, index) => ({
    queuePosition: index + 1,
    ...order,
    waitTime: Math.round((currentTime - new Date(`2024-01-15T${order.orderTime}:00`)) / (1000 * 60))
  }));
}

console.log(prioritizeKitchenOrders(orders));
```

### 6. E-commerce Inventory Sort by Demand and Stock
**Scenario**: Warehouse organizing products by sales velocity and stock levels
```javascript
// Real-world data
const inventory = [
  {productId: "P001", name: "Wireless Headphones", stock: 50, weeklySales: 25, category: "Electronics", lastRestocked: "2024-01-10"},
  {productId: "P002", name: "Phone Case", stock: 200, weeklySales: 15, category: "Accessories", lastRestocked: "2024-01-12"},
  {productId: "P003", name: "Laptop Stand", stock: 5, weeklySales: 20, category: "Accessories", lastRestocked: "2024-01-08"},
  {productId: "P004", name: "USB Cable", stock: 100, weeklySales: 30, category: "Accessories", lastRestocked: "2024-01-14"},
  {productId: "P005", name: "Bluetooth Speaker", stock: 15, weeklySales: 18, category: "Electronics", lastRestocked: "2024-01-09"}
];

// Solution
function sortInventoryByPriority(inventory) {
  return inventory.map(item => {
    // Calculate key metrics
    const daysOfStock = Math.floor(item.stock / (item.weeklySales / 7));
    const velocityScore = item.weeklySales / item.stock; // Higher = faster moving
    const urgencyScore = daysOfStock < 7 ? 3 : daysOfStock < 14 ? 2 : 1;
    
    return {
      ...item,
      daysOfStock,
      velocityScore: parseFloat(velocityScore.toFixed(3)),
      urgencyScore,
      restockPriority: urgencyScore * velocityScore
    };
  }).sort((a, b) => {
    // Primary: Restock priority (higher = more urgent)
    if (Math.abs(a.restockPriority - b.restockPriority) > 0.1) {
      return b.restockPriority - a.restockPriority;
    }
    
    // Secondary: Days of stock (fewer days = higher priority)
    if (Math.abs(a.daysOfStock - b.daysOfStock) > 2) {
      return a.daysOfStock - b.daysOfStock;
    }
    
    // Tertiary: Weekly sales (higher sales = higher priority)
    return b.weeklySales - a.weeklySales;
  }).map((item, index) => ({
    priority: index + 1,
    ...item,
    action: item.daysOfStock < 7 ? "URGENT RESTOCK" : 
            item.daysOfStock < 14 ? "Schedule Restock" : "Monitor"
  }));
}

console.log(sortInventoryByPriority(inventory));
```

### 7. Job Application Screening System
**Scenario**: HR system ranking job applications by qualifications and experience
```javascript
// Real-world data
const applications = [
  {id: "A001", name: "John Doe", experience: 5, education: "Masters", skills: ["JavaScript", "React", "Node.js"], salary: 80000, applicationDate: "2024-01-15"},
  {id: "A002", name: "Jane Smith", experience: 3, education: "Bachelors", skills: ["JavaScript", "Vue.js", "Python"], salary: 70000, applicationDate: "2024-01-14"},
  {id: "A003", name: "Bob Wilson", experience: 7, education: "Bachelors", skills: ["JavaScript", "React", "Python", "AWS"], salary: 90000, applicationDate: "2024-01-16"},
  {id: "A004", name: "Alice Brown", experience: 2, education: "Masters", skills: ["JavaScript", "Angular"], salary: 65000, applicationDate: "2024-01-13"}
];

// Solution
function screenApplications(applications, jobRequirements) {
  const requirements = jobRequirements || {
    minExperience: 3,
    preferredEducation: "Masters",
    requiredSkills: ["JavaScript", "React"],
    maxSalary: 85000
  };
  
  return applications.map(app => {
    // Calculate match scores
    const experienceScore = Math.min(app.experience / requirements.minExperience, 2); // Cap at 2x
    const educationScore = app.education === requirements.preferredEducation ? 1.2 : 
                          app.education === "Masters" ? 1.1 : 1.0;
    
    const skillMatches = requirements.requiredSkills.filter(skill => 
      app.skills.includes(skill)).length;
    const skillScore = skillMatches / requirements.requiredSkills.length;
    
    const salaryScore = app.salary <= requirements.maxSalary ? 1.0 : 
                       (requirements.maxSalary / app.salary);
    
    const totalScore = (experienceScore * 0.3 + educationScore * 0.2 + 
                       skillScore * 0.4 + salaryScore * 0.1);
    
    return {
      ...app,
      matchScore: parseFloat(totalScore.toFixed(2)),
      skillMatches,
      meetsRequirements: app.experience >= requirements.minExperience && 
                        skillMatches >= requirements.requiredSkills.length && 
                        app.salary <= requirements.maxSalary
    };
  }).sort((a, b) => {
    // Primary: Meets requirements
    if (a.meetsRequirements !== b.meetsRequirements) {
      return b.meetsRequirements - a.meetsRequirements;
    }
    
    // Secondary: Match score
    if (Math.abs(a.matchScore - b.matchScore) > 0.1) {
      return b.matchScore - a.matchScore;
    }
    
    // Tertiary: Application date (earlier is better)
    return new Date(a.applicationDate) - new Date(b.applicationDate);
  }).map((app, index) => ({
    ranking: index + 1,
    ...app,
    recommendation: index < 3 ? "Interview" : 
                   app.meetsRequirements ? "Consider" : "Reject"
  }));
}

console.log(screenApplications(applications));
```

### 8. Medical Patient Triage System
**Scenario**: Hospital emergency room prioritizing patients by severity and wait time
```javascript
// Real-world data
const patients = [
  {id: "P001", name: "John Doe", severity: "High", symptoms: "Chest pain", waitTime: 45, age: 65, insurance: "Premium"},
  {id: "P002", name: "Jane Smith", severity: "Medium", symptoms: "Broken arm", waitTime: 30, age: 28, insurance: "Basic"},
  {id: "P003", name: "Bob Wilson", severity: "Critical", symptoms: "Heart attack", waitTime: 5, age: 58, insurance: "Premium"},
  {id: "P004", name: "Alice Brown", severity: "Low", symptoms: "Minor cut", waitTime: 60, age: 35, insurance: "Basic"},
  {id: "P005", name: "Charlie Davis", severity: "High", symptoms: "Severe bleeding", waitTime: 20, age: 42, insurance: "Premium"}
];

// Solution
function triagePatients(patients) {
  const severityPriority = {"Critical": 4, "High": 3, "Medium": 2, "Low": 1};
  
  return patients.sort((a, b) => {
    // Primary: Severity level
    const severityDiff = severityPriority[b.severity] - severityPriority[a.severity];
    if (severityDiff !== 0) {
      return severityDiff;
    }
    
    // Secondary: Wait time (longer wait = higher priority within same severity)
    if (Math.abs(a.waitTime - b.waitTime) > 10) {
      return b.waitTime - a.waitTime;
    }
    
    // Tertiary: Age (elderly get slight priority)
    const agePriorityA = a.age >= 65 ? 1 : a.age <= 18 ? 1 : 0;
    const agePriorityB = b.age >= 65 ? 1 : b.age <= 18 ? 1 : 0;
    
    if (agePriorityA !== agePriorityB) {
      return agePriorityB - agePriorityA;
    }
    
    // Final: Insurance type (administrative priority)
    const insurancePriority = {"Premium": 2, "Basic": 1};
    return (insurancePriority[b.insurance] || 0) - (insurancePriority[a.insurance] || 0);
  }).map((patient, index) => ({
    triageOrder: index + 1,
    ...patient,
    estimatedWaitTime: index * 15, // 15 minutes per position
    priority: severityPriority[patient.severity] >= 3 ? "Immediate" : "Standard"
  }));
}

console.log(triagePatients(patients));
```

### 9. Social Media Content Feed Algorithm
**Scenario**: Social platform sorting posts by engagement, recency, and user relevance
```javascript
// Real-world data
const posts = [
  {id: "POST001", userId: "U001", content: "Just launched my new app!", likes: 150, comments: 25, shares: 12, timestamp: "2024-01-15T10:00:00Z", userFollowers: 1000},
  {id: "POST002", userId: "U002", content: "Beautiful sunset today", likes: 50, comments: 8, shares: 3, timestamp: "2024-01-15T14:00:00Z", userFollowers: 500},
  {id: "POST003", userId: "U003", content: "Breaking: Tech news update", likes: 300, comments: 75, shares: 45, timestamp: "2024-01-15T09:00:00Z", userFollowers: 5000},
  {id: "POST004", userId: "U004", content: "My morning coffee routine", likes: 25, comments: 5, shares: 1, timestamp: "2024-01-15T15:00:00Z", userFollowers: 200},
  {id: "POST005", userId: "U005", content: "Coding tips for beginners", likes: 200, comments: 40, shares: 30, timestamp: "2024-01-15T12:00:00Z", userFollowers: 2000}
];

// Solution
function generateFeed(posts, currentUserId = "current_user") {
  const now = new Date("2024-01-15T16:00:00Z");
  
  return posts.map(post => {
    // Calculate engagement metrics
    const totalEngagement = post.likes + (post.comments * 2) + (post.shares * 3);
    const engagementRate = totalEngagement / post.userFollowers;
    
    // Calculate recency score (newer posts get higher score)
    const hoursOld = (now - new Date(post.timestamp)) / (1000 * 60 * 60);
    const recencyScore = Math.max(0, 24 - hoursOld) / 24; // Score from 0-1
    
    // User influence score
    const influenceScore = Math.log10(post.userFollowers + 1) / 4; // Normalized log scale
    
    // Combined feed score
    const feedScore = (engagementRate * 0.4) + (recencyScore * 0.3) + (influenceScore * 0.3);
    
    return {
      ...post,
      engagementRate: parseFloat(engagementRate.toFixed(3)),
      recencyScore: parseFloat(recencyScore.toFixed(2)),
      hoursOld: Math.round(hoursOld),
      feedScore: parseFloat(feedScore.toFixed(3))
    };
  }).sort((a, b) => {
    // Primary: Feed score
    if (Math.abs(a.feedScore - b.feedScore) > 0.1) {
      return b.feedScore - a.feedScore;
    }
    
    // Secondary: Recency (if scores are close, prefer newer)
    return b.recencyScore - a.recencyScore;
  }).map((post, index) => ({
    feedPosition: index + 1,
    ...post,
    visibility: index < 5 ? "High" : index < 10 ? "Medium" : "Low"
  }));
}

console.log(generateFeed(posts));
```

### 10. Flight Booking Search Results
**Scenario**: Travel website sorting flight options by price, duration, and departure time
```javascript
// Real-world data
const flights = [
  {flightId: "FL001", airline: "Delta", price: 450, duration: 180, departure: "08:00", arrival: "11:00", stops: 0, aircraft: "Boeing 737"},
  {flightId: "FL002", airline: "United", price: 380, duration: 240, departure: "06:30", arrival: "10:30", stops: 1, aircraft: "Airbus A320"},
  {flightId: "FL003", airline: "Southwest", price: 320, duration: 200, departure: "14:00", arrival: "17:20", stops: 0, aircraft: "Boeing 737"},
  {flightId: "FL004", airline: "American", price: 520, duration: 160, departure: "19:00", arrival: "21:40", stops: 0, aircraft: "Boeing 787"},
  {flightId: "FL005", airline: "JetBlue", price: 400, duration: 300, departure: "12:00", arrival: "17:00", stops: 2, aircraft: "Embraer 190"}
];

// Solution
function sortFlightResults(flights, sortPreference = "best_value") {
  const sortingStrategies = {
    best_value: (a, b) => {
      // Value score: considers price, duration, and convenience
      const valueScoreA = (1000 - a.price) * 0.4 + (300 - a.duration) * 0.3 + (3 - a.stops) * 100 * 0.3;
      const valueScoreB = (1000 - b.price) * 0.4 + (300 - b.duration) * 0.3 + (3 - b.stops) * 100 * 0.3;
      return valueScoreB - valueScoreA;
    },
    
    price_low: (a, b) => {
      if (a.price !== b.price) return a.price - b.price;
      return a.duration - b.duration; // Secondary: shorter duration
    },
    
    duration_short: (a, b) => {
      if (a.duration !== b.duration) return a.duration - b.duration;
      return a.stops - b.stops; // Secondary: fewer stops
    },
    
    departure_early: (a, b) => {
      const timeA = parseInt(a.departure.replace(':', ''));
      const timeB = parseInt(b.departure.replace(':', ''));
      if (timeA !== timeB) return timeA - timeB;
      return a.price - b.price; // Secondary: lower price
    },
    
    non_stop_first: (a, b) => {
      if (a.stops !== b.stops) return a.stops - b.stops;
      return a.price - b.price; // Secondary: lower price
    }
  };
  
  return flights.sort(sortingStrategies[sortPreference] || sortingStrategies.best_value)
    .map((flight, index) => ({
      searchRank: index + 1,
      ...flight,
      priceCategory: flight.price < 350 ? "Budget" : flight.price < 450 ? "Standard" : "Premium",
      convenience: flight.stops === 0 ? "Non-stop" : `${flight.stops} stop${flight.stops > 1 ? 's' : ''}`,
      departureCategory: (() => {
        const hour = parseInt(flight.departure.split(':')[0]);
        if (hour < 6) return "Red-eye";
        if (hour < 12) return "Morning";
        if (hour < 18) return "Afternoon";
        return "Evening";
      })()
    }));
}

console.log(sortFlightResults(flights, "best_value"));
```

### 11. News Article Relevance Ranking
**Scenario**: News aggregator sorting articles by relevance, freshness, and source credibility
```javascript
// Real-world data
const articles = [
  {id: "ART001", title: "Breaking: New Tech Breakthrough", source: "TechCrunch", publishTime: "2024-01-15T14:00:00Z", views: 15000, category: "Technology", credibilityScore: 0.9},
  {id: "ART002", title: "Market Analysis: Stocks Rise", source: "Reuters", publishTime: "2024-01-15T12:00:00Z", views: 8000, category: "Finance", credibilityScore: 0.95},
  {id: "ART003", title: "Sports Update: Championship Game", source: "ESPN", publishTime: "2024-01-15T16:00:00Z", views: 25000, category: "Sports", credibilityScore: 0.85},
  {id: "ART004", title: "Health Tips for Winter", source: "HealthLine", publishTime: "2024-01-15T10:00:00Z", views: 5000, category: "Health", credibilityScore: 0.8},
  {id: "ART005", title: "Climate Change Report Released", source: "BBC", publishTime: "2024-01-15T15:00:00Z", views: 12000, category: "Environment", credibilityScore: 0.92}
];

// Solution
function rankNewsArticles(articles, userPreferences = {}) {
  const now = new Date("2024-01-15T17:00:00Z");
  const preferences = {
    preferredCategories: ["Technology", "Finance"],
    sourceCredibilityWeight: 0.3,
    freshnessWeight: 0.3,
    popularityWeight: 0.2,
    relevanceWeight: 0.2,
    ...userPreferences
  };
  
  return articles.map(article => {
    // Freshness score (newer = better)
    const hoursOld = (now - new Date(article.publishTime)) / (1000 * 60 * 60);
    const freshnessScore = Math.max(0, (24 - hoursOld) / 24);
    
    // Popularity score (normalized by log scale)
    const popularityScore = Math.log10(article.views + 1) / 5; // Normalize to 0-1
    
    // Relevance score (based on user preferences)
    const relevanceScore = preferences.preferredCategories.includes(article.category) ? 1 : 0.5;
    
    // Combined ranking score
    const rankingScore = (
      article.credibilityScore * preferences.sourceCredibilityWeight +
      freshnessScore * preferences.freshnessWeight +
      popularityScore * preferences.popularityWeight +
      relevanceScore * preferences.relevanceWeight
    );
    
    return {
      ...article,
      hoursOld: Math.round(hoursOld),
      freshnessScore: parseFloat(freshnessScore.toFixed(2)),
      popularityScore: parseFloat(popularityScore.toFixed(2)),
      relevanceScore,
      rankingScore: parseFloat(rankingScore.toFixed(3))
    };
  }).sort((a, b) => {
    // Primary: Ranking score
    if (Math.abs(a.rankingScore - b.rankingScore) > 0.05) {
      return b.rankingScore - a.rankingScore;
    }
    
    // Secondary: Views (more popular)
    return b.views - a.views;
  }).map((article, index) => ({
    newsRank: index + 1,
    ...article,
    recommendation: index < 3 ? "Top Story" : index < 8 ? "Featured" : "Standard"
  }));
}

console.log(rankNewsArticles(articles));
```

### 12. Real Estate Property Listing Sort
**Scenario**: Real estate website sorting properties by user criteria and market factors
```javascript
// Real-world data
const properties = [
  {id: "PROP001", address: "123 Oak St", price: 450000, bedrooms: 3, bathrooms: 2, sqft: 1800, yearBuilt: 2015, daysOnMarket: 45, neighborhood: "Downtown"},
  {id: "PROP002", address: "456 Pine Ave", price: 320000, bedrooms: 2, bathrooms: 1, sqft: 1200, yearBuilt: 1995, daysOnMarket: 12, neighborhood: "Suburbs"},
  {id: "PROP003", address: "789 Elm Dr", price: 650000, bedrooms: 4, bathrooms: 3, sqft: 2500, yearBuilt: 2020, daysOnMarket: 8, neighborhood: "Uptown"},
  {id: "PROP004", address: "321 Maple Ln", price: 380000, bedrooms: 3, bathrooms: 2, sqft: 1600, yearBuilt: 2010, daysOnMarket: 60, neighborhood: "Midtown"},
  {id: "PROP005", address: "654 Cedar Ct", price: 280000, bedrooms: 2, bathrooms: 1, sqft: 1000, yearBuilt: 1985, daysOnMarket: 90, neighborhood: "Suburbs"}
];

// Solution
function sortProperties(properties, criteria = {}) {
  const searchCriteria = {
    maxPrice: 500000,
    minBedrooms: 2,
    minBathrooms: 1,
    sortBy: "value", // value, price, size, newest, market_activity
    ...criteria
  };
  
  // Filter properties first
  const filtered = properties.filter(prop => 
    prop.price <= searchCriteria.maxPrice &&
    prop.bedrooms >= searchCriteria.minBedrooms &&
    prop.bathrooms >= searchCriteria.minBathrooms
  );
  
  return filtered.map(property => {
    // Calculate value metrics
    const pricePerSqft = property.price / property.sqft;
    const ageScore = Math.max(0, (2024 - property.yearBuilt)) / 40; // Newer is better
    const marketActivityScore = Math.max(0, (180 - property.daysOnMarket)) / 180; // Less time = more activity
    
    // Value score considers price efficiency and property condition
    const valueScore = (1000 - pricePerSqft) / 1000 + (1 - ageScore) * 0.3 + marketActivityScore * 0.2;
    
    return {
      ...property,
      pricePerSqft: Math.round(pricePerSqft),
      propertyAge: 2024 - property.yearBuilt,
      marketActivityScore: parseFloat(marketActivityScore.toFixed(2)),
      valueScore: parseFloat(valueScore.toFixed(3))
    };
  }).sort((a, b) => {
    switch (searchCriteria.sortBy) {
      case "price":
        return a.price - b.price;
      
      case "size":
        return b.sqft - a.sqft;
      
      case "newest":
        if (a.yearBuilt !== b.yearBuilt) return b.yearBuilt - a.yearBuilt;
        return a.price - b.price;
      
      case "market_activity":
        if (Math.abs(a.marketActivityScore - b.marketActivityScore) > 0.1) {
          return b.marketActivityScore - a.marketActivityScore;
        }
        return a.daysOnMarket - b.daysOnMarket;
      
      case "value":
      default:
        if (Math.abs(a.valueScore - b.valueScore) > 0.1) {
          return b.valueScore - a.valueScore;
        }
        return a.pricePerSqft - b.pricePerSqft;
    }
  }).map((property, index) => ({
    searchRank: index + 1,
    ...property,
    recommendation: (() => {
      if (property.valueScore > 0.8) return "Excellent Value";
      if (property.marketActivityScore > 0.7) return "Hot Property";
      if (property.daysOnMarket > 60) return "Price Negotiable";
      return "Standard Listing";
    })()
  }));
}

console.log(sortProperties(properties));
```

### 13. Customer Service Queue Management
**Scenario**: Call center prioritizing customer support tickets by urgency and customer tier
```javascript
// Real-world data
const supportRequests = [
  {ticketId: "T001", customerId: "C001", customerTier: "Platinum", issueType: "Technical", severity: "High", waitTime: 15, accountValue: 50000},
  {ticketId: "T002", customerId: "C002", customerTier: "Gold", issueType: "Billing", severity: "Medium", waitTime: 45, accountValue: 15000},
  {ticketId: "T003", customerId: "C003", customerTier: "Silver", issueType: "General", severity: "Low", waitTime: 60, accountValue: 5000},
  {ticketId: "T004", customerId: "C004", customerTier: "Platinum", issueType: "Service Outage", severity: "Critical", waitTime: 5, accountValue: 75000},
  {ticketId: "T005", customerId: "C005", customerTier: "Gold", issueType: "Feature Request", severity: "Low", waitTime: 30, accountValue: 20000}
];

// Solution
function prioritizeSupportQueue(requests) {
  const tierWeight = {"Platinum": 4, "Gold": 3, "Silver": 2, "Bronze": 1};
  const severityWeight = {"Critical": 5, "High": 4, "Medium": 3, "Low": 2, "Info": 1};
  const issueTypeWeight = {"Service Outage": 5, "Technical": 4, "Billing": 3, "General": 2, "Feature Request": 1};
  
  return requests.map(request => {
    // Calculate priority scores
    const tierScore = tierWeight[request.customerTier] || 1;
    const severityScore = severityWeight[request.severity] || 1;
    const issueScore = issueTypeWeight[request.issueType] || 1;
    const waitScore = Math.min(request.waitTime / 30, 2); // Cap at 2x for very long waits
    const valueScore = Math.log10(request.accountValue + 1) / 5; // Account value influence
    
    // Combined priority score
    const priorityScore = (severityScore * 0.35) + (tierScore * 0.25) + (issueScore * 0.2) + 
                         (waitScore * 0.15) + (valueScore * 0.05);
    
    return {
      ...request,
      priorityScore: parseFloat(priorityScore.toFixed(2)),
      estimatedResolutionTime: (() => {
        const baseTime = {"Critical": 15, "High": 30, "Medium": 60, "Low": 120}[request.severity] || 60;
        const tierMultiplier = {"Platinum": 0.8, "Gold": 0.9, "Silver": 1.0, "Bronze": 1.1}[request.customerTier] || 1.0;
        return Math.round(baseTime * tierMultiplier);
      })()
    };
  }).sort((a, b) => {
    // Primary: Priority score
    if (Math.abs(a.priorityScore - b.priorityScore) > 0.2) {
      return b.priorityScore - a.priorityScore;
    }
    
    // Secondary: Wait time (longer wait gets priority)
    if (Math.abs(a.waitTime - b.waitTime) > 10) {
      return b.waitTime - a.waitTime;
    }
    
    // Tertiary: Account value
    return b.accountValue - a.accountValue;
  }).map((request, index) => ({
    queuePosition: index + 1,
    ...request,
    slaStatus: request.waitTime > 60 ? "SLA Risk" : request.waitTime > 30 ? "Monitor" : "On Track",
    agentAssignment: index < 2 ? "Senior Agent" : index < 5 ? "Standard Agent" : "Junior Agent"
  }));
}

console.log(prioritizeSupportQueue(supportRequests));
```

### 14. Event Planning Task Prioritization
**Scenario**: Wedding planner organizing tasks by deadline, importance, and dependencies
```javascript
// Real-world data
const eventTasks = [
  {taskId: "T001", task: "Book venue", deadline: "2024-02-01", importance: "Critical", estimatedHours: 8, dependencies: [], status: "Not Started"},
  {taskId: "T002", task: "Send invitations", deadline: "2024-02-15", importance: "High", estimatedHours: 4, dependencies: ["T001"], status: "Not Started"},
  {taskId: "T003", task: "Order flowers", deadline: "2024-03-01", importance: "Medium", estimatedHours: 2, dependencies: ["T001"], status: "Not Started"},
  {taskId: "T004", task: "Hire photographer", deadline: "2024-02-10", importance: "High", estimatedHours: 6, dependencies: [], status: "In Progress"},
  {taskId: "T005", task: "Choose menu", deadline: "2024-02-20", importance: "High", estimatedHours: 3, dependencies: ["T001"], status: "Not Started"},
  {taskId: "T006", task: "Book DJ", deadline: "2024-02-25", importance: "Medium", estimatedHours: 2, dependencies: ["T001"], status: "Not Started"}
];

// Solution
function prioritizeEventTasks(tasks) {
  const today = new Date("2024-01-20");
  const importanceWeight = {"Critical": 5, "High": 4, "Medium": 3, "Low": 2, "Optional": 1};
  const statusWeight = {"Not Started": 1, "In Progress": 0.8, "Completed": 0};
  
  return tasks.map(task => {
    // Calculate time-based metrics
    const deadline = new Date(task.deadline);
    const daysUntilDeadline = Math.ceil((deadline - today) / (1000 * 60 * 60 * 24));
    const urgencyScore = Math.max(0, (30 - daysUntilDeadline) / 30); // More urgent as deadline approaches
    
    // Check if dependencies are blocking
    const hasBlockingDependencies = task.dependencies.length > 0;
    const dependencyScore = hasBlockingDependencies ? 0.7 : 1.0;
    
    // Calculate overall priority
    const importanceScore = importanceWeight[task.importance] || 1;
    const statusScore = statusWeight[task.status] || 1;
    
    const priorityScore = (importanceScore * 0.4) + (urgencyScore * 0.3) + 
                         (dependencyScore * 0.2) + (statusScore * 0.1);
    
    return {
      ...task,
      daysUntilDeadline,
      urgencyScore: parseFloat(urgencyScore.toFixed(2)),
      priorityScore: parseFloat(priorityScore.toFixed(2)),
      canStart: !hasBlockingDependencies || task.status === "In Progress"
    };
  }).sort((a, b) => {
    // Primary: Can the task be started (unblocked tasks first)
    if (a.canStart !== b.canStart) {
      return b.canStart - a.canStart;
    }
    
    // Secondary: Priority score
    if (Math.abs(a.priorityScore - b.priorityScore) > 0.1) {
      return b.priorityScore - a.priorityScore;
    }
    
    // Tertiary: Days until deadline (sooner deadlines first)
    return a.daysUntilDeadline - b.daysUntilDeadline;
  }).map((task, index) => ({
    planningOrder: index + 1,
    ...task,
    actionStatus: (() => {
      if (!task.canStart) return "Blocked - Wait for Dependencies";
      if (task.daysUntilDeadline < 7) return "URGENT - Start Immediately";
      if (task.daysUntilDeadline < 14) return "High Priority - Schedule Soon";
      return "Normal Priority";
    })(),
    recommendedStartDate: (() => {
      const startDate = new Date(task.deadline);
      startDate.setDate(startDate.getDate() - Math.ceil(task.estimatedHours / 8 * 7) - 3); // Buffer time
      return startDate.toISOString().split('T')[0];
    })()
  }));
}

console.log(prioritizeEventTasks(eventTasks));
```

### 15. University Course Schedule Optimization
**Scenario**: Academic system arranging class schedules by requirements and constraints
```javascript
// Real-world data
const courses = [
  {courseId: "CS101", courseName: "Intro to Programming", credits: 3, difficulty: "Beginner", timeSlot: "MWF 9:00", professor: "Dr. Smith", capacity: 30, enrolled: 28, prerequisites: []},
  {courseId: "CS201", courseName: "Data Structures", credits: 4, difficulty: "Intermediate", timeSlot: "TTH 10:00", professor: "Dr. Johnson", capacity: 25, enrolled: 20, prerequisites: ["CS101"]},
  {courseId: "MATH150", courseName: "Calculus I", credits: 4, difficulty: "Intermediate", timeSlot: "MWF 11:00", professor: "Dr. Brown", capacity: 35, enrolled: 32, prerequisites: []},
  {courseId: "ENG101", courseName: "English Composition", credits: 3, difficulty: "Beginner", timeSlot: "TTH 2:00", professor: "Dr. Wilson", capacity: 20, enrolled: 15, prerequisites: []},
  {courseId: "CS301", courseName: "Algorithms", credits: 4, difficulty: "Advanced", timeSlot: "MWF 2:00", professor: "Dr. Davis", capacity: 20, enrolled: 18, prerequisites: ["CS201", "MATH150"]}
];

// Solution
function optimizeStudentSchedule(courses, studentProfile) {
  const profile = {
    completedCourses: ["CS101"],
    maxCredits: 12,
    preferredDifficulty: "Intermediate",
    avoidTimeSlots: ["MWF 8:00"],
    prioritySubjects: ["CS", "MATH"],
    ...studentProfile
  };
  
  return courses.map(course => {
    // Check prerequisites
    const prerequisitesMet = course.prerequisites.every(prereq => 
      profile.completedCourses.includes(prereq)
    );
    
    // Calculate availability
    const availableSpots = course.capacity - course.enrolled;
    const availabilityScore = availableSpots / course.capacity;
    
    // Subject priority score
    const subjectCode = course.courseId.substring(0, course.courseId.search(/\d/));
    const subjectPriorityScore = profile.prioritySubjects.includes(subjectCode) ? 1 : 0.5;
    
    // Difficulty match score
    const difficultyMatch = course.difficulty === profile.preferredDifficulty ? 1 : 0.7;
    
    // Time slot preference
    const timePreference = profile.avoidTimeSlots.includes(course.timeSlot) ? 0.3 : 1;
    
    // Combined recommendation score
    const recommendationScore = prerequisitesMet ? 
      (subjectPriorityScore * 0.3 + availabilityScore * 0.25 + difficultyMatch * 0.25 + timePreference * 0.2) : 0;
    
    return {
      ...course,
      availableSpots,
      prerequisitesMet,
      recommendationScore: parseFloat(recommendationScore.toFixed(3)),
      enrollmentStatus: availableSpots > 0 ? "Open" : "Full",
      difficultyMatch: course.difficulty === profile.preferredDifficulty
    };
  }).filter(course => course.prerequisitesMet && course.availableSpots > 0)
    .sort((a, b) => {
      // Primary: Recommendation score
      if (Math.abs(a.recommendationScore - b.recommendationScore) > 0.1) {
        return b.recommendationScore - a.recommendationScore;
      }
      
      // Secondary: Available spots (less competition)
      return b.availableSpots - a.availableSpots;
    }).map((course, index) => ({
      scheduleRank: index + 1,
      ...course,
      recommendation: index < 3 ? "Highly Recommended" : 
                     index < 6 ? "Recommended" : "Consider",
      enrollmentUrgency: course.availableSpots <= 2 ? "Enroll ASAP" : 
                        course.availableSpots <= 5 ? "Enroll Soon" : "Normal Priority"
    }));
}

// Example usage
const studentProfile = {
  completedCourses: ["CS101"],
  maxCredits: 16,
  preferredDifficulty: "Intermediate",
  prioritySubjects: ["CS", "MATH"]
};

console.log(optimizeStudentSchedule(courses, studentProfile));
```

### 16. Delivery Route Optimization
**Scenario**: Logistics company optimizing delivery routes by distance, priority, and time windows
```javascript
// Real-world data
const deliveries = [
  {deliveryId: "D001", address: "123 Main St", priority: "High", time