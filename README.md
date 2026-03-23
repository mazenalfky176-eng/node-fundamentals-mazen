# node-fundamentals-mazen
Node.js workshop materials for IEEE Foundation Week. Covers core Node.js, async programming, Express.js, and a lab task for IEEE PUA committee API.
# Node.js Workshop Materials

## 1. Speaker Notes (Simplified Explanations)

### Module A: Core Node.js
node-fundamentals-mazen/
├── module-a-foundation/
│   ├── package.json
│   └── app.js
├── module-b-async/
│   ├── promises.js
│   └── async-await.js
├── module-c-express/
│   ├── package.json
│   ├── server.js
│   └── routes/
│       └── users.js
├── lab-task/
│   ├── package.json
│   └── server.js
├── .gitignore
├── README.md
└── extensions.md
{
  "name": "node-fundamentals-mazen",
  "version": "1.0.0",
  "description": "Node.js workshop code for Mazen",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "author": "Mazen Alfky",
  "license": "ISC"
}
// Built-in modules examples
const os = require('os');
const path = require('path');
const fs = require('fs');

console.log('=== OS Module ===');
console.log('Platform:', os.platform());
console.log('Free memory:', os.freemem() / 1024 / 1024, 'MB');

console.log('\n=== Path Module ===');
const filePath = path.join(__dirname, 'test.txt');
console.log('File path:', filePath);

console.log('\n=== FS Module (Synchronous) ===');
try {
  fs.writeFileSync(filePath, 'Hello from Mazen');
  const data = fs.readFileSync(filePath, 'utf8');
  console.log('File content:', data);
} catch (err) {
  console.error('Error:', err);
}

console.log('\n=== FS Module (Asynchronous) ===');
fs.writeFile(filePath, 'Async write example by Mazen', (err) => {
  if (err) throw err;
  fs.readFile(filePath, 'utf8', (err, data) => {
    if (err) throw err;
    console.log('Async file content:', data);
  });
});
const fs = require('fs').promises;

// Using Promises
fs.readFile('test.txt', 'utf8')
  .then(data => {
    console.log('Promise result:', data);
    return fs.writeFile('output.txt', data.toUpperCase());
  })
  .then(() => console.log('File written successfully by Mazen'))
  .catch(err => console.error('Error:', err));
  const fs = require('fs').promises;

async function processFile() {
  try {
    const data = await fs.readFile('test.txt', 'utf8');
    console.log('Data read:', data);
    await fs.writeFile('output.txt', data.toUpperCase());
    console.log('File written by Mazen');
  } catch (err) {
    console.error('Error:', err);
  }
}

processFile();
{
  "name": "express-server-mazen",
  "version": "1.0.0",
  "description": "Express.js server example for Mazen",
  "main": "server.js",
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "morgan": "^1.10.0",
    "cors": "^2.8.5"
  },
  "devDependencies": {
    "nodemon": "^3.0.0"
  },
  "author": "Mazen Alfky"
}
const express = require('express');
const morgan = require('morgan');
const cors = require('cors');

const app = express();
const PORT = 3000;

// Middleware
app.use(cors());
app.use(morgan('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Custom middleware
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url} - Mazen's server`);
  next();
});

// Routes
app.get('/', (req, res) => {
  res.send('Hello from Mazen\'s Express server!');
});

app.get('/api/users', (req, res) => {
  const users = [
    { id: 1, name: 'Mazen' },
    { id: 2, name: 'Ahmed' }
  ];
  res.json(users);
});

app.post('/api/users', (req, res) => {
  const newUser = req.body;
  console.log('New user added by Mazen:', newUser);
  res.status(201).json({ message: 'User created', user: newUser });
});

app.put('/api/users/:id', (req, res) => {
  const userId = req.params.id;
  const updatedData = req.body;
  res.json({ message: `User ${userId} updated by Mazen`, data: updatedData });
});

app.delete('/api/users/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ message: `User ${userId} deleted by Mazen` });
});

// Start server
app.listen(PORT, () => {
  console.log(`Mazen's server running on http://localhost:${PORT}`);
});
const express = require('express');
const router = express.Router();

let users = [
  { id: 1, name: 'Mazen' },
  { id: 2, name: 'Ahmed' }
];

router.get('/', (req, res) => {
  res.json(users);
});

router.post('/', (req, res) => {
  const newUser = { id: users.length + 1, ...req.body };
  users.push(newUser);
  res.status(201).json(newUser);
});

module.exports = router;
const userRoutes = require('./routes/users');
app.use('/api/users', userRoutes);{
  "name": "ieee-api-mazen",
  "version": "1.0.0",
  "description": "IEEE PUA committee API by Mazen",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  },
  "author": "Mazen Alfky"
}const express = require('express');
const app = express();
app.use(express.json());

// In-memory data
let members = [
  { id: 1, name: 'Mazen Alfky', role: 'Workshop Leader' },
  { id: 2, name: 'Ahmed Ali', role: 'Chair' },
  { id: 3, name: 'Sara Mahmoud', role: 'Vice Chair' }
];

// GET all members
app.get('/members', (req, res) => {
  res.json(members);
});

// GET member by ID
app.get('/members/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const member = members.find(m => m.id === id);
  if (!member) return res.status(404).json({ error: 'Member not found' });
  res.json(member);
});

// POST new member
app.post('/members', (req, res) => {
  const newMember = {
    id: members.length + 1,
    name: req.body.name,
    role: req.body.role
  };
  members.push(newMember);
  res.status(201).json(newMember);
});

// PUT (update) member
app.put('/members/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = members.findIndex(m => m.id === id);
  if (index === -1) return res.status(404).json({ error: 'Member not found' });
  members[index] = { ...members[index], ...req.body };
  res.json(members[index]);
});

// DELETE member
app.delete('/members/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const index = members.findIndex(m => m.id === id);
  if (index === -1) return res.status(404).json({ error: 'Member not found' });
  members.splice(index, 1);
  res.status(204).send();
});

const PORT = 3000;
app.listen(PORT, () => {
  console.log(`Mazen's IEEE API running on http://localhost:${PORT}`);
});
const express = require('express');
const app = express();
app.use(express.json());

app.get('/', (req, res) => res.send("Hello from Mazen's Express server!"));
app.listen(3000, () => console.log('Server running at http://localhost:3000'));
