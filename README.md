# 🚀 JobTracker — Smart Job Application Tracker

> A full-stack web application to manage, track, and analyze your entire job search pipeline from a single, beautiful dashboard.

![React](https://img.shields.io/badge/React-18.x-61DAFB?style=for-the-badge&logo=react)
![Node.js](https://img.shields.io/badge/Node.js-18.x-339933?style=for-the-badge&logo=node.js)
![Express](https://img.shields.io/badge/Express.js-4.x-000000?style=for-the-badge&logo=express)
![MongoDB](https://img.shields.io/badge/MongoDB-6.x-47A248?style=for-the-badge&logo=mongodb)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3.x-06B6D4?style=for-the-badge&logo=tailwind-css)
![Redux](https://img.shields.io/badge/Redux_Toolkit-2.x-764ABC?style=for-the-badge&logo=redux)

---

## 📌 Problem Statement

Job seekers apply to dozens — sometimes hundreds — of companies across multiple platforms like LinkedIn, Naukri, company websites, and referrals, but have **no centralized system** to track where they applied, what stage they are at, or which companies ghosted them.

**Result:** Missed follow-ups, forgotten deadlines, duplicate applications, and zero visibility into the job search pipeline.

---

## 💡 Solution

**JobTrackr** is a full-stack personal job application management dashboard where users can:

- ✅ Log every job application in one place
- 📊 Track applications through a visual status pipeline
- 🔍 Search, filter, and sort applications instantly
- 📈 Analyze job search progress with dashboard charts
- 🌙 Switch between Dark and Light mode

**Status Pipeline:**
```
Applied → Shortlisted → Interview → Offer → Rejected → Ghosted
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | React 18, React Router v6, Tailwind CSS |
| **State Management** | Redux Toolkit + Context API |
| **Backend** | Node.js, Express.js |
| **Database** | MongoDB with Mongoose |
| **Charts** | Recharts |
| **HTTP Client** | Axios |
| **Auth Storage** | LocalStorage (JWT-like flow) |

---

## 📁 Project Structure

```
jobtrackr/
│
├── client/                             # ⚛️ React Frontend
│   ├── public/
│   └── src/
│       ├── api/
│       │   └── axiosInstance.js        # Axios base config + interceptors
│       ├── components/
│       │   ├── Navbar.jsx
│       │   ├── ApplicationCard.jsx
│       │   ├── ApplicationTable.jsx
│       │   ├── StatusBadge.jsx
│       │   ├── Pagination.jsx
│       │   ├── SearchBar.jsx           # Debounced search
│       │   ├── FilterPanel.jsx
│       │   └── ProtectedRoute.jsx
│       ├── context/
│       │   └── ThemeContext.jsx        # Dark / Light mode
│       ├── hooks/
│       │   └── useDebounce.js          # Custom debounce hook
│       ├── pages/
│       │   ├── Home.jsx
│       │   ├── Login.jsx
│       │   ├── Signup.jsx
│       │   ├── Dashboard.jsx           # Stats + Charts
│       │   ├── Applications.jsx        # Main list view
│       │   ├── AddApplication.jsx
│       │   ├── EditApplication.jsx
│       │   └── Profile.jsx
│       ├── store/
│       │   ├── store.js
│       │   ├── authSlice.js
│       │   └── applicationsSlice.js
│       ├── App.jsx
│       └── main.jsx
│
└── server/                             # 🟢 Node.js Backend
    ├── controllers/
    │   ├── auth.controller.js
    │   └── application.controller.js
    ├── middleware/
    │   └── authMiddleware.js
    ├── models/
    │   ├── User.js
    │   └── Application.js
    ├── routes/
    │   ├── auth.routes.js
    │   └── application.routes.js
    ├── .env
    └── server.js
```

---

## 🗄️ Data Models

### User
```js
{
  name:      String,   // required
  email:     String,   // required, unique
  password:  String,   // hashed
  theme:     String,   // 'light' | 'dark'
  createdAt: Date
}
```

### Application
```js
{
  user:        ObjectId,  // ref → User
  company:     String,    // required
  role:        String,    // required
  jobURL:      String,
  appliedDate: Date,      // required
  status:      String,    // Applied | Shortlisted | Interview | Offer | Rejected | Ghosted
  source:      String,    // LinkedIn | Naukri | Referral | Company Website | Other
  location:    String,
  salary:      Number,
  round:       String,    // current interview round
  notes:       String,
  createdAt:   Date
}
```

---

## 🔌 API Endpoints

### Auth Routes — `/api/auth`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/signup` | Register a new user | ❌ |
| POST | `/login` | Login and receive user data | ❌ |

### Application Routes — `/api/applications`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/` | Get all applications (paginated, filtered) | ✅ |
| POST | `/` | Create a new application | ✅ |
| PUT | `/:id` | Update an application | ✅ |
| DELETE | `/:id` | Delete an application | ✅ |
| GET | `/stats` | Get dashboard statistics | ✅ |

### Query Parameters for `GET /api/applications`

| Param | Type | Description | Default |
|-------|------|-------------|---------|
| `page` | Number | Page number | `1` |
| `limit` | Number | Results per page | `10` |
| `search` | String | Search by company or role | `""` |
| `status` | String | Filter by status | `""` |
| `source` | String | Filter by source | `""` |
| `sort` | String | Sort field (e.g., `-appliedDate`) | `-appliedDate` |

---

## ✨ Features

### 1️⃣ Routing & Navigation
Client-side routing via **React Router v6** across:
- `/` — Home / Landing
- `/login` — Login Page
- `/signup` — Signup Page
- `/dashboard` — Stats & Charts
- `/applications` — Full Application List
- `/applications/add` — Add New Application
- `/applications/edit/:id` — Edit Application
- `/profile` — User Profile & Settings

### 2️⃣ React Hooks
| Hook | Usage |
|------|-------|
| `useState` | Form inputs, modal states, filters |
| `useEffect` | Fetch applications on mount, fetch stats |
| `useRef` | Auto-focus search bar, form field focus on error |
| `useContext` | Theme context consumption |

### 3️⃣ State Management
- **Redux Toolkit** — `authSlice` (user session) + `applicationsSlice` (app list, filters)
- **Context API** — `ThemeContext` for dark/light mode

### 4️⃣ Authentication
- Signup with name, email, and password
- Login stores user data in `localStorage`
- Protected routes redirect unauthenticated users to `/login`
- Password validations: minimum 8 characters, must contain a number

### 5️⃣ Dark / Light Mode
- Tailwind `dark:` class-based theming
- Toggle in the Navbar and Profile page
- Preference persisted to `localStorage`

### 6️⃣ Search, Filter & Sort
- 🔍 Search by company name or job role
- 🎛️ Filter by Status, Source, and Location
- 🔃 Sort by Applied Date, Salary, or Company Name

### 7️⃣ Debouncing
- Custom `useDebounce(value, 400)` hook
- Applied on the search bar — delays API call by 400ms after user stops typing

### 8️⃣ Pagination
- Backend pagination with MongoDB `skip` + `limit`
- Frontend pagination UI with page numbers, Prev/Next controls
- Displays total count and current range (e.g., "Showing 1–10 of 47")

### 9️⃣ CRUD Operations
- **Create** — Add a new job application via a validated form
- **Read** — View all applications in table or card layout
- **Update** — Edit any application's details or status
- **Delete** — Remove an application with a confirmation prompt

### 🔟 API Integration
- Axios instance with base URL and auth headers
- Loading spinners during API calls
- Toast notifications on success and error
- Try/catch blocks on all API calls

### 1️⃣1️⃣ Form Validation
- Required field checks
- URL format validation for Job URL
- Date cannot be in the future
- Inline error messages per field
- Controlled components throughout

### 1️⃣2️⃣ Responsive UI
- **Mobile** — Stacked card layout
- **Tablet** — 2-column grid
- **Desktop** — Full sortable table view
- Hamburger menu for mobile navigation

### 1️⃣3️⃣ Error Handling
- Backend returns structured `{ success, message, data }` responses
- Frontend displays error toasts and inline messages
- Global 404 page for unknown routes
- Graceful empty-state UI when no data is found

---

## 📊 Dashboard Highlights

The dashboard gives a real-time overview of your job search:

- **Stats Cards** — Total Applied, Interviews Scheduled, Offers Received, Rejection Rate
- **Doughnut Chart** — Applications by Status (Recharts)
- **Bar Chart** — Applications submitted per week
- **Recent Activity** — Last 5 applications at a glance

---

## ⚙️ Getting Started

### Prerequisites
- Node.js >= 18.x
- MongoDB (local or Atlas)
- npm or yarn

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/jobtrackr.git
cd jobtrackr
```

### 2. Setup the Backend
```bash
cd server
npm install
```

Create a `.env` file inside `/server`:
```env
PORT=5000
MONGO_URI=mongodb://localhost:27017/jobtrackr
JWT_SECRET=your_super_secret_key
```

Start the server:
```bash
npm run dev
```

### 3. Setup the Frontend
```bash
cd ../client
npm install
```

Create a `.env` file inside `/client`:
```env
VITE_API_BASE_URL=http://localhost:5000/api
```

Start the frontend:
```bash
npm run dev
```

### 4. Open in Browser
```
http://localhost:5173
```

---

## 🔑 Key Code Snippets

### Custom Debounce Hook
```js
// hooks/useDebounce.js
import { useState, useEffect } from 'react';

export const useDebounce = (value, delay = 400) => {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);
  return debounced;
};
```

### MongoDB Pagination + Search (Controller)
```js
// controllers/application.controller.js
const getApplications = async (req, res) => {
  try {
    const { page = 1, limit = 10, search = '', status, sort = '-appliedDate' } = req.query;
    const query = {
      user: req.user.id,
      ...(search && {
        $or: [
          { company: { $regex: search, $options: 'i' } },
          { role:    { $regex: search, $options: 'i' } }
        ]
      }),
      ...(status && { status })
    };
    const total = await Application.countDocuments(query);
    const apps  = await Application.find(query)
      .sort(sort)
      .skip((page - 1) * limit)
      .limit(Number(limit));

    res.json({ success: true, apps, total, pages: Math.ceil(total / limit), current: Number(page) });
  } catch (err) {
    res.status(500).json({ success: false, message: err.message });
  }
};
```

### Redux Auth Slice
```js
// store/authSlice.js
import { createSlice } from '@reduxjs/toolkit';

const authSlice = createSlice({
  name: 'auth',
  initialState: {
    user: JSON.parse(localStorage.getItem('user')) || null,
    isAuthenticated: !!localStorage.getItem('user')
  },
  reducers: {
    login: (state, action) => {
      state.user = action.payload;
      state.isAuthenticated = true;
      localStorage.setItem('user', JSON.stringify(action.payload));
    },
    logout: (state) => {
      state.user = null;
      state.isAuthenticated = false;
      localStorage.removeItem('user');
    }
  }
});

export const { login, logout } = authSlice.actions;
export default authSlice.reducer;
```

### Theme Context
```jsx
// context/ThemeContext.jsx
import { createContext, useState, useEffect, useContext } from 'react';

const ThemeContext = createContext();

export const ThemeProvider = ({ children }) => {
  const [dark, setDark] = useState(() => localStorage.getItem('theme') === 'dark');

  useEffect(() => {
    document.documentElement.classList.toggle('dark', dark);
    localStorage.setItem('theme', dark ? 'dark' : 'light');
  }, [dark]);

  return (
    <ThemeContext.Provider value={{ dark, toggleTheme: () => setDark(prev => !prev) }}>
      {children}
    </ThemeContext.Provider>
  );
};

export const useTheme = () => useContext(ThemeContext);
```

### Protected Route
```jsx
// components/ProtectedRoute.jsx
import { useSelector } from 'react-redux';
import { Navigate } from 'react-router-dom';

const ProtectedRoute = ({ children }) => {
  const { isAuthenticated } = useSelector(state => state.auth);
  return isAuthenticated ? children : <Navigate to="/login" replace />;
};

export default ProtectedRoute;
```

---

## 🗺️ Feature Checklist

| Feature | Status |
|---------|--------|
| React Router Navigation | ✅ |
| useState, useEffect, useRef, useContext | ✅ |
| Redux Toolkit State Management | ✅ |
| Context API (Theme) | ✅ |
| Signup & Login with LocalStorage | ✅ |
| Protected Routes | ✅ |
| Dark / Light Mode with Persistence | ✅ |
| Search with Debouncing | ✅ |
| Filter by Status & Source | ✅ |
| Sort by Date / Salary | ✅ |
| Backend Pagination (MongoDB) | ✅ |
| Frontend Pagination UI | ✅ |
| Full CRUD Operations | ✅ |
| REST API with Express | ✅ |
| Form Validation & Error Messages | ✅ |
| Responsive UI (Mobile + Tablet + Desktop) | ✅ |
| Loading States & Toast Notifications | ✅ |
| Error Handling (Frontend + Backend) | ✅ |
| Dashboard Stats & Charts | ✅ |

---

## 🌐 Pages Overview

| Page | Route | Description |
|------|-------|-------------|
| Home | `/` | Landing page with features and CTA |
| Login | `/login` | Email + password login |
| Signup | `/signup` | Registration with validation |
| Dashboard | `/dashboard` | Stats cards + Recharts visualizations |
| Applications | `/applications` | Full list with search, filter, sort, pagination |
| Add Application | `/applications/add` | Validated form to log a new job |
| Edit Application | `/applications/edit/:id` | Update any existing application |
| Profile | `/profile` | Update user info + theme toggle |
| 404 | `*` | Custom not-found page |

---

## 👥 Team

| Name | Role |
|------|------|
| Member 1 | Frontend — Dashboard, Applications List |
| Member 2 | Frontend — Auth Pages, Forms, Routing |
| Member 3 | Backend — Auth APIs, Middleware |
| Member 4 | Backend — Application APIs, MongoDB |

---

## 📄 License

This project was built as part of the **Full Stack Hackathon Event**.
Free to use, modify, and extend for learning purposes.

---

<p align="center">Built with ❤️ for the Full Stack Hackathon</p>
