# SprintNest Frontend Structure

## Overview

This document outlines the planned structure for the SprintNest frontend, which will be built using React.js with Tailwind CSS, ShadCN UI components, and Lucide Icons.

## Folder Structure

```
client/
├── public/                    # Static files
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
│
├── src/                       # Source code
│   ├── assets/                # Static assets
│   │   ├── images/            # Image files
│   │   └── icons/             # Custom icons
│   │
│   ├── components/            # Reusable UI components
│   │   ├── auth/              # Authentication components
│   │   │   ├── LoginForm.jsx
│   │   │   └── RegisterForm.jsx
│   │   ├── common/            # Common UI elements
│   │   │   ├── Button.jsx
│   │   │   ├── Card.jsx
│   │   │   ├── Input.jsx
│   │   │   └── Modal.jsx
│   │   ├── dashboard/         # Dashboard components
│   │   │   ├── StatCard.jsx
│   │   │   ├── TeamMembers.jsx
│   │   │   └── TaskSummary.jsx
│   │   ├── layout/            # Layout components
│   │   │   ├── Navbar.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   └── PageContainer.jsx
│   │   ├── sprints/           # Sprint components
│   │   │   ├── SprintForm.jsx
│   │   │   ├── SprintCard.jsx
│   │   │   └── SprintTaskList.jsx
│   │   ├── tasks/             # Task components
│   │   │   ├── TaskForm.jsx
│   │   │   ├── TaskCard.jsx
│   │   │   └── KanbanBoard.jsx
│   │   └── teams/             # Team components
│   │       ├── TeamForm.jsx
│   │       ├── InviteCode.jsx
│   │       └── MemberList.jsx
│   │
│   ├── contexts/              # React contexts
│   │   ├── AuthContext.jsx    # Authentication state
│   │   ├── TeamContext.jsx    # Team state
│   │   ├── TaskContext.jsx    # Task state
│   │   └── SprintContext.jsx  # Sprint state
│   │
│   ├── hooks/                 # Custom React hooks
│   │   ├── useAuth.js         # Authentication hook
│   │   ├── useTeam.js         # Team operations hook
│   │   ├── useTasks.js        # Task operations hook
│   │   └── useSprints.js      # Sprint operations hook
│   │
│   ├── lib/                   # Utility functions
│   │   ├── api.js             # API client
│   │   ├── helpers.js         # Helper functions
│   │   └── validation.js      # Form validation
│   │
│   ├── pages/                 # Page components
│   │   ├── auth/              # Authentication pages
│   │   │   ├── Login.jsx
│   │   │   └── Register.jsx
│   │   ├── dashboard/         # Dashboard pages
│   │   │   └── Dashboard.jsx
│   │   ├── sprints/           # Sprint pages
│   │   │   ├── SprintList.jsx
│   │   │   └── SprintDetail.jsx
│   │   ├── tasks/             # Task pages
│   │   │   ├── TaskBoard.jsx
│   │   │   └── TaskDetail.jsx
│   │   └── teams/             # Team pages
│   │       ├── CreateTeam.jsx
│   │       ├── JoinTeam.jsx
│   │       └── TeamSettings.jsx
│   │
│   ├── services/              # API service functions
│   │   ├── authService.js     # Authentication API calls
│   │   ├── teamService.js     # Team API calls
│   │   ├── taskService.js     # Task API calls
│   │   └── sprintService.js   # Sprint API calls
│   │
│   ├── styles/                # Global styles
│   │   └── globals.css        # Global CSS with Tailwind
│   │
│   ├── App.jsx                # Main App component
│   ├── index.jsx              # Entry point
│   └── routes.jsx             # Route definitions
│
├── .env                       # Environment variables
├── .gitignore                 # Git ignore file
├── package.json               # Dependencies
├── tailwind.config.js         # Tailwind configuration
└── vite.config.js             # Vite configuration
```

## Component Structure

### Layout Components

#### Navbar.jsx
```jsx
import { useState } from 'react';
import { Link } from 'react-router-dom';
import { useAuth } from '../../hooks/useAuth';
import { LogOut, Menu, X } from 'lucide-react';

const Navbar = () => {
  const { user, logout } = useAuth();
  const [isMenuOpen, setIsMenuOpen] = useState(false);

  return (
    <nav className="bg-white border-b border-gray-200 px-4 py-2.5">
      <div className="flex flex-wrap justify-between items-center">
        <div className="flex items-center">
          <Link to="/" className="flex items-center">
            <span className="self-center text-xl font-semibold whitespace-nowrap">SprintNest</span>
          </Link>
        </div>
        
        {user ? (
          <div className="flex items-center">
            <button
              onClick={logout}
              className="flex items-center text-gray-500 hover:text-gray-900"
            >
              <LogOut className="h-5 w-5 mr-1" />
              <span>Logout</span>
            </button>
          </div>
        ) : (
          <div className="flex items-center">
            <Link to="/login" className="text-gray-500 hover:text-gray-900 mr-4">
              Login
            </Link>
            <Link to="/register" className="text-white bg-blue-600 hover:bg-blue-700 px-4 py-2 rounded">
              Register
            </Link>
          </div>
        )}
      </div>
    </nav>
  );
};

export default Navbar;
```

#### Sidebar.jsx
```jsx
import { Link, useLocation } from 'react-router-dom';
import { useAuth } from '../../hooks/useAuth';
import { useTeam } from '../../hooks/useTeam';
import { Home, Users, CheckSquare, Calendar, Settings } from 'lucide-react';

const Sidebar = () => {
  const { user } = useAuth();
  const { team } = useTeam();
  const location = useLocation();
  
  const isActive = (path) => location.pathname === path;
  
  const navItems = [
    { name: 'Dashboard', path: '/dashboard', icon: <Home className="h-5 w-5" /> },
    { name: 'Team', path: '/team', icon: <Users className="h-5 w-5" /> },
    { name: 'Tasks', path: '/tasks', icon: <CheckSquare className="h-5 w-5" /> },
    { name: 'Sprints', path: '/sprints', icon: <Calendar className="h-5 w-5" /> },
  ];
  
  if (!user) return null;
  
  return (
    <aside className="w-64 h-screen bg-gray-50 border-r border-gray-200 fixed">
      <div className="px-4 py-5 flex flex-col h-full">
        <div className="mb-6">
          <h2 className="text-lg font-semibold">{team?.name || 'No Team'}</h2>
          {team && <p className="text-sm text-gray-500">Invite: {team.inviteCode}</p>}
        </div>
        
        <nav className="flex-1">
          <ul>
            {navItems.map((item) => (
              <li key={item.path} className="mb-1">
                <Link
                  to={item.path}
                  className={`flex items-center px-4 py-2 rounded-md ${isActive(item.path) ? 'bg-blue-100 text-blue-600' : 'text-gray-700 hover:bg-gray-100'}`}
                >
                  {item.icon}
                  <span className="ml-3">{item.name}</span>
                </Link>
              </li>
            ))}
          </ul>
        </nav>
        
        <div className="mt-auto">
          <div className="flex items-center px-4 py-3 border-t border-gray-200">
            <div className="flex-shrink-0">
              <div className="w-8 h-8 rounded-full bg-gray-300 flex items-center justify-center">
                {user.name.charAt(0)}
              </div>
            </div>
            <div className="ml-3">
              <p className="text-sm font-medium">{user.name}</p>
              <p className="text-xs text-gray-500">{user.role}</p>
            </div>
          </div>
        </div>
      </div>
    </aside>
  );
};

export default Sidebar;
```

### Authentication Components

#### LoginForm.jsx
```jsx
import { useState } from 'react';
import { useAuth } from '../../hooks/useAuth';
import { Button } from '../common/Button';
import { Input } from '../common/Input';
import { Github } from 'lucide-react';

const LoginForm = () => {
  const [formData, setFormData] = useState({
    email: '',
    password: ''
  });
  const [error, setError] = useState('');
  const { login, loginWithGithub } = useAuth();
  
  const handleChange = (e) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    setError('');
    
    try {
      await login(formData.email, formData.password);
    } catch (err) {
      setError(err.response?.data?.error?.message || 'Login failed');
    }
  };
  
  return (
    <form onSubmit={handleSubmit} className="space-y-4">
      {error && <div className="p-3 bg-red-100 text-red-700 rounded">{error}</div>}
      
      <div>
        <label htmlFor="email" className="block text-sm font-medium text-gray-700 mb-1">Email</label>
        <Input
          id="email"
          name="email"
          type="email"
          value={formData.email}
          onChange={handleChange}
          required
        />
      </div>
      
      <div>
        <label htmlFor="password" className="block text-sm font-medium text-gray-700 mb-1">Password</label>
        <Input
          id="password"
          name="password"
          type="password"
          value={formData.password}
          onChange={handleChange}
          required
        />
      </div>
      
      <Button type="submit" className="w-full">Login</Button>
      
      <div className="relative">
        <div className="absolute inset-0 flex items-center">
          <div className="w-full border-t border-gray-300"></div>
        </div>
        <div className="relative flex justify-center text-sm">
          <span className="px-2 bg-white text-gray-500">Or continue with</span>
        </div>
      </div>
      
      <Button 
        type="button" 
        variant="outline" 
        className="w-full flex items-center justify-center"
        onClick={loginWithGithub}
      >
        <Github className="h-5 w-5 mr-2" />
        GitHub
      </Button>
    </form>
  );
};

export default LoginForm;
```

### Task Components

#### KanbanBoard.jsx
```jsx
import { useState, useEffect } from 'react';
import { DndProvider, useDrag, useDrop } from 'react-dnd';
import { HTML5Backend } from 'react-dnd-html5-backend';
import { useTasks } from '../../hooks/useTasks';
import TaskCard from './TaskCard';

const Column = ({ status, tasks, onDrop }) => {
  const [{ isOver }, drop] = useDrop({
    accept: 'task',
    drop: (item) => onDrop(item.id, status),
    collect: (monitor) => ({
      isOver: !!monitor.isOver(),
    }),
  });

  const statusColors = {
    'To Do': 'bg-gray-100',
    'In Progress': 'bg-blue-50',
    'Done': 'bg-green-50',
  };

  return (
    <div
      ref={drop}
      className={`flex-1 p-4 rounded-lg ${statusColors[status]} ${isOver ? 'border-2 border-dashed border-blue-400' : ''}`}
    >
      <h3 className="font-medium mb-4">{status}</h3>
      <div className="space-y-3">
        {tasks.filter(task => task.status === status).map(task => (
          <TaskCard key={task._id} task={task} />
        ))}
      </div>
    </div>
  );
};

const DraggableTaskCard = ({ task }) => {
  const [{ isDragging }, drag] = useDrag({
    type: 'task',
    item: { id: task._id },
    collect: (monitor) => ({
      isDragging: !!monitor.isDragging(),
    }),
  });

  return (
    <div ref={drag} style={{ opacity: isDragging ? 0.5 : 1 }}>
      <TaskCard task={task} />
    </div>
  );
};

const KanbanBoard = () => {
  const { tasks, updateTask, fetchTasks } = useTasks();
  const [filteredTasks, setFilteredTasks] = useState([]);
  const [filter, setFilter] = useState({
    status: 'all',
    assignee: 'all',
  });

  useEffect(() => {
    fetchTasks();
  }, [fetchTasks]);

  useEffect(() => {
    let filtered = [...tasks];
    
    if (filter.status !== 'all') {
      filtered = filtered.filter(task => task.status === filter.status);
    }
    
    if (filter.assignee !== 'all') {
      filtered = filtered.filter(task => task.assignee === filter.assignee);
    }
    
    setFilteredTasks(filtered);
  }, [tasks, filter]);

  const handleDrop = async (taskId, newStatus) => {
    await updateTask(taskId, { status: newStatus });
  };

  const statuses = ['To Do', 'In Progress', 'Done'];

  return (
    <DndProvider backend={HTML5Backend}>
      <div className="mb-6">
        <div className="flex space-x-4">
          {/* Filters would go here */}
        </div>
      </div>
      
      <div className="flex space-x-4">
        {statuses.map(status => (
          <Column
            key={status}
            status={status}
            tasks={filteredTasks}
            onDrop={handleDrop}
          />
        ))}
      </div>
    </DndProvider>
  );
};

export default KanbanBoard;
```

## Context Structure

### AuthContext.jsx
```jsx
import { createContext, useState, useEffect } from 'react';
import { authService } from '../services/authService';

export const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [token, setToken] = useState(localStorage.getItem('token'));

  useEffect(() => {
    const loadUser = async () => {
      if (token) {
        try {
          const userData = await authService.getCurrentUser();
          setUser(userData);
        } catch (err) {
          localStorage.removeItem('token');
          setToken(null);
        }
      }
      setLoading(false);
    };

    loadUser();
  }, [token]);

  const login = async (email, password) => {
    const response = await authService.login(email, password);
    localStorage.setItem('token', response.token);
    setToken(response.token);
    setUser(response.user);
    return response.user;
  };

  const register = async (name, email, password) => {
    const response = await authService.register(name, email, password);
    localStorage.setItem('token', response.token);
    setToken(response.token);
    setUser(response.user);
    return response.user;
  };

  const loginWithGithub = () => {
    window.location.href = `${process.env.REACT_APP_API_URL}/api/auth/github`;
  };

  const logout = () => {
    localStorage.removeItem('token');
    setToken(null);
    setUser(null);
  };

  return (
    <AuthContext.Provider
      value={{
        user,
        loading,
        login,
        register,
        loginWithGithub,
        logout,
        isAuthenticated: !!user,
      }}
    >
      {children}
    </AuthContext.Provider>
  );
};
```

## API Service Structure

### api.js
```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.REACT_APP_API_URL || 'http://localhost:5000/api',
  headers: {
    'Content-Type': 'application/json',
  },
});

// Add a request interceptor to add the auth token to every request
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Add a response interceptor to handle common errors
api.interceptors.response.use(
  (response) => response.data,
  (error) => {
    // Handle session expiration
    if (error.response?.status === 401) {
      localStorage.removeItem('token');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

### taskService.js
```javascript
import api from '../lib/api';

export const taskService = {
  // Get all tasks for the user's team
  getTasks: async () => {
    return await api.get('/tasks');
  },
  
  // Get a specific task by ID
  getTask: async (taskId) => {
    return await api.get(`/tasks/${taskId}`);
  },
  
  // Create a new task
  createTask: async (taskData) => {
    return await api.post('/tasks', taskData);
  },
  
  // Update an existing task
  updateTask: async (taskId, taskData) => {
    return await api.put(`/tasks/${taskId}`, taskData);
  },
  
  // Delete a task
  deleteTask: async (taskId) => {
    return await api.delete(`/tasks/${taskId}`);
  },
  
  // Get tasks for a specific user
  getUserTasks: async (userId) => {
    return await api.get(`/tasks/user/${userId}`);
  },
};
```

## Routing Structure

### routes.jsx
```jsx
import { BrowserRouter, Routes, Route, Navigate } from 'react-router-dom';
import { useAuth } from './hooks/useAuth';

// Layout
import Layout from './components/layout/Layout';

// Auth Pages
import Login from './pages/auth/Login';
import Register from './pages/auth/Register';

// Team Pages
import CreateTeam from './pages/teams/CreateTeam';
import JoinTeam from './pages/teams/JoinTeam';
import TeamSettings from './pages/teams/TeamSettings';

// Dashboard
import Dashboard from './pages/dashboard/Dashboard';

// Task Pages
import TaskBoard from './pages/tasks/TaskBoard';
import TaskDetail from './pages/tasks/TaskDetail';

// Sprint Pages
import SprintList from './pages/sprints/SprintList';
import SprintDetail from './pages/sprints/SprintDetail';

const PrivateRoute = ({ children }) => {
  const { isAuthenticated, loading } = useAuth();
  
  if (loading) return <div>Loading...</div>;
  
  return isAuthenticated ? children : <Navigate to="/login" />;
};

const TeamRequiredRoute = ({ children }) => {
  const { user, loading } = useAuth();
  
  if (loading) return <div>Loading...</div>;
  
  if (!user) return <Navigate to="/login" />;
  
  return user.team ? children : <Navigate to="/create-team" />;
};

const AppRoutes = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public Routes */}
        <Route path="/login" element={<Login />} />
        <Route path="/register" element={<Register />} />
        
        {/* Private Routes */}
        <Route path="/" element={
          <PrivateRoute>
            <Layout />
          </PrivateRoute>
        }>
          {/* Team Setup */}
          <Route path="create-team" element={<CreateTeam />} />
          <Route path="join-team" element={<JoinTeam />} />
          
          {/* Team Required Routes */}
          <Route path="dashboard" element={
            <TeamRequiredRoute>
              <Dashboard />
            </TeamRequiredRoute>
          } />
          
          <Route path="team" element={
            <TeamRequiredRoute>
              <TeamSettings />
            </TeamRequiredRoute>
          } />
          
          <Route path="tasks" element={
            <TeamRequiredRoute>
              <TaskBoard />
            </TeamRequiredRoute>
          } />
          
          <Route path="tasks/:taskId" element={
            <TeamRequiredRoute>
              <TaskDetail />
            </TeamRequiredRoute>
          } />
          
          <Route path="sprints" element={
            <TeamRequiredRoute>
              <SprintList />
            </TeamRequiredRoute>
          } />
          
          <Route path="sprints/:sprintId" element={
            <TeamRequiredRoute>
              <SprintDetail />
            </TeamRequiredRoute>
          } />
          
          {/* Default redirect */}
          <Route path="*" element={<Navigate to="/dashboard" replace />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
};

export default AppRoutes;
```

## UI Component Library Integration

### ShadCN UI Integration

ShadCN UI components will be used for form elements, modals, dropdowns, and other UI components. These components will be customized to match the SprintNest design system.

### Tailwind CSS Configuration

```javascript
// tailwind.config.js
module.exports = {
  content: ['./src/**/*.{js,jsx,ts,tsx}'],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#f0f9ff',
          100: '#e0f2fe',
          200: '#bae6fd',
          300: '#7dd3fc',
          400: '#38bdf8',
          500: '#0ea5e9',
          600: '#0284c7',
          700: '#0369a1',
          800: '#075985',
          900: '#0c4a6e',
        },
      },
      fontFamily: {
        sans: ['Inter', 'sans-serif'],
      },
    },
  },
  plugins: [require('@tailwindcss/forms')],
};
```

## Deployment Configuration

### Vercel Configuration

```json
// vercel.json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/static-build",
      "config": { "distDir": "build" }
    }
  ],
  "routes": [
    { "src": "/static/(.*)", "dest": "/static/$1" },
    { "src": "/favicon.ico", "dest": "/favicon.ico" },
    { "src": "/manifest.json", "dest": "/manifest.json" },
    { "src": "/logo192.png", "dest": "/logo192.png" },
    { "src": "/(.*)", "dest": "/index.html" }
  ],
  "env": {
    "REACT_APP_API_URL": "https://api.sprintnest.com"
  }
}
```

## Responsive Design Approach

The SprintNest UI will be designed with a mobile-first approach using Tailwind CSS responsive classes. Key responsive design principles include:

1. **Flexible layouts**: Using Tailwind's grid and flex utilities for responsive layouts
2. **Responsive navigation**: Collapsible sidebar on mobile devices
3. **Adaptive components**: Components that adjust their layout based on screen size
4. **Touch-friendly UI**: Larger touch targets on mobile devices
5. **Responsive typography**: Text sizes that adjust based on screen size

## Performance Optimization

1. **Code splitting**: Using React.lazy and Suspense for component-level code splitting
2. **Memoization**: Using React.memo, useMemo, and useCallback to prevent unnecessary re-renders
3. **Image optimization**: Optimizing images for different screen sizes
4. **Bundle size optimization**: Analyzing and reducing bundle size
5. **Lazy loading**: Implementing lazy loading for images and components