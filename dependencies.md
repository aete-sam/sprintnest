# SprintNest Dependencies

This document outlines the dependencies required for the SprintNest project, including both frontend and backend packages.

## Frontend Dependencies

### Core Dependencies

```bash
# Create React App with Vite
npm create vite@latest client -- --template react
cd client

# React and Routing
npm install react-router-dom

# UI Libraries
npm install tailwindcss postcss autoprefixer
npm install lucide-react
npm install class-variance-authority clsx tailwind-merge
npm install @radix-ui/react-slot @radix-ui/react-dialog @radix-ui/react-dropdown-menu @radix-ui/react-toast

# Drag and Drop
npm install react-dnd react-dnd-html5-backend

# Form Handling
npm install react-hook-form zod @hookform/resolvers

# HTTP Client
npm install axios

# Date Handling
npm install date-fns

# Charts
npm install recharts
```

### Development Dependencies

```bash
# TypeScript (optional)
npm install -D typescript @types/react @types/react-dom @types/node

# Tailwind Plugins
npm install -D @tailwindcss/forms

# Testing
npm install -D vitest jsdom @testing-library/react @testing-library/jest-dom
```

## Backend Dependencies

### Core Dependencies

```bash
# Create Node.js project
mkdir server && cd server
npm init -y

# Express Framework
npm install express

# MongoDB and Mongoose
npm install mongoose

# Authentication
npm install bcryptjs jsonwebtoken passport passport-jwt passport-github2

# Environment Variables
npm install dotenv

# Request Validation
npm install express-validator

# CORS and Security
npm install cors helmet

# Logging
npm install morgan winston

# File Upload
npm install multer
```

### Development Dependencies

```bash
# Development Server
npm install -D nodemon

# Testing
npm install -D jest supertest

# TypeScript (optional)
npm install -D typescript @types/node @types/express ts-node
```

## Explanation of Dependencies

### Frontend

#### Core Dependencies

- **React Router DOM**: For client-side routing
- **Tailwind CSS**: For utility-first styling
- **Lucide React**: For modern, customizable icons
- **Class Variance Authority, clsx, tailwind-merge**: For composing CSS classes conditionally
- **Radix UI**: For accessible UI primitives
- **React DnD**: For drag-and-drop functionality in the Kanban board
- **React Hook Form & Zod**: For form handling and validation
- **Axios**: For making HTTP requests to the backend API
- **date-fns**: For date manipulation and formatting
- **Recharts**: For creating charts and visualizations in the dashboard

#### Development Dependencies

- **TypeScript**: For static type checking (optional)
- **Tailwind CSS Forms**: For styling form elements
- **Vitest & Testing Library**: For unit and component testing

### Backend

#### Core Dependencies

- **Express**: Web framework for Node.js
- **Mongoose**: MongoDB object modeling tool
- **bcryptjs**: For password hashing
- **jsonwebtoken**: For JWT authentication
- **passport**: Authentication middleware for Node.js
- **passport-jwt**: Passport strategy for JWT authentication
- **passport-github2**: Passport strategy for GitHub OAuth authentication
- **dotenv**: For loading environment variables
- **express-validator**: For request validation
- **cors**: For handling Cross-Origin Resource Sharing
- **helmet**: For securing HTTP headers
- **morgan & winston**: For HTTP request logging and application logging
- **multer**: For handling file uploads

#### Development Dependencies

- **nodemon**: For automatically restarting the server during development
- **jest & supertest**: For API testing
- **TypeScript**: For static type checking (optional)

## Setup Instructions

### Frontend Setup

1. Create a new React project with Vite:
   ```bash
   npm create vite@latest client -- --template react
   cd client
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Set up Tailwind CSS:
   ```bash
   npx tailwindcss init -p
   ```

4. Configure Tailwind CSS by updating `tailwind.config.js`:
   ```javascript
   module.exports = {
     content: ['./src/**/*.{js,jsx,ts,tsx}'],
     theme: {
       extend: {
         // Custom theme extensions
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
   ```

6. Import the CSS file in your main entry file (`src/main.jsx`):
   ```jsx
   import './styles/globals.css';
   ```

### Backend Setup

1. Create a new Node.js project:
   ```bash
   mkdir server && cd server
   npm init -y
   ```

2. Install dependencies:
   ```bash
   npm install express mongoose bcryptjs jsonwebtoken passport passport-jwt passport-github2 dotenv express-validator cors helmet morgan winston multer
   ```

3. Install development dependencies:
   ```bash
   npm install -D nodemon jest supertest
   ```

4. Create a `.env` file in the server directory:
   ```
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/sprintnest
   JWT_SECRET=your_jwt_secret_key
   GITHUB_CLIENT_ID=your_github_client_id
   GITHUB_CLIENT_SECRET=your_github_client_secret
   GITHUB_CALLBACK_URL=http://localhost:5000/api/auth/github/callback
   ```

5. Update `package.json` scripts:
   ```json
   "scripts": {
     "start": "node src/server.js",
     "dev": "nodemon src/server.js",
     "test": "jest"
   }
   ```

## Environment Variables

### Frontend (.env)

```
VITE_APP_API_URL=http://localhost:5000/api
```

### Backend (.env)

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

## MongoDB Setup

1. Install MongoDB locally or use MongoDB Atlas (cloud service)

2. For local installation:
   - Download and install MongoDB from the [official website](https://www.mongodb.com/try/download/community)
   - Start the MongoDB service
   - Connect using the connection string: `mongodb://localhost:27017/sprintnest`

3. For MongoDB Atlas:
   - Create a free account on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
   - Create a new cluster
   - Configure network access and database users
   - Get your connection string and update the `MONGODB_URI` in your `.env` file

## GitHub OAuth Setup

1. Go to your GitHub account settings
2. Navigate to Developer settings > OAuth Apps > New OAuth App
3. Fill in the application details:
   - Application name: SprintNest
   - Homepage URL: http://localhost:3000
   - Authorization callback URL: http://localhost:5000/api/auth/github/callback
4. Register the application
5. Copy the Client ID and generate a new Client Secret
6. Add these credentials to your backend `.env` file