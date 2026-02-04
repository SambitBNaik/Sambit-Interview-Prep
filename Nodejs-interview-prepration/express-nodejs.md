# Express.js & Node.js Interview Preparation Guide

---

## Table of Contents
1. [Node.js Fundamentals](#nodejs-fundamentals)
2. [Express.js Basics](#expressjs-basics)
3. [Middleware](#middleware)
4. [Routing](#routing)
5. [Request & Response](#request--response)
6. [Error Handling](#error-handling)
7. [Database Integration](#database-integration)
8. [Authentication & Security](#authentication--security)
9. [Performance & Optimization](#performance--optimization)
10. [Testing](#testing)
11. [Common Interview Questions](#common-interview-questions)

---

# Node.js Fundamentals

## 1. What is Node.js?

**Definition:**
Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine. It allows developers to run JavaScript on the server-side.

**Key Features:**
- **Event-Driven**: Uses an event-driven, non-blocking I/O model
- **Single-Threaded**: Uses a single thread with event loop
- **Asynchronous**: Non-blocking operations
- **Fast**: Built on V8 engine (compiles JS to machine code)
- **NPM**: Largest package ecosystem

**Use Cases:**
- REST APIs
- Real-time applications (chat, notifications)
- Microservices
- Streaming applications
- Server-side rendering

---

## 2. Event Loop

**What is Event Loop?**
The event loop is what allows Node.js to perform non-blocking I/O operations despite JavaScript being single-threaded.

**Phases of Event Loop:**
```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           poll            │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

**Phases Explanation:**
1. **Timers**: Executes callbacks scheduled by `setTimeout()` and `setInterval()`
2. **Pending Callbacks**: Executes I/O callbacks deferred to the next loop
3. **Idle, Prepare**: Internal use only
4. **Poll**: Retrieves new I/O events; executes I/O callbacks
5. **Check**: Executes `setImmediate()` callbacks
6. **Close Callbacks**: Executes close callbacks (e.g., `socket.on('close')`)

**Example:**
```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

console.log('4');

// Output: 1, 4, 3, 2
// Why? Sync code → Microtasks (Promises) → Macrotasks (setTimeout)
```

---

## 3. Blocking vs Non-Blocking

**Blocking (Synchronous):**
```javascript
const fs = require('fs');

// Blocks execution until file is read
const data = fs.readFileSync('file.txt', 'utf8');
console.log(data);
console.log('After reading'); // Waits for file read
```

**Non-Blocking (Asynchronous):**
```javascript
const fs = require('fs');

// Does not block execution
fs.readFile('file.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log(data);
});
console.log('After reading'); // Executes immediately
```

**Why Non-Blocking?**
- Better performance
- Handles more concurrent requests
- Doesn't freeze the application

---

## 4. Module System

**Types of Modules:**

### 1. Core Modules (Built-in)
```javascript
const fs = require('fs');
const http = require('http');
const path = require('path');
const os = require('os');
```

### 2. Local Modules (Your Files)
```javascript
// math.js
module.exports = {
    add: (a, b) => a + b,
    subtract: (a, b) => a - b
};

// app.js
const math = require('./math');
console.log(math.add(5, 3)); // 8
```

### 3. Third-Party Modules (NPM)
```javascript
const express = require('express');
const mongoose = require('mongoose');
```

**CommonJS vs ES Modules:**
```javascript
// CommonJS (Node.js default)
const express = require('express');
module.exports = { /* ... */ };

// ES Modules (Modern)
import express from 'express';
export default { /* ... */ };
```

---

## 5. Buffer and Streams

**Buffer:**
Temporary storage for binary data (used for file operations, network communications).

```javascript
// Creating buffers
const buf1 = Buffer.from('Hello');
const buf2 = Buffer.alloc(10);

console.log(buf1.toString()); // 'Hello'
console.log(buf1.length); // 5
```

**Streams:**
Process data piece by piece without loading entire content into memory.

**Types of Streams:**
1. **Readable**: Read data from source (fs.createReadStream)
2. **Writable**: Write data to destination (fs.createWriteStream)
3. **Duplex**: Both readable and writable (TCP socket)
4. **Transform**: Modify data while reading/writing (zlib)

```javascript
const fs = require('fs');

// Without stream (loads entire file in memory)
const data = fs.readFileSync('large-file.txt');

// With stream (reads chunk by chunk)
const readStream = fs.createReadStream('large-file.txt');
readStream.on('data', (chunk) => {
    console.log('Chunk:', chunk);
});
```

---

# Express.js Basics

## 1. What is Express.js?

**Definition:**
Express.js is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.

**Why Use Express?**
- Simplifies server creation
- Middleware support
- Routing system
- Template engine support
- Easy integration with databases
- Large community and ecosystem

**Basic Express Server:**
```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(3000, () => {
    console.log('Server running on port 3000');
});
```

---

## 2. Application vs Router

**Application (app):**
```javascript
const express = require('express');
const app = express(); // Creates application

app.get('/users', (req, res) => { /* ... */ });
app.post('/users', (req, res) => { /* ... */ });
```

**Router:**
```javascript
const express = require('express');
const router = express.Router(); // Creates router

router.get('/', (req, res) => { /* ... */ });
router.post('/', (req, res) => { /* ... */ });

module.exports = router;

// In main app
app.use('/users', userRouter);
```

**Difference:**
- `app` is the main application instance
- `router` is a mini-application for modular routing
- Routers can be mounted on app

---

# Middleware

## 1. What is Middleware?

**Definition:**
Middleware functions are functions that have access to the request object (req), response object (res), and the next middleware function in the application's request-response cycle.

**Middleware Flow:**
```
Request → Middleware 1 → Middleware 2 → Route Handler → Response
```

**Basic Middleware:**
```javascript
app.use((req, res, next) => {
    console.log('Time:', Date.now());
    next(); // Pass control to next middleware
});
```

---

## 2. Types of Middleware

### 1. Application-Level Middleware
```javascript
app.use((req, res, next) => {
    console.log('Request Type:', req.method);
    next();
});

app.use('/user/:id', (req, res, next) => {
    console.log('Request URL:', req.originalUrl);
    next();
});
```

### 2. Router-Level Middleware
```javascript
const router = express.Router();

router.use((req, res, next) => {
    console.log('Router middleware');
    next();
});

router.get('/user/:id', (req, res) => {
    res.send('User Info');
});
```

### 3. Error-Handling Middleware
```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});
```

### 4. Built-in Middleware
```javascript
// Serve static files
app.use(express.static('public'));

// Parse JSON bodies
app.use(express.json());

// Parse URL-encoded bodies
app.use(express.urlencoded({ extended: true }));
```

### 5. Third-Party Middleware
```javascript
const cors = require('cors');
const morgan = require('morgan');
const helmet = require('helmet');

app.use(cors());
app.use(morgan('dev'));
app.use(helmet());
```

---

## 3. Middleware Execution Order

**Important:** Order matters! Middleware executes in the order it's defined.

```javascript
// This runs first
app.use((req, res, next) => {
    console.log('1');
    next();
});

// This runs second
app.use((req, res, next) => {
    console.log('2');
    next();
});

// This runs third
app.get('/', (req, res) => {
    console.log('3');
    res.send('Done');
});

// Output: 1, 2, 3
```

**Common Mistake:**
```javascript
// ❌ Wrong - route defined before middleware
app.get('/users', (req, res) => {
    res.send('Users');
});

app.use(express.json()); // Too late!

// ✅ Correct - middleware before routes
app.use(express.json());

app.get('/users', (req, res) => {
    res.send('Users');
});
```

---

# Routing

## 1. Route Methods

```javascript
// GET - Retrieve data
app.get('/users', (req, res) => {
    res.send('Get all users');
});

// POST - Create data
app.post('/users', (req, res) => {
    res.send('Create user');
});

// PUT - Update entire resource
app.put('/users/:id', (req, res) => {
    res.send('Update user');
});

// PATCH - Update partial resource
app.patch('/users/:id', (req, res) => {
    res.send('Partially update user');
});

// DELETE - Delete data
app.delete('/users/:id', (req, res) => {
    res.send('Delete user');
});

// ALL - Handle all HTTP methods
app.all('/secret', (req, res) => {
    res.send('Accessing secret section');
});
```

---

## 2. Route Parameters

### URL Parameters
```javascript
app.get('/users/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
});

// Multiple parameters
app.get('/users/:userId/posts/:postId', (req, res) => {
    const { userId, postId } = req.params;
    res.send(`User: ${userId}, Post: ${postId}`);
});

// Optional parameters
app.get('/users/:id?', (req, res) => {
    if (req.params.id) {
        res.send(`User ID: ${req.params.id}`);
    } else {
        res.send('All users');
    }
});
```

### Query Parameters
```javascript
// URL: /search?name=john&age=25
app.get('/search', (req, res) => {
    const { name, age } = req.query;
    res.send(`Name: ${name}, Age: ${age}`);
});
```

---

## 3. Route Handlers

### Single Handler
```javascript
app.get('/users', (req, res) => {
    res.send('Users');
});
```

### Multiple Handlers (Chaining)
```javascript
const checkAuth = (req, res, next) => {
    if (req.isAuthenticated()) {
        next();
    } else {
        res.status(401).send('Unauthorized');
    }
};

app.get('/admin', checkAuth, (req, res) => {
    res.send('Admin panel');
});
```

### Array of Handlers
```javascript
const middleware1 = (req, res, next) => { next(); };
const middleware2 = (req, res, next) => { next(); };

app.get('/users', [middleware1, middleware2], (req, res) => {
    res.send('Users');
});
```

---

## 4. app.route()

**Chain route handlers for a route path:**
```javascript
app.route('/users')
    .get((req, res) => {
        res.send('Get all users');
    })
    .post((req, res) => {
        res.send('Create user');
    })
    .put((req, res) => {
        res.send('Update user');
    });
```

---

# Request & Response

## 1. Request Object (req)

**Common Properties:**
```javascript
app.get('/test', (req, res) => {
    // Route parameters
    console.log(req.params);
    
    // Query string parameters
    console.log(req.query);
    
    // Request body (needs body-parser)
    console.log(req.body);
    
    // HTTP method
    console.log(req.method); // GET, POST, etc.
    
    // Request path
    console.log(req.path); // /test
    
    // Full URL
    console.log(req.originalUrl);
    
    // Headers
    console.log(req.headers);
    console.log(req.get('Content-Type'));
    
    // IP address
    console.log(req.ip);
    
    // Cookies (needs cookie-parser)
    console.log(req.cookies);
    
    // Request protocol
    console.log(req.protocol); // http or https
});
```

---

## 2. Response Object (res)

**Common Methods:**
```javascript
app.get('/response-demo', (req, res) => {
    // Send string
    res.send('Hello World');
    
    // Send JSON
    res.json({ name: 'John', age: 30 });
    
    // Send status code
    res.status(404).send('Not Found');
    res.sendStatus(200); // OK
    
    // Redirect
    res.redirect('/login');
    res.redirect(301, '/moved-permanently');
    
    // Send file
    res.sendFile('/path/to/file.pdf');
    
    // Download file
    res.download('/path/to/file.pdf');
    
    // Set headers
    res.set('Content-Type', 'text/html');
    res.type('json');
    
    // Set cookie
    res.cookie('name', 'value', { maxAge: 900000 });
    
    // Clear cookie
    res.clearCookie('name');
    
    // Render template
    res.render('index', { title: 'Home' });
});
```

**Method Chaining:**
```javascript
res.status(200).json({ success: true });
res.status(404).send('Not Found');
```

---

# Error Handling

## 1. Synchronous Errors

```javascript
app.get('/sync-error', (req, res) => {
    throw new Error('Something went wrong!');
    // Express automatically catches this
});
```

---

## 2. Asynchronous Errors

**Without async/await:**
```javascript
app.get('/async-error', (req, res, next) => {
    fs.readFile('file.txt', (err, data) => {
        if (err) {
            next(err); // Pass error to error handler
        } else {
            res.send(data);
        }
    });
});
```

**With async/await:**
```javascript
app.get('/async-error', async (req, res, next) => {
    try {
        const data = await someAsyncOperation();
        res.json(data);
    } catch (error) {
        next(error); // Pass to error handler
    }
});
```

---

## 3. Error Handling Middleware

**Basic Error Handler:**
```javascript
app.use((err, req, res, next) => {
    console.error(err.stack);
    res.status(500).send('Something broke!');
});
```

**Advanced Error Handler:**
```javascript
app.use((err, req, res, next) => {
    // Log error
    console.error(err.stack);
    
    // Determine status code
    const statusCode = err.statusCode || 500;
    
    // Send response
    res.status(statusCode).json({
        success: false,
        message: err.message,
        stack: process.env.NODE_ENV === 'development' ? err.stack : undefined
    });
});
```

**Custom Error Class:**
```javascript
class AppError extends Error {
    constructor(message, statusCode) {
        super(message);
        this.statusCode = statusCode;
        this.isOperational = true;
        Error.captureStackTrace(this, this.constructor);
    }
}

// Usage
app.get('/users/:id', async (req, res, next) => {
    const user = await User.findById(req.params.id);
    if (!user) {
        return next(new AppError('User not found', 404));
    }
    res.json(user);
});
```

---

## 4. 404 Handler

```javascript
// Handle 404 - must be AFTER all routes
app.use((req, res, next) => {
    res.status(404).json({
        success: false,
        message: 'Route not found'
    });
});
```

---

# Database Integration

## 1. MongoDB with Mongoose

**Connection:**
```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost/myapp', {
    useNewUrlParser: true,
    useUnifiedTopology: true
})
.then(() => console.log('MongoDB connected'))
.catch(err => console.error('MongoDB connection error:', err));
```

**Schema & Model:**
```javascript
const userSchema = new mongoose.Schema({
    name: { type: String, required: true },
    email: { type: String, required: true, unique: true },
    age: { type: Number, min: 0 },
    createdAt: { type: Date, default: Date.now }
});

const User = mongoose.model('User', userSchema);
```

**CRUD Operations:**
```javascript
// Create
app.post('/users', async (req, res) => {
    try {
        const user = new User(req.body);
        await user.save();
        res.status(201).json(user);
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

// Read
app.get('/users', async (req, res) => {
    const users = await User.find();
    res.json(users);
});

app.get('/users/:id', async (req, res) => {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).json({ error: 'User not found' });
    res.json(user);
});

// Update
app.put('/users/:id', async (req, res) => {
    const user = await User.findByIdAndUpdate(
        req.params.id,
        req.body,
        { new: true, runValidators: true }
    );
    if (!user) return res.status(404).json({ error: 'User not found' });
    res.json(user);
});

// Delete
app.delete('/users/:id', async (req, res) => {
    const user = await User.findByIdAndDelete(req.params.id);
    if (!user) return res.status(404).json({ error: 'User not found' });
    res.json({ message: 'User deleted' });
});
```

---

## 2. MySQL/PostgreSQL (SQL Databases)

**Using pg (PostgreSQL):**
```javascript
const { Pool } = require('pg');

const pool = new Pool({
    user: 'username',
    host: 'localhost',
    database: 'mydb',
    password: 'password',
    port: 5432
});

// Query
app.get('/users', async (req, res) => {
    try {
        const result = await pool.query('SELECT * FROM users');
        res.json(result.rows);
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

// Parameterized query
app.get('/users/:id', async (req, res) => {
    const { id } = req.params;
    const result = await pool.query('SELECT * FROM users WHERE id = $1', [id]);
    res.json(result.rows[0]);
});
```

---

# Authentication & Security

## 1. JWT Authentication

**Installation:**
```bash
npm install jsonwebtoken bcryptjs
```

**Implementation:**
```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

// Register
app.post('/register', async (req, res) => {
    const { email, password } = req.body;
    
    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);
    
    // Save user
    const user = await User.create({ email, password: hashedPassword });
    
    res.status(201).json({ message: 'User created' });
});

// Login
app.post('/login', async (req, res) => {
    const { email, password } = req.body;
    
    // Find user
    const user = await User.findOne({ email });
    if (!user) return res.status(401).json({ error: 'Invalid credentials' });
    
    // Check password
    const isMatch = await bcrypt.compare(password, user.password);
    if (!isMatch) return res.status(401).json({ error: 'Invalid credentials' });
    
    // Generate token
    const token = jwt.sign(
        { userId: user._id },
        process.env.JWT_SECRET,
        { expiresIn: '7d' }
    );
    
    res.json({ token });
});

// Auth middleware
const auth = (req, res, next) => {
    try {
        const token = req.header('Authorization').replace('Bearer ', '');
        const decoded = jwt.verify(token, process.env.JWT_SECRET);
        req.userId = decoded.userId;
        next();
    } catch (error) {
        res.status(401).json({ error: 'Please authenticate' });
    }
};

// Protected route
app.get('/profile', auth, async (req, res) => {
    const user = await User.findById(req.userId);
    res.json(user);
});
```

---

## 2. Security Best Practices

### Use Helmet
```javascript
const helmet = require('helmet');
app.use(helmet());
```

### CORS
```javascript
const cors = require('cors');
app.use(cors({
    origin: 'https://yourfrontend.com',
    credentials: true
}));
```

### Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

### Input Validation
```javascript
const { body, validationResult } = require('express-validator');

app.post('/users', [
    body('email').isEmail(),
    body('password').isLength({ min: 8 })
], (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
        return res.status(400).json({ errors: errors.array() });
    }
    // Process request
});
```

### Environment Variables
```javascript
require('dotenv').config();

const dbPassword = process.env.DB_PASSWORD;
const jwtSecret = process.env.JWT_SECRET;
```

---

# Performance & Optimization

## 1. Compression
```javascript
const compression = require('compression');
app.use(compression());
```

## 2. Caching
```javascript
const redis = require('redis');
const client = redis.createClient();

app.get('/users/:id', async (req, res) => {
    const { id } = req.params;
    
    // Check cache
    client.get(`user:${id}`, async (err, data) => {
        if (data) {
            return res.json(JSON.parse(data));
        }
        
        // Fetch from database
        const user = await User.findById(id);
        
        // Store in cache
        client.setex(`user:${id}`, 3600, JSON.stringify(user));
        
        res.json(user);
    });
});
```

## 3. Clustering
```javascript
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
    const numCPUs = os.cpus().length;
    
    for (let i = 0; i < numCPUs; i++) {
        cluster.fork();
    }
    
    cluster.on('exit', (worker) => {
        console.log(`Worker ${worker.process.pid} died`);
        cluster.fork();
    });
} else {
    // Worker process
    const app = express();
    // ... app setup
    app.listen(3000);
}
```

## 4. Use PM2 in Production
```bash
npm install -g pm2
pm2 start app.js -i max  # Use all CPU cores
pm2 monit                 # Monitor
pm2 logs                  # View logs
```

---

# Testing

## 1. Unit Testing with Jest

```javascript
// math.js
const add = (a, b) => a + b;
module.exports = { add };

// math.test.js
const { add } = require('./math');

test('adds 1 + 2 to equal 3', () => {
    expect(add(1, 2)).toBe(3);
});
```

## 2. API Testing with Supertest

```javascript
const request = require('supertest');
const app = require('./app');

describe('GET /users', () => {
    it('should return all users', async () => {
        const res = await request(app)
            .get('/users')
            .expect(200);
        
        expect(Array.isArray(res.body)).toBe(true);
    });
});

describe('POST /users', () => {
    it('should create a new user', async () => {
        const res = await request(app)
            .post('/users')
            .send({ name: 'John', email: 'john@example.com' })
            .expect(201);
        
        expect(res.body).toHaveProperty('_id');
        expect(res.body.name).toBe('John');
    });
});
```

---

# Common Interview Questions

## 1. What is the difference between Node.js and Express.js?

**Answer:**
- **Node.js** is a runtime environment that allows JavaScript to run on the server
- **Express.js** is a web framework built on top of Node.js that simplifies building web applications and APIs
- Node.js provides the foundation; Express provides structure and features

---

## 2. What is middleware? Explain with example.

**Answer:**
Middleware functions have access to req, res, and next. They execute in order and can:
- Execute code
- Modify req/res objects
- End request-response cycle
- Call next middleware

```javascript
app.use((req, res, next) => {
    console.log('Request received');
    next(); // Pass to next middleware
});
```

---

## 3. Difference between app.use() and app.get()?

**Answer:**
- `app.use()`: Mounts middleware, works for all HTTP methods
- `app.get()`: Defines route for GET requests only

```javascript
app.use('/users', middleware);  // All methods on /users
app.get('/users', handler);     // Only GET on /users
```

---

## 4. How to handle async errors in Express?

**Answer:**
Use try-catch with async/await and pass errors to next():

```javascript
app.get('/users', async (req, res, next) => {
    try {
        const users = await User.find();
        res.json(users);
    } catch (error) {
        next(error); // Pass to error handler
    }
});

// Error handler
app.use((err, req, res, next) => {
    res.status(500).json({ error: err.message });
});
```

---

## 5. What is the purpose of next() in middleware?

**Answer:**
`next()` passes control to the next middleware function. Without calling next(), the request will hang.

```javascript
app.use((req, res, next) => {
    console.log('Middleware 1');
    next(); // Continue to next middleware
});

app.use((req, res, next) => {
    console.log('Middleware 2');
    res.send('Done'); // End here
});
```

---

## 6. How does Express handle 404 errors?

**Answer:**
Define a catch-all route AFTER all other routes:

```javascript
// All routes above

// 404 handler
app.use((req, res) => {
    res.status(404).json({ error: 'Route not found' });
});
```

---

## 7. Difference between res.send() and res.json()?

**Answer:**
- `res.send()`: Can send string, object, Buffer, etc. Auto-detects content type
- `res.json()`: Specifically for JSON, sets Content-Type to application/json

```javascript
res.send({ name: 'John' });      // Works
res.send('Hello');               // Works
res.json({ name: 'John' });      // Explicit JSON
```

---

## 8. What is CORS and how to enable it?

**Answer:**
CORS (Cross-Origin Resource Sharing) allows requests from different origins.

```javascript
const cors = require('cors');

// Allow all origins
app.use(cors());

// Specific origin
app.use(cors({
    origin: 'https://example.com'
}));
```

---

## 9. How to serve static files in Express?

**Answer:**
```javascript
app.use(express.static('public'));

// Now files in 'public' folder are accessible:
// http://localhost:3000/image.jpg
// http://localhost:3000/css/style.css
```

---

## 10. What is the difference between PUT and PATCH?

**Answer:**
- **PUT**: Replace entire resource
- **PATCH**: Update specific fields

```javascript
// PUT - update entire user
app.put('/users/:id', (req, res) => {
    // req.body should have ALL user fields
});

// PATCH - update specific fields
app.patch('/users/:id', (req, res) => {
    // req.body can have only changed fields
});
```

---

## Summary

### Key Concepts to Remember:
1. ✅ Node.js is single-threaded with event loop
2. ✅ Express simplifies Node.js web development
3. ✅ Middleware executes in order
4. ✅ Always handle async errors
5. ✅ Use proper HTTP methods (GET, POST, PUT, DELETE)
6. ✅ Implement authentication and security
7. ✅ Optimize with caching and compression
8. ✅ Write tests for your APIs
9. ✅ Use environment variables for secrets
10. ✅ Follow REST API best practices

### Interview Preparation Tips:
- 📚 Understand event loop deeply
- 🔧 Practice building REST APIs
- 🛡️ Know security best practices
- 🧪 Learn testing basics
- 📊 Understand database integration
- ⚡ Know performance optimization techniques

Good luck with your interviews! 🚀