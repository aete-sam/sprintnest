# SprintNest Backend Structure

## Overview

This document outlines the planned structure for the SprintNest backend, which will be built using Node.js, Express.js, and MongoDB with Mongoose ODM.

## Folder Structure

```
server/
├── config/                 # Configuration files
│   ├── db.js               # Database connection
│   ├── passport.js         # Authentication strategies
│   └── config.js           # Environment variables
│
├── controllers/            # Route controllers
│   ├── authController.js   # Authentication logic
│   ├── userController.js   # User management
│   ├── teamController.js   # Team operations
│   ├── taskController.js   # Task CRUD operations
│   └── sprintController.js # Sprint management
│
├── middleware/             # Express middleware
│   ├── auth.js             # Authentication middleware
│   ├── error.js            # Error handling
│   ├── validation.js       # Request validation
│   └── permissions.js      # Role-based access control
│
├── models/                 # Mongoose models
│   ├── User.js             # User schema
│   ├── Team.js             # Team schema
│   ├── Task.js             # Task schema
│   └── Sprint.js           # Sprint schema
│
├── routes/                 # API routes
│   ├── auth.js             # Authentication routes
│   ├── users.js            # User routes
│   ├── teams.js            # Team routes
│   ├── tasks.js            # Task routes
│   └── sprints.js          # Sprint routes
│
├── utils/                  # Utility functions
│   ├── validators.js       # Input validation
│   ├── errorResponse.js    # Error response formatter
│   └── generateCode.js     # Invite code generator
│
├── .env                    # Environment variables
├── .gitignore              # Git ignore file
├── package.json            # Dependencies
└── server.js               # Entry point
```

## API Endpoints

### Authentication

- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user
- `GET /api/auth/github` - GitHub OAuth login
- `GET /api/auth/github/callback` - GitHub OAuth callback

### Users

- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update user profile

### Teams

- `POST /api/teams` - Create a new team
- `GET /api/teams/me` - Get user's current team
- `POST /api/teams/join` - Join a team with invite code
- `GET /api/teams/members` - Get team members
- `DELETE /api/teams/members/:userId` - Remove team member (PM only)

### Tasks

- `POST /api/tasks` - Create a new task
- `GET /api/tasks` - Get all tasks for team
- `GET /api/tasks/:taskId` - Get a specific task
- `PUT /api/tasks/:taskId` - Update a task
- `DELETE /api/tasks/:taskId` - Delete a task
- `GET /api/tasks/user/:userId` - Get tasks for a specific user

### Sprints

- `POST /api/sprints` - Create a new sprint (PM only)
- `GET /api/sprints` - Get all sprints for team
- `GET /api/sprints/active` - Get active sprint
- `PUT /api/sprints/:sprintId` - Update a sprint (PM only)
- `DELETE /api/sprints/:sprintId` - Delete a sprint (PM only)
- `POST /api/sprints/:sprintId/tasks/:taskId` - Add task to sprint
- `DELETE /api/sprints/:sprintId/tasks/:taskId` - Remove task from sprint

## Database Models

### User Model

```javascript
const UserSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true
  },
  avatar: {
    type: String
  },
  team: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Team'
  },
  role: {
    type: String,
    enum: ['Project Manager', 'Team Member'],
    default: 'Team Member'
  },
  githubId: {
    type: String
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

### Team Model

```javascript
const TeamSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  inviteCode: {
    type: String,
    unique: true
  },
  owner: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  members: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'User'
    }
  ],
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

### Task Model

```javascript
const TaskSchema = new mongoose.Schema({
  title: {
    type: String,
    required: true
  },
  description: {
    type: String
  },
  status: {
    type: String,
    enum: ['To Do', 'In Progress', 'Done'],
    default: 'To Do'
  },
  assignee: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User'
  },
  team: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Team',
    required: true
  },
  sprint: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Sprint'
  },
  priority: {
    type: String,
    enum: ['Low', 'Medium', 'High'],
    default: 'Medium'
  },
  labels: [
    {
      type: String
    }
  ],
  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  },
  updatedAt: {
    type: Date,
    default: Date.now
  }
});
```

### Sprint Model

```javascript
const SprintSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  startDate: {
    type: Date,
    required: true
  },
  endDate: {
    type: Date,
    required: true
  },
  team: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Team',
    required: true
  },
  tasks: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: 'Task'
    }
  ],
  isActive: {
    type: Boolean,
    default: false
  },
  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});
```

## Authentication Flow

1. User registers or logs in
2. Server validates credentials and issues JWT
3. Client stores JWT in localStorage
4. JWT is sent with each request in Authorization header
5. Server middleware validates JWT and extracts user info

## Error Handling

All API endpoints will use a consistent error response format:

```javascript
{
  success: false,
  error: {
    message: 'Error message',
    statusCode: 400,
    errors: [] // Validation errors if applicable
  }
}
```

## Middleware

### Authentication Middleware

```javascript
const auth = async (req, res, next) => {
  try {
    // Get token from header
    const token = req.header('Authorization').replace('Bearer ', '');
    
    // Verify token
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    // Find user
    const user = await User.findById(decoded.id);
    
    if (!user) {
      throw new Error();
    }
    
    // Add user to request
    req.user = user;
    next();
  } catch (err) {
    res.status(401).json({ success: false, error: { message: 'Not authorized', statusCode: 401 } });
  }
};
```

### Permission Middleware

```javascript
const isProjectManager = (req, res, next) => {
  if (req.user.role !== 'Project Manager') {
    return res.status(403).json({ 
      success: false, 
      error: { 
        message: 'Access denied. Project Manager role required', 
        statusCode: 403 
      } 
    });
  }
  next();
};
```

## Deployment Considerations

- Set up environment variables for production
- Configure CORS for production frontend URL
- Set up MongoDB Atlas connection string
- Implement rate limiting for API endpoints
- Add security headers with Helmet.js