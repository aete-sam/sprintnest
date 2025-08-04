# Getting Started with SprintNest

This guide will help you set up and start developing the SprintNest project, a project management tool for agile teams with sprint planning and task management capabilities.

## Prerequisites

Before you begin, ensure you have the following installed on your system:

- **Node.js** (v16.x or later)
- **npm** (v8.x or later) or **yarn** (v1.22.x or later)
- **MongoDB** (v5.x or later) or a MongoDB Atlas account
- **Git** (v2.x or later)

## Clone the Repository

```bash
git clone https://github.com/aete-sam/sprintnest.git
cd sprintnest
```

## Project Structure

The SprintNest project follows a monorepo structure with separate directories for the frontend (client) and backend (server):

```
sprintnest/
├── client/                # Frontend React application
├── server/                # Backend Node.js application
├── .gitignore            # Git ignore file
├── README.md             # Project overview
└── package.json          # Root package.json for scripts
```

## Frontend Setup

### Setting up the React Application

1. Create the client directory and initialize a new React application with Vite:

```bash
npm create vite@latest client -- --template react
cd client
```

2. Install the required dependencies:

```bash
npm install react-router-dom axios
npm install react-dnd react-dnd-html5-backend
npm install react-hook-form zod @hookform/resolvers
npm install date-fns recharts
```

3. Set up Tailwind CSS:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

4. Configure Tailwind CSS by updating `tailwind.config.js`:

```javascript
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

5. Create or update `src/styles/globals.css`:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    @apply antialiased;
  }
  body {
    @apply bg-gray-50 text-gray-900;
  }
}
```

6. Import the CSS file in your main entry file (`src/main.jsx`):

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import './styles/globals.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

7. Create a `.env` file in the client directory:

```
VITE_APP_API_URL=http://localhost:5000/api
```

## Backend Setup

### Setting up the Node.js Server

1. Create the server directory and initialize a new Node.js application:

```bash
mkdir -p server
cd server
npm init -y
```

2. Install the required dependencies:

```bash
npm install express mongoose bcryptjs jsonwebtoken
npm install passport passport-jwt passport-github2
npm install dotenv express-validator cors helmet
npm install morgan winston multer
npm install -D nodemon
```

3. Create a `.env` file in the server directory:

```
PORT=5000
MONGODB_URI=mongodb://localhost:27017/sprintnest
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=7d
NODE_ENV=development

# GitHub OAuth
GITHUB_CLIENT_ID=your_github_client_id
GITHUB_CLIENT_SECRET=your_github_client_secret
GITHUB_CALLBACK_URL=http://localhost:5000/api/auth/github/callback
FRONTEND_URL=http://localhost:3000
```

4. Create the basic server structure:

```bash
mkdir -p src/controllers src/models src/routes src/middleware src/config src/utils
touch src/server.js
```

5. Update `package.json` scripts:

```json
"scripts": {
  "start": "node src/server.js",
  "dev": "nodemon src/server.js",
  "test": "jest"
}
```

## MongoDB Setup

### Local MongoDB Setup

1. Install MongoDB locally from the [official website](https://www.mongodb.com/try/download/community)
2. Start the MongoDB service
3. Connect using the connection string: `mongodb://localhost:27017/sprintnest`

### MongoDB Atlas Setup (Alternative)

1. Create a free account on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create a new cluster
3. Configure network access and database users
4. Get your connection string and update the `MONGODB_URI` in your `.env` file

## Running the Application

### Running the Backend

```bash
cd server
npm run dev
```

The server will start on http://localhost:5000

### Running the Frontend

```bash
cd client
npm run dev
```

The development server will start on http://localhost:3000

## Development Workflow

1. **Backend Development**:
   - Create models in `server/src/models/`
   - Create controllers in `server/src/controllers/`
   - Define routes in `server/src/routes/`
   - Implement middleware in `server/src/middleware/`

2. **Frontend Development**:
   - Create components in `client/src/components/`
   - Define pages in `client/src/pages/`
   - Implement contexts in `client/src/contexts/`
   - Create API services in `client/src/services/`

## Implementation Strategy

The SprintNest project will be implemented in phases:

### Phase 1: Project Setup & Infrastructure

- Set up the project structure
- Configure the development environment
- Set up the database connection
- Implement basic API structure

### Phase 2: Authentication & User Management

- Implement user registration and login
- Set up JWT authentication
- Implement GitHub OAuth integration
- Create user profile management

### Phase 3: Team Management

- Implement team creation and joining
- Set up team member management
- Create team settings and permissions

### Phase 4: Task Management

- Implement task creation and editing
- Set up task assignment and status tracking
- Create the Kanban board interface

### Phase 5: Sprint Management

- Implement sprint creation and planning
- Set up sprint backlog and active sprint views
- Create sprint reports and burndown charts

### Phase 6: Dashboard & UI Refinement

- Implement the dashboard with key metrics
- Refine the user interface and experience
- Add responsive design for mobile devices

### Phase 7: Testing & Deployment

- Write unit and integration tests
- Set up CI/CD pipeline
- Deploy the application to production

## Best Practices

### Code Style

- Follow the Airbnb JavaScript Style Guide
- Use ESLint and Prettier for code formatting
- Write meaningful comments and documentation

### Git Workflow

- Use feature branches for new features
- Create pull requests for code reviews
- Write descriptive commit messages

### Testing

- Write unit tests for critical functionality
- Implement integration tests for API endpoints
- Perform manual testing for UI components

## Resources

- [React Documentation](https://reactjs.org/docs/getting-started.html)
- [Express.js Documentation](https://expressjs.com/)
- [Mongoose Documentation](https://mongoosejs.com/docs/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [React DnD Documentation](https://react-dnd.github.io/react-dnd/)

## Troubleshooting

### Common Issues

1. **MongoDB Connection Issues**:
   - Ensure MongoDB is running locally or your Atlas connection string is correct
   - Check network access settings if using Atlas

2. **Node.js Version Conflicts**:
   - Use nvm (Node Version Manager) to switch between Node.js versions
   - Ensure your Node.js version is compatible with the project requirements

3. **Dependency Installation Problems**:
   - Try clearing the npm cache: `npm cache clean --force`
   - Delete the `node_modules` folder and reinstall dependencies

4. **Environment Variable Issues**:
   - Ensure all required environment variables are set in the `.env` files
   - Restart the server after changing environment variables

### Getting Help

If you encounter any issues not covered in this guide, please:

1. Check the project documentation
2. Search for similar issues in the project repository
3. Reach out to the project maintainers for assistance