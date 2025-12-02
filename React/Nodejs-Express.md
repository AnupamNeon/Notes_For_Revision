# Complete Node.js + Express.js Notes
### From Basic to Advanced with Real-Life Use Cases

---

## TABLE OF CONTENTS

- [PART 1: Node.js Fundamentals](#part-1-nodejs-fundamentals)
- [PART 2: Express.js Basics](#part-2-expressjs-basics)
- [PART 3: Express.js Intermediate](#part-3-expressjs-intermediate)
- [PART 4: Database Integration](#part-4-database-integration)
- [PART 5: Authentication & Security](#part-5-authentication--security)
- [PART 6: Advanced Concepts](#part-6-advanced-concepts)
- [PART 7: Real-World Projects](#part-7-real-world-projects)
- [PART 8: Deployment & Best Practices](#part-8-deployment--best-practices)

---

## PART 1: NODE.JS FUNDAMENTALS

### 1.1 What is Node.js?

**Node.js** is a JavaScript runtime built on Chrome’s V8 engine that allows you to run JavaScript on the server side.

#### Key Characteristics
- Single-threaded (with event loop)
- Non-blocking I/O
- Event-driven architecture
- Cross-platform
- NPM — the largest package ecosystem

#### Node.js Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                     NODE.JS ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────┤
│  Your Code (JavaScript) → Node.js APIs → V8 Engine (C++)    │
│                                 ↓                           │
│                             libuv                           │
│                        (Event Loop + Thread Pool)           │
│                                 ↓                           │
│             File System ← Network I/O ← Timers              │
└─────────────────────────────────────────────────────────────┘
```

#### The Event Loop Phases

```text
┌──────────────────┐
│      timers      │ ← setTimeout, setInterval
└────────┬─────────┘
         ↓
┌────────▼─────────┐
│ pending callbacks│ ← deferred I/O callbacks
└────────┬─────────┘
         ↓
┌────────▼─────────┐
│  idle, prepare   │ ← internal use
└────────┬─────────┘
         ↓
┌────────▼─────────┐    ┌───────────────────┐
│      poll        │◄───│ incoming data,    │
└────────┬─────────┘    │ connections, etc. │
         ↓              └───────────────────┘
┌────────▼─────────┐
│      check       │ ← setImmediate()
└────────┬─────────┘
         ↓
┌────────▼─────────┐
│ close callbacks  │ ← socket.on('close')
└──────────────────┘
```

### 1.2 Setting Up Node.js

#### Installation (Recommended: nvm)

```bash
# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# Install and use Node.js LTS
nvm install 20
nvm use 20

# Verify
node --version   # v20.x.x
npm --version
```

#### Your First Node.js Program

```js
// hello.js
console.log("Hello, Node.js!");

// Run
node hello.js
```

#### REPL (Read-Eval-Print Loop)

```bash
node
> 2 + 2
4
> const name = "Node"
> `Hello ${name}`
'Hello Node'
> .exit
```

### 1.3 Node.js Module System

#### CommonJS (Default)

```js
// math.js
const add = (a, b) => a + b;
const subtract = (a, b) => a - b;

module.exports = { add, subtract };

// app.js
const { add, subtract } = require('./math');
console.log(add(5, 3));      // 8
console.log(subtract(10, 4)); // 6
```

#### ES Modules (Modern)

```js
// package.json
{
  "type": "module"
}

// math.mjs
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;
export default { add, subtract };

// app.mjs
import calc, { add } from './math.mjs';
console.log(add(5, 3));
```

#### Built-in Core Modules

```js
// path
const path = require('path');
path.join(__dirname, 'uploads', 'file.txt');
path.extname('image.png');     // .png
path.resolve('config');        // absolute path

// os
const os = require('os');
os.platform();     // 'linux', 'darwin', 'win32'
os.cpus().length;  // number of cores
os.freemem();

// url
const { URL } = require('url');
const myUrl = new URL('https://example.com:8080/path?name=john#section');
myUrl.searchParams.get('name'); // 'john'
```

### 1.4 File System (fs) Module

#### Async/Await (Recommended)

```js
const fs = require('fs').promises;

// Read
const data = await fs.readFile('file.txt', 'utf8');
console.log(data);

// Write
await fs.writeFile('output.txt', 'Hello World!');

// Append
await fs.appendFile('output.txt', '\nNew line');

// Directory
await fs.mkdir('logs', { recursive: true });
const files = await fs.readdir('logs');
await fs.rm('logs', { recursive: true, force: true });
```

#### Real-Life: Daily Log Manager

```js
const fs = require('fs').promises;
const path = require('path');

class LogManager {
  constructor(dir = 'logs') {
    this.dir = dir;
    this.init();
  }

  async init() {
    await fs.mkdir(this.dir, { recursive: true });
  }

  getFileName() {
    const date = new Date().toISOString().split('T')[0];
    return path.join(this.dir, `${date}.log`);
  }

  async log(level, message) {
    const entry = `[${new Date().toISOString()}] [${level.toUpperCase()}] ${message}\n`;
    await fs.appendFile(this.getFileName(), entry);
  }

  async info(msg) { await this.log('info', msg); }
  async error(msg) { await this.log('error', msg); }
}

// Usage
const logger = new LogManager();
logger.info('Server started');
logger.error('DB connection failed');
```

### 1.5 Events Module

```js
const EventEmitter = require('events');

class OrderSystem extends EventEmitter {
  placeOrder(order) {
    this.emit('orderPlaced', order);
  }
}

const orders = new OrderSystem();

orders.on('orderPlaced', (order) => {
  console.log(`New order: ${order.item}`);
});

orders.placeOrder({ id: 1, item: 'Laptop' });
```

### 1.6 Streams

#### Types of Streams

```text
Readable    | Writable    | Duplex         | Transform
fs.read     | fs.write    | net.Socket     | zlib.gzip
process.stdin | process.stdout | TCP          | crypto
```

#### Piping (Most Efficient)

```js
// Copy large file efficiently
fs.createReadStream('video.mp4')
  .pipe(fs.createWriteStream('copy.mp4'));
```

#### Transform Stream Example

```js
const { Transform } = require('stream');

class Uppercase extends Transform {
  _transform(chunk, encoding, callback) {
    this.push(chunk.toString().toUpperCase());
    callback();
  }
}

fs.createReadStream('input.txt')
  .pipe(new Uppercase())
  .pipe(fs.createWriteStream('output.txt'));
```

#### Real-Life: File Compression Service

```js
const { pipeline } = require('stream/promises');
const zlib = require('zlib');

await pipeline(
  fs.createReadStream('large.log'),
  zlib.createGzip(),
  fs.createWriteStream('large.log.gz')
);
```

### 1.7 Native HTTP Module

```js
const http = require('http');

const server = http.createServer((req, res) => {
  if (req.method === 'GET' && req.url === '/') {
    res.writeHead(200, { 'Content-Type': 'text/html' });
    res.end('<h1>Hello from Node.js!</h1>');
  }
});

server.listen(3000, () => console.log('Server on :3000'));
```

### 1.8 NPM & package.json

```bash
npm init -y
npm install express
npm install -D nodemon
npm install lodash@4.17.21
```

#### Essential Scripts

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js",
  "test": "jest",
  "lint": "eslint ."
}
```

---

## PART 2: EXPRESS.JS BASICS

### 2.1 What is Express.js?

Minimal, unopinionated web framework for Node.js.

#### Why Express?
- Middleware system
- Routing
- Template engine support
- Huge ecosystem

#### First Express App

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Hello Express!');
});

app.listen(3000, () => console.log('Server running on port 3000'));
```

### 2.2 Request & Response Objects

```js
app.get('/user/:id', (req, res) => {
  console.log(req.params.id);     // URL param
  console.log(req.query.name);    // ?name=john
  console.log(req.headers.authorization);
  console.log(req.ip);

  res.json({ message: 'OK' });
  res.status(201).send('Created');
  res.redirect('/login');
});
```

### 2.3 Routing

#### Route Parameters & Patterns

```js
app.get('/users/:id', handler);           // required
app.get('/products/:category?', handler); // optional
app.get('/flights/:from-:to', handler);
app.get('/api/*', handler);               // wildcard
```

#### Express Router

```js
// routes/users.js
const router = express.Router();
router.get('/', getAllUsers);
router.post('/', createUser);
module.exports = router;

// app.js
app.use('/api/users', usersRouter);
```

### 2.4 Middleware

```js
app.use(express.json());                    // body parser
app.use(express.static('public'));          // static files
app.use(morgan('dev'));                     // logger
app.use(helmet());                          // security headers
app.use(cors());                            // enable CORS
```

#### Custom Middleware

```js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

const auth = (req, res, next) => {
  if (!req.headers.authorization) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
};

app.use(logger);
app.get('/admin', auth, adminHandler);
```

### 2.5 Error Handling

```js
// 404 handler
app.use((req, res, next) => {
  next(new Error('Not Found'));
});

// Global error handler
app.use((err, req, res, next) => {
  res.status(err.status || 500).json({
    error: {
      message: err.message
    }
  });
});
```

---

## PART 3: EXPRESS.JS INTERMEDIATE

### 3.1 Serving Static Files

```js
app.use('/static', express.static(path.join(__dirname, 'public')));
app.use('/uploads', express.static('uploads'));
```

### 3.2 Template Engines

#### EJS Example

```bash
npm install ejs
```

```js
app.set('view engine', 'ejs');
app.get('/', (req, res) => {
  res.render('index', { title: 'Home', user: req.user });
});
```

### 3.3 File Upload with Multer

```bash
npm install multer
```

```js
const upload = multer({
  dest: 'uploads/',
  limits: { fileSize: 5 * 1024 * 1024 },
  fileFilter: (req, file, cb) => {
    if (!file.mimetype.startsWith('image/')) cb(new Error('Images only!'));
    cb(null, true);
  }
});

app.post('/upload', upload.single('avatar'), (req, res) => {
  res.json({ file: req.file });
});
```

### 3.4 Sessions & Cookies

```bash
npm install express-session cookie-parser
```

```js
app.use(cookieParser());
app.use(session({
  secret: 'your-secret',
  resave: false,
  saveUninitialized: false,
  cookie: { secure: true, httpOnly: true }
}));
```

### 3.5 CORS

```js
app.use(cors({
  origin: 'https://myfrontend.com',
  credentials: true
}));
```

---

## PART 4: DATABASE INTEGRATION

### 4.1 MongoDB + Mongoose

```bash
npm install mongoose
```

#### Connection

```js
await mongoose.connect(process.env.MONGODB_URI);
console.log('MongoDB Connected');
```

#### Advanced User Schema Example

```js
const userSchema = new mongoose.Schema({
  name: { type: String, required: true, trim: true },
  email: { type: String, unique: true, lowercase: true },
  password: { type: String, select: false },
  role: { type: String, enum: ['user', 'admin'], default: 'user' }
}, { timestamps: true });

// Virtual
userSchema.virtual('profile').get(function() {
  return `https://example.com/users/${this._id}`;
});

// Instance method
userSchema.methods.correctPassword = async function(pw) {
  return await bcrypt.compare(pw, this.password);
};

const User = mongoose.model('User', userSchema);
```

#### CRUD Controllers (Async/Await)

```js
exports.getAllUsers = async (req, res) => {
  try {
    const users = await User.find().select('-password');
    res.json({ status: 'success', data: { users } });
  } catch (err) {
    res.status(500).json({ status: 'error', message: err.message });
  }
};
```

---

## PART 5: AUTHENTICATION & SECURITY

### 5.1 JWT Authentication (Stateless)

#### Installation
```bash
npm install jsonwebtoken bcryptjs
```

#### User Model (with password hashing)
```js
// models/User.js
const bcrypt = require('bcryptjs');
const userSchema = new mongoose.Schema({
  name: String,
  email: { type: String, unique: true, required: true },
  password: { type: String, required: true, select: false },
  role: { type: String, enum: ['user', 'admin'], default: 'user' }
});

userSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();
  this.password = await bcrypt.hash(this.password, 12);
  next();
});

userSchema.methods.correctPassword = async function(candidate, hash) {
  return await bcrypt.compare(candidate, hash);
};

userSchema.methods.createJWT = function() {
  return jwt.sign(
    { id: this._id, role: this.role },
    process.env.JWT_SECRET,
    { expiresIn: process.env.JWT_EXPIRES_IN || '7d' }
  );
};
```

#### Auth Controller
```js
// controllers/authController.js
const User = require('../models/User');
const jwt = require('jsonwebtoken');

const signToken = (id) => jwt.sign({ id }, process.env.JWT_SECRET, {
  expiresIn: process.env.JWT_EXPIRES_IN
});

exports.register = async (req, res) => {
  const { name, email, password } = req.body;
  const user = await User.create({ name, email, password });
  
  const token = signToken(user._id);
  
  res.status(201).json({
    status: 'success',
    token,
    data: { user: { name: user.name, email: user.email } }
  });
};

exports.login = async (req, res) => {
  const { email, password } = req.body;
  
  if (!email || !password) {
    return res.status(400).json({ message: 'Please provide email and password' });
  }
  
  const user = await User.findOne({ email }).select('+password');
  if (!user || !(await user.correctPassword(password, user.password))) {
    return res.status(401).json({ message: 'Incorrect email or password' });
  }
  
  const token = signToken(user._id);
  res.json({ status: 'success', token });
};
```

#### Protect Routes Middleware
```js
exports.protect = async (req, res, next) => {
  let token;
  if (req.headers.authorization?.startsWith('Bearer')) {
    token = req.headers.authorization.split(' ')[1];
  }
  
  if (!token) return res.status(401).json({ message: 'You are not logged in' });
  
  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  const user = await User.findById(decoded.id);
  if (!user) return res.status(401).json({ message: 'User no longer exists' });
  
  req.user = user;
  next();
};

exports.restrictTo = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ message: 'You do not have permission' });
    }
    next();
  };
};
```

#### Usage
```js
app.post('/register', authController.register);
app.post('/login', authController.login);
app.get('/profile', authController.protect, (req, res) => {
  res.json({ user: req.user });
});
app.delete('/users/:id', authController.protect, authController.restrictTo('admin'), deleteUser);
```

### 5.2 Password Reset Flow

```js
// Send reset token via email
exports.forgotPassword = async (req, res) => {
  const user = await User.findOne({ email: req.body.email });
  if (!user) return res.status(404).json({ message: 'No user with that email' });
  
  const resetToken = crypto.randomBytes(32).toString('hex');
  user.passwordResetToken = crypto.createHash('sha256').update(resetToken).digest('hex');
  user.passwordResetExpires = Date.now() + 10 * 60 * 1000;
  await user.save({ validateBeforeSave: false });
  
  const resetURL = `${req.protocol}://${req.get('host')}/reset-password/${resetToken}`;
  // Send email (nodemailer, etc.)
  res.json({ message: 'Token sent to email' });
};
```

### 5.3 Rate Limiting & Brute Force Protection

```bash
npm install express-rate-limit
```

```js
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 5,
  message: 'Too many login attempts, try again later'
});

app.post('/login', loginLimiter, authController.login);
```

### 5.4 Helmet + Security Headers

```js
const helmet = require('helmet');
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "trusted-cdn.com"]
    }
  }
}));
```

---

## PART 6: ADVANCED CONCEPTS

### 6.1 Clustering (Use All CPU Cores)

```js
// server.js
const cluster = require('cluster');
const os = require('os');

if (cluster.isMaster) {
  const cpus = os.cpus().length;
  console.log(`Forking for ${cpus} CPUs`);
  for (let i = 0; i < cpus; i++) {
    cluster.fork();
  }
  
  cluster.on('exit', (worker) => {
    console.log(`Worker ${worker.process.pid} died`);
    cluster.fork();
  });
} else {
  require('./app'); // Your Express app
  app.listen(3000);
}
```

### 6.2 Worker Threads (For Heavy CPU Tasks)

```js
// heavy-task.js
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
  const worker = new Worker(__filename);
  worker.on('message', (result) => console.log(result));
} else {
  // Heavy computation
  const result = heavyFunction();
  parentPort.postMessage(result);
}
```

### 6.3 Caching Strategies

#### In-Memory (NodeCache)
```bash
npm install node-cache
```

```js
const NodeCache = require('node-cache');
const cache = new NodeCache({ stdTTL: 600 });

app.get('/products', (req, res) => {
  const cached = cache.get('all-products');
  if (cached) return res.json(cached);
  
  const products = await Product.find();
  cache.set('all-products', products);
  res.json(products);
});
```

#### Redis
```bash
npm install redis
```

```js
const redis = require('redis');
const client = redis.createClient();

app.get('/posts', async (req, res) => {
  const cached = await client.get('posts');
  if (cached) return res.json(JSON.parse(cached));
  
  const posts = await Post.find();
  await client.setEx('posts', 3600, JSON.stringify(posts));
  res.json(posts);
});
```

### 6.4 WebSockets with Socket.IO

```bash
npm install socket.io
```

```js
// server.js
const io = require('socket.io')(server);

io.on('connection', (socket) => {
  console.log('User connected:', socket.id);
  
  socket.on('sendMessage', (msg) => {
    io.emit('message', { user: socket.id, text: msg });
  });
  
  socket.on('disconnect', () => {
    console.log('User disconnected');
  });
});
```

---

## PART 7: REAL-WORLD PROJECTS

### 7.1 REST API — Blog Platform

**Features:**
- User auth (JWT)
- Posts CRUD
- Comments with nesting
- Like system
- Pagination, filtering, search

**Folder Structure:**
```
/src
  /controllers
    postController.js
    authController.js
  /routes
    postRoutes.js
  /models
    Post.js
    User.js
  /middleware
    auth.js
    errorHandler.js
  /utils
    catchAsync.js
    AppError.js
  app.js
  server.js
```

### 7.2 Real-Time Chat App (Socket.IO + MongoDB)

```js
io.on('connection', (socket) => {
  socket.on('joinRoom', ({ room, user }) => {
    socket.join(room);
    socket.to(room).emit('userJoined', user);
  });
  
  socket.on('chatMessage', (msg) => {
    const message = new Message({ room: msg.room, user: msg.user, text: msg.text });
    message.save();
    io.to(msg.room).emit('message', message);
  });
});
```

### 7.3 File Upload API with Cloudinary

```bash
npm install cloudinary multer cloudinary-storage
```

```js
const { CloudinaryStorage } = require('multer-storage-cloudinary');

const storage = new CloudinaryStorage({
  cloudinary: cloudinary.v2,
  params: {
    folder: 'uploads',
    allowed_formats: ['jpg', 'png', 'mp4'],
    transformation: [{ width: 1000, height: 1000, crop: 'limit' }]
  }
});

const upload = multer({ storage });
app.post('/upload', upload.single('file'), (req, res) => {
  res.json({ url: req.file.path });
});
```

---

## PART 8: DEPLOYMENT & BEST PRACTICES

### 8.1 Environment Variables (.env)

```env
NODE_ENV=production
PORT=3000
MONGODB_URI=mongodb+srv://user:pass@cluster0.mongodb.net/myapp
JWT_SECRET=super-secret-jwt-key-here
JWT_EXPIRES_IN=7d
```

```js
// config/env.js
require('dotenv').config();
module.exports = {
  port: process.env.PORT || 3000,
  mongoUri: process.env.MONGODB_URI,
  jwtSecret: process.env.JWT_SECRET
};
```

### 8.2 PM2 Process Manager

```bash
npm install -g pm2

# Start app
pm2 start server.js --name "my-api"

# Cluster mode
pm2 start server.js -i max --name "api-cluster"

# Monitor
pm2 monit

# Startup script
pm2 startup
pm2 save
```

### 8.3 Docker Setup

```dockerfile
# Dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - MONGODB_URI=mongodb://mongo:27017/myapp
    depends_on:
      - mongo
  mongo:
    image: mongo:7
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db
volumes:
  mongo-data:
```

### 8.4 CI/CD with GitHub Actions

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm test
      - name: Deploy to Server
        uses: appleboy/ssh-action@v0.1.10
        with:
          host: ${{ secrets.HOST }}
          username: ${{ secrets.USERNAME }}
          key: ${{ secrets.SSH_KEY }}
          script: |
            cd /var/www/myapp
            git pull
            npm install --production
            pm2 restart my-api
```

### 8.5 Logging (Winston + Daily Rotate)

```bash
npm install winston winston-daily-rotate-file
```

```js
const winston = require('winston');
const DailyRotateFile = require('winston-daily-rotate-file');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new DailyRotateFile({
      filename: 'logs/%DATE%-error.log',
      level: 'error',
      datePattern: 'YYYY-MM-DD',
      maxFiles: '30d'
    }),
    new DailyRotateFile({
      filename: 'logs/%DATE%-combined.log',
      datePattern: 'YYYY-MM-DD',
      maxFiles: '30d'
    })
  ]
});

if (process.env.NODE_ENV !== 'production') {
  logger.add(new winston.transports.Console({
    format: winston.format.simple()
  }));
}

module.exports = logger;
```

---
