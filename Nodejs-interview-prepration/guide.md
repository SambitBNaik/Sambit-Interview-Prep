# Node.js Backend Preparation Guide

## Table of Contents
1. [Core Node.js Concepts](#core-nodejs-concepts)
2. [Express.js Framework](#expressjs-framework)
3. [Database Integration](#database-integration)
4. [Authentication & Authorization](#authentication--authorization)
5. [RESTful API Design](#restful-api-design)
6. [Middleware](#middleware)
7. [Error Handling](#error-handling)
8. [Security Best Practices](#security-best-practices)
9. [Testing](#testing)
10. [Performance & Optimization](#performance--optimization)
11. [Interview Questions](#interview-questions)

---

## Core Node.js Concepts

### Event Loop
- Understanding the event loop architecture
- Call stack, callback queue, and microtask queue
- Blocking vs non-blocking operations
- `process.nextTick()` vs `setImmediate()`

### Asynchronous Programming
```javascript
// Callbacks
fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});

// Promises
const readFilePromise = promisify(fs.readFile);
readFilePromise('file.txt')
  .then(data => console.log(data))
  .catch(err => console.error(err));

// Async/Await
async function readFile() {
  try {
    const data = await readFilePromise('file.txt');
    console.log(data);
  } catch (err) {
    console.error(err);
  }
}
```

### Modules
- CommonJS (`require`, `module.exports`)
- ES6 Modules (`import`, `export`)
- Built-in modules: `fs`, `path`, `http`, `crypto`, `events`

### Streams
```javascript
const fs = require('fs');

// Readable stream
const readStream = fs.createReadStream('input.txt');
readStream.on('data', (chunk) => {
  console.log('Received chunk:', chunk);
});

// Writable stream
const writeStream = fs.createWriteStream('output.txt');
writeStream.write('Hello World');

// Pipe streams
readStream.pipe(writeStream);
```

### Buffer
- Working with binary data
- Converting between Buffer and strings
- Buffer memory management

---

## Express.js Framework

### Basic Setup
```javascript
const express = require('express');
const app = express();

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.get('/', (req, res) => {
  res.send('Hello World');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

### Routing
```javascript
// Basic routes
app.get('/users', getAllUsers);
app.get('/users/:id', getUserById);
app.post('/users', createUser);
app.put('/users/:id', updateUser);
app.delete('/users/:id', deleteUser);

// Route parameters and query strings
app.get('/search', (req, res) => {
  const { query, page, limit } = req.query;
  // Process search
});

// Express Router
const userRouter = express.Router();
userRouter.get('/', getAllUsers);
userRouter.post('/', createUser);
app.use('/api/users', userRouter);
```

### Request and Response Objects
- `req.params`, `req.query`, `req.body`, `req.headers`
- `res.send()`, `res.json()`, `res.status()`, `res.redirect()`

---

## Database Integration

### MongoDB with Mongoose
```javascript
const mongoose = require('mongoose');

// Connection
mongoose.connect('mongodb://localhost:27017/mydb', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});

// Schema
const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  password: { type: String, required: true },
  createdAt: { type: Date, default: Date.now }
});

// Model
const User = mongoose.model('User', userSchema);

// CRUD Operations
const user = new User({ name: 'John', email: 'john@example.com' });
await user.save();

const users = await User.find({ name: 'John' });
const user = await User.findById(id);
await User.updateOne({ _id: id }, { name: 'Jane' });
await User.deleteOne({ _id: id });
```

### PostgreSQL with Sequelize
```javascript
const { Sequelize, DataTypes } = require('sequelize');

const sequelize = new Sequelize('database', 'username', 'password', {
  host: 'localhost',
  dialect: 'postgres'
});

const User = sequelize.define('User', {
  name: { type: DataTypes.STRING, allowNull: false },
  email: { type: DataTypes.STRING, unique: true }
});

await sequelize.sync();
await User.create({ name: 'John', email: 'john@example.com' });
```

### SQL Queries (Raw)
```javascript
const mysql = require('mysql2/promise');

const connection = await mysql.createConnection({
  host: 'localhost',
  user: 'root',
  database: 'test'
});

const [rows] = await connection.execute(
  'SELECT * FROM users WHERE id = ?',
  [userId]
);
```

---

## Authentication & Authorization

### JWT (JSON Web Tokens)
```javascript
const jwt = require('jsonwebtoken');
const bcrypt = require('bcryptjs');

// Registration
async function register(req, res) {
  const { email, password } = req.body;
  const hashedPassword = await bcrypt.hash(password, 10);
  
  const user = await User.create({
    email,
    password: hashedPassword
  });
  
  res.status(201).json({ message: 'User created' });
}

// Login
async function login(req, res) {
  const { email, password } = req.body;
  const user = await User.findOne({ email });
  
  if (!user || !(await bcrypt.compare(password, user.password))) {
    return res.status(401).json({ message: 'Invalid credentials' });
  }
  
  const token = jwt.sign(
    { userId: user._id, email: user.email },
    process.env.JWT_SECRET,
    { expiresIn: '1h' }
  );
  
  res.json({ token });
}

// Auth Middleware
function authenticateToken(req, res, next) {
  const token = req.headers['authorization']?.split(' ')[1];
  
  if (!token) {
    return res.status(401).json({ message: 'No token provided' });
  }
  
  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.status(403).json({ message: 'Invalid token' });
    req.user = user;
    next();
  });
}
```

### Session-Based Authentication
```javascript
const session = require('express-session');

app.use(session({
  secret: 'your-secret-key',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true, maxAge: 3600000 }
}));

// Login
app.post('/login', async (req, res) => {
  // Verify credentials
  req.session.userId = user._id;
  res.json({ message: 'Logged in' });
});

// Protected route
app.get('/profile', (req, res) => {
  if (!req.session.userId) {
    return res.status(401).json({ message: 'Not authenticated' });
  }
  // Return user profile
});
```

### OAuth 2.0 / Passport.js
```javascript
const passport = require('passport');
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
    clientID: GOOGLE_CLIENT_ID,
    clientSecret: GOOGLE_CLIENT_SECRET,
    callbackURL: "http://localhost:3000/auth/google/callback"
  },
  function(accessToken, refreshToken, profile, cb) {
    // Find or create user
    User.findOrCreate({ googleId: profile.id }, cb);
  }
));
```

---

## RESTful API Design

### HTTP Methods
- **GET**: Retrieve resources
- **POST**: Create new resources
- **PUT**: Update entire resource
- **PATCH**: Partial update
- **DELETE**: Remove resources

### Status Codes
- **200**: OK
- **201**: Created
- **204**: No Content
- **400**: Bad Request
- **401**: Unauthorized
- **403**: Forbidden
- **404**: Not Found
- **500**: Internal Server Error

### API Versioning
```javascript
// URL versioning
app.use('/api/v1/users', userRoutesV1);
app.use('/api/v2/users', userRoutesV2);

// Header versioning
app.use((req, res, next) => {
  const version = req.headers['api-version'];
  // Route based on version
});
```

### Pagination
```javascript
app.get('/users', async (req, res) => {
  const page = parseInt(req.query.page) || 1;
  const limit = parseInt(req.query.limit) || 10;
  const skip = (page - 1) * limit;
  
  const users = await User.find()
    .skip(skip)
    .limit(limit);
  
  const total = await User.countDocuments();
  
  res.json({
    data: users,
    pagination: {
      page,
      limit,
      total,
      pages: Math.ceil(total / limit)
    }
  });
});
```

---

## Middleware

### Types of Middleware
```javascript
// Application-level middleware
app.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Router-level middleware
router.use((req, res, next) => {
  // Check authentication
  next();
});

// Error-handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});

// Built-in middleware
app.use(express.json());
app.use(express.static('public'));

// Third-party middleware
const cors = require('cors');
app.use(cors());

const morgan = require('morgan');
app.use(morgan('combined'));
```

### Custom Middleware
```javascript
// Logging middleware
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

// Validation middleware
const validateUser = (req, res, next) => {
  const { email, password } = req.body;
  if (!email || !password) {
    return res.status(400).json({ error: 'Missing required fields' });
  }
  next();
};

// Rate limiting
const rateLimit = require('express-rate-limit');
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100
});
app.use('/api/', limiter);
```

---

## Error Handling

### Try-Catch with Async/Await
```javascript
app.get('/users/:id', async (req, res, next) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) {
      return res.status(404).json({ error: 'User not found' });
    }
    res.json(user);
  } catch (error) {
    next(error);
  }
});
```

### Custom Error Classes
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
throw new AppError('User not found', 404);
```

### Global Error Handler
```javascript
app.use((err, req, res, next) => {
  err.statusCode = err.statusCode || 500;
  err.status = err.status || 'error';
  
  if (process.env.NODE_ENV === 'development') {
    res.status(err.statusCode).json({
      status: err.status,
      error: err,
      message: err.message,
      stack: err.stack
    });
  } else {
    res.status(err.statusCode).json({
      status: err.status,
      message: err.message
    });
  }
});
```

### Async Error Wrapper
```javascript
const catchAsync = (fn) => {
  return (req, res, next) => {
    fn(req, res, next).catch(next);
  };
};

// Usage
app.get('/users', catchAsync(async (req, res) => {
  const users = await User.find();
  res.json(users);
}));
```

---

## Security Best Practices

### Environment Variables
```javascript
require('dotenv').config();

const dbURL = process.env.DATABASE_URL;
const jwtSecret = process.env.JWT_SECRET;
```

### Helmet.js (Security Headers)
```javascript
const helmet = require('helmet');
app.use(helmet());
```

### CORS Configuration
```javascript
const cors = require('cors');

app.use(cors({
  origin: 'https://yourdomain.com',
  credentials: true,
  optionsSuccessStatus: 200
}));
```

### Input Validation & Sanitization
```javascript
const { body, validationResult } = require('express-validator');

app.post('/users',
  body('email').isEmail().normalizeEmail(),
  body('password').isLength({ min: 6 }),
  (req, res) => {
    const errors = validationResult(req);
    if (!errors.isEmpty()) {
      return res.status(400).json({ errors: errors.array() });
    }
    // Process request
  }
);
```

### SQL Injection Prevention
```javascript
// Use parameterized queries
const [rows] = await connection.execute(
  'SELECT * FROM users WHERE id = ?',
  [userId]
);

// With ORM
const user = await User.findOne({ where: { id: userId } });
```

### XSS Protection
```javascript
const xss = require('xss-clean');
app.use(xss());
```

### Rate Limiting
```javascript
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: 'Too many requests from this IP'
});

app.use('/api/', limiter);
```

---

## Testing

### Unit Testing with Jest
```javascript
// user.service.test.js
const userService = require('./user.service');

describe('User Service', () => {
  test('should create a new user', async () => {
    const userData = { name: 'John', email: 'john@example.com' };
    const user = await userService.createUser(userData);
    
    expect(user).toHaveProperty('_id');
    expect(user.name).toBe('John');
  });
  
  test('should throw error for duplicate email', async () => {
    const userData = { name: 'Jane', email: 'existing@example.com' };
    
    await expect(userService.createUser(userData))
      .rejects.toThrow('Email already exists');
  });
});
```

### Integration Testing with Supertest
```javascript
const request = require('supertest');
const app = require('./app');

describe('User API', () => {
  test('GET /api/users should return users', async () => {
    const response = await request(app)
      .get('/api/users')
      .expect('Content-Type', /json/)
      .expect(200);
    
    expect(response.body).toBeInstanceOf(Array);
  });
  
  test('POST /api/users should create user', async () => {
    const response = await request(app)
      .post('/api/users')
      .send({ name: 'John', email: 'john@example.com' })
      .expect(201);
    
    expect(response.body).toHaveProperty('_id');
  });
});
```

### Mocking
```javascript
jest.mock('./user.model');
const User = require('./user.model');

User.findById.mockResolvedValue({
  _id: '123',
  name: 'John'
});
```

---

## Performance & Optimization

### Clustering
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
  // Start server
  app.listen(3000);
}
```

### Caching with Redis
```javascript
const redis = require('redis');
const client = redis.createClient();

// Cache middleware
const cache = (duration) => {
  return async (req, res, next) => {
    const key = `cache:${req.originalUrl}`;
    
    const cachedData = await client.get(key);
    if (cachedData) {
      return res.json(JSON.parse(cachedData));
    }
    
    res.originalJson = res.json;
    res.json = (data) => {
      client.setex(key, duration, JSON.stringify(data));
      res.originalJson(data);
    };
    
    next();
  };
};

app.get('/users', cache(300), async (req, res) => {
  const users = await User.find();
  res.json(users);
});
```

### Database Indexing
```javascript
// Mongoose
userSchema.index({ email: 1 });
userSchema.index({ name: 1, createdAt: -1 });

// Create indexes
await User.createIndexes();
```

### Compression
```javascript
const compression = require('compression');
app.use(compression());
```

### Connection Pooling
```javascript
const pool = mysql.createPool({
  host: 'localhost',
  user: 'root',
  database: 'test',
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0
});
```

---

## Interview Questions

### Basic Questions
1. **What is Node.js and how does it work?**
   - Node.js is a JavaScript runtime built on Chrome's V8 engine that executes JavaScript on the server side using an event-driven, non-blocking I/O model.

2. **Explain the Event Loop**
   - The event loop handles asynchronous callbacks. It processes the call stack, then checks the microtask queue (promises), then the macrotask queue (setTimeout, setInterval).

3. **What is the difference between `process.nextTick()` and `setImmediate()`?**
   - `process.nextTick()` executes before the next iteration of the event loop, while `setImmediate()` executes in the check phase of the next event loop iteration.

4. **What are streams in Node.js?**
   - Streams are objects that allow reading/writing data in chunks rather than loading everything into memory. Types: Readable, Writable, Duplex, Transform.

5. **Explain middleware in Express**
   - Middleware functions have access to request, response, and next function. They can execute code, modify req/res objects, end the request-response cycle, or call the next middleware.

### Intermediate Questions
6. **How do you handle errors in async/await?**
   - Use try-catch blocks or create an async wrapper function that catches errors and passes them to error-handling middleware.

7. **What is the difference between PUT and PATCH?**
   - PUT replaces the entire resource, while PATCH applies partial modifications.

8. **Explain JWT authentication**
   - JWT creates a token containing user information signed with a secret. The token is sent with each request for authentication without storing session data on the server.

9. **How do you prevent SQL injection?**
   - Use parameterized queries, prepared statements, or ORM libraries that handle escaping automatically.

10. **What is CORS and how do you handle it?**
    - Cross-Origin Resource Sharing allows controlled access to resources from different origins. Handle it using the cors middleware or custom headers.

### Advanced Questions
11. **How does Node.js handle child processes?**
    - Using the `child_process` module with methods like `spawn()`, `exec()`, `execFile()`, and `fork()`.

12. **Explain connection pooling**
    - Connection pooling maintains a cache of database connections that can be reused, reducing the overhead of creating new connections.

13. **How would you scale a Node.js application?**
    - Horizontal scaling (multiple instances), clustering, load balancing, caching, database optimization, microservices architecture.

14. **What are worker threads?**
    - Worker threads enable parallel JavaScript execution on multiple CPU cores for CPU-intensive tasks without blocking the event loop.

15. **Explain the difference between clustering and worker threads**
    - Clustering creates multiple Node.js processes (separate memory), while worker threads create threads within a single process (shared memory).

### Coding Questions
16. **Implement a basic rate limiter middleware**
17. **Create a custom error handling middleware**
18. **Write a function to upload files using Multer**
19. **Implement JWT authentication from scratch**
20. **Create a REST API with CRUD operations**

---

## Additional Resources

### Official Documentation
- [Node.js Docs](https://nodejs.org/docs/)
- [Express.js Guide](https://expressjs.com/en/guide/routing.html)
- [MongoDB Manual](https://docs.mongodb.com/)

### Recommended Packages
- **express**: Web framework
- **mongoose**: MongoDB ORM
- **dotenv**: Environment variables
- **jsonwebtoken**: JWT authentication
- **bcryptjs**: Password hashing
- **helmet**: Security headers
- **cors**: Cross-origin resource sharing
- **morgan**: HTTP request logger
- **express-validator**: Input validation
- **multer**: File upload handling
- **nodemailer**: Email sending
- **socket.io**: Real-time communication
- **jest**: Testing framework
- **supertest**: HTTP assertion library

### Best Practices Checklist
- ✅ Use environment variables for sensitive data
- ✅ Implement proper error handling
- ✅ Validate and sanitize user input
- ✅ Use HTTPS in production
- ✅ Implement rate limiting
- ✅ Log important events
- ✅ Write tests for critical functionality
- ✅ Use a process manager (PM2)
- ✅ Monitor application performance
- ✅ Keep dependencies updated
- ✅ Follow RESTful conventions
- ✅ Document your API

---

## Practice Projects

1. **Blog API**: User authentication, CRUD for posts, comments
2. **E-commerce Backend**: Products, cart, orders, payment integration
3. **Social Media API**: User profiles, posts, likes, followers
4. **Task Management System**: Projects, tasks, assignments, notifications
5. **Real-time Chat Application**: WebSocket connections, rooms, messages

---

**Good luck with your Node.js backend preparation! 🚀**