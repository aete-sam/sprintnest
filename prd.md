# Product Requirements Document (PRD)  
**Product Name:** SprintNest (MVP)  
**Type:** Web Application  
**Stack:** MERN (MongoDB, Express.js, React.js, Node.js)  
**UI Libraries:**  
- Tailwind CSS (utility-first styling)  
- ShadCN UI (modern, accessible components)  
- Lucide Icons (professional open-source icon set)

## 1. Purpose

SprintNest is a lightweight Agile project management tool built for small remote software teams following the SCRUM methodology. It provides task management, sprint planning, and team collaboration tools without the overhead of complex platforms like Jira. The MVP focuses on simplicity and clarity with a clean, responsive UI.

## 2. Core Features (MVP Scope)

### 2.1 User Management

- User registration and login using email/password or GitHub OAuth
- Each user can only belong to one team/project at a time
- Roles:
  - **Project Manager**: Full access to team management and task assignment
  - **Team Member**: Can view, create, and manage own tasks

### 2.2 Team Management

- Create a team (Project Manager role assigned automatically)
- Generate a shareable invite code (e.g. `SPN-XYZ123`)
- Join a team using an invite code
- View team roster (list of members and roles)
- Project Manager can remove members from the team

### 2.3 Task Management

- Create/edit/delete tasks
- Fields:
  - Title (required)
  - Description
  - Assignee (Project Manager can assign to anyone; Members to self)
  - Status (`To Do`, `In Progress`, `Done`)
  - Optional: priority or labels
- Drag-and-drop Kanban board to change task status
- Task filtering by status and assignee

### 2.4 Sprint Management

- Create a sprint (Project Manager only)
  - Fields: name, start date, end date
  - Only one active sprint per team
- Assign existing tasks to a sprint
- Sprint view: task list with statuses for active sprint

### 2.5 Dashboard (Basic)

- Summary of:
  - Total tasks
  - Tasks per status
  - Tasks per team member
  - Current sprint name and dates

## 3. Technical Specifications

### 3.1 Stack

- Frontend: React.js with Vite
- Backend: Node.js + Express.js
- Database: MongoDB (MongoDB Atlas)
- Authentication: JWT or GitHub OAuth

### 3.2 UI Libraries

- **Tailwind CSS**: Core styling
- **ShadCN UI**: Form components, modals, buttons, dropdowns, tabs
- **Lucide Icons**: Open-source professional icon system
- **React DnD or DnD Kit**: For drag-and-drop Kanban interactions

### 3.3 API Endpoints (Sample)

- `POST /api/auth/signup`
- `POST /api/auth/login`
- `POST /api/teams/create`
- `POST /api/teams/join`
- `GET /api/teams/me`
- `POST /api/sprints/create`
- `GET /api/sprints/active`
- `POST /api/tasks/create`
- `PATCH /api/tasks/:taskId`
- `GET /api/tasks/team/:teamId`

## 4. User Roles and Permissions

| Action                       | Project Manager | Team Member |
|-----------------------------|-----------------|-------------|
| Create team                 | Yes             | No          |
| Join team                   | Yes             | Yes         |
| Remove users                | Yes             | No          |
| Create/edit/delete tasks    | Yes             | Yes (own)   |
| Assign tasks to others      | Yes             | No          |
| Create/edit/delete sprint   | Yes             | No          |
| Change task status          | Yes             | Yes (own)   |

## 5. UI Components Overview

- Auth Pages: Login, Sign Up (ShadCN Forms)
- Team Setup: Create Team / Join Team screen
- Team Dashboard: Team name, invite code, member list
- Sprint View: Current sprint tasks grouped by status
- Task Board: Kanban columns with draggable task cards
- Task Form Modal: Reusable component for create/edit
- Navigation: Sidebar or top navbar with team name and logout

## 6. Non-Functional Requirements

- Responsive design (mobile/tablet-friendly)
- Basic security (hashed passwords, JWT validation)
- API access protection via JWT middleware
- Environment-based deployment configuration
- Clean code organization and reusability
- Version control via GitHub (monorepo recommended)

## 7. Deployment Plan

- Frontend: Vercel (React app)
- Backend: Render or Railway (Node API)
- Database: MongoDB Atlas (free tier)
- CI/CD: GitHub Actions for auto-deploy on `main` branch

## 8. Out of Scope (for MVP)

- GitHub issue integration
- AI features (e.g. resource prediction, budget forecasting)
- Multi-team support per user
- Analytics charts and burndown metrics
- Notifications and real-time collaboration
- Mobile app