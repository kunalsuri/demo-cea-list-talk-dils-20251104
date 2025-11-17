---
mode: 'agent'
description: 'Build My-SAAS-APP – A Full-Stack Feature-Driven SaaS Template with React, TypeScript & Express'
---

# **Role**

Act as a **Coding LLM** with the role of an **expert full-stack engineer** building a **feature-based SaaS template** named **My-SAAS-APP**.
The system must be modern, modular, scalable, and ready for real SaaS projects.

---

# **Core Setup**

* **Stack**: React (TypeScript) + Express (TypeScript)
* **Bundler**: **Vite**
* **Routing**: **React Router**
* **Styling/UI**: TailwindCSS + [shadcn/ui](https://ui.shadcn.com/) + Lucide Icons
* **Fonts**: Inter / SF Pro Display
* **Design Language**: Card-based UI, responsive grid, smooth theme transitions
* **State Management**:

  * **React Query** → server state
  * **Zustand** → lightweight client state


---

# **Monorepo Structure**

My-SAAS-APP/
├── client/                          # React + TypeScript frontend
│   └── src/
│       ├── features/                # Feature-based structure
│       │   └── user-profile/        # User profile module
│       │       ├── components/      # Feature-specific components
│       │       ├── hooks/           # Feature-specific hooks
│       │       └── index.ts         # Entry for feature
│       ├── components/              # Shared UI components / wrappers (no naming collisions with shadcn/ui)
│       ├── pages/                   # Page-level routes
│       │   ├── Landing.tsx
│       │   ├── Login.tsx
│       │   ├── Signup.tsx
│       │   └── Dashboard.tsx
│       ├── lib/                     # Client utilities
│       │   ├── api.ts               # API calls (fetch wrappers)
│       │   └── state.ts             # Zustand/React Query setup
│       ├── App.tsx                  # Main app component
│       └── main.tsx                 # React entrypoint
│
├── server/                          # Express + TypeScript backend
│   ├── routes/                      # Route definitions
│   │   ├── auth.ts
│   │   ├── users.ts
│   │   └── sessions.ts
│   ├── controllers/                 # Route handlers/business logic
│   │   ├── authController.ts
│   │   └── userController.ts
│   ├── middleware/                  # Middleware functions
│   │   └── authMiddleware.ts
│   ├── utils/                       # Helpers
│   │   ├── fileStorage.ts           # Read/write JSON helpers
│   │   └── validation.ts
│   ├── seed/
│   │   └── seedUsers.ts             # Startup seeding logic
│   └── server.ts                    # Express server entrypoint
│
├── data/                            # File-based storage (prototype only)
│   └── users.json                   # User + session storage
│   └── sessions.json
│
├── package.json
├── tsconfig.json
└── README.md


- Use **feature-based folders** under `/client/src/features/*` for modular development.  
- Example: `/client/src/features/user-profile/` should handle all profile logic.  
- Ensure modularity so frontend and backend teams can work in parallel.

---

# **Style Guide**

### **Colors**

  - Primary: `#0070F3` (blue)  
  - Secondary: `#7928CA` (purple)  
  - Accent: `#FF0080`  
  - Dark: `#000000`  
  - Light: `#FFFFFF`  
  - Text (light): `#333333`  
  - Text (dark): `#FFFFFF`
 
### **UI/UX Patterns**
  - Fully Responsive design (desktop + mobile).  
  - Smooth transitions (dark/light mode).  
  - Reusable components that MUST use `shadcn/ui` such as cards, buttons, inputs.
  - Set up theme provider + Tailwind config before building pages

---

# Visual References
- GitHub landing pages that are clean, developer-focused (inspiration only).
- Vercel dashboards (professional, clean, modern).  

---

# **Features**

## **Landing Page**

- Clean, professional, GitHub-inspired layout
- Light/dark theme toggle using React best practices.

---

## **Dashboard**

- Must replicate the **layout & style from GitHub**.  
- Sidebar, dashboard, and other UI elements must use **shadcn/ui** components.  
- Feature-modular and extendable → Modular and future-ready for extensions based on feature-driven development (FDD).  
- Fully theme-aware → Dark/light theme supported out of the box.  

---

# **User Profile Feature**

  Located at: `/client/src/features/user-profile/`
  * Edit + save profile data
  * Sync updates to `users.json` → `"users"` array
  * Maintain full front-back separation

---

# **User Data Seeding (`/data/users.json`)**

### Requirements

* Ensure `users.json` exists at server startup
* Ensure **minimum 4 seeded users** exist
* The **Admin Demo User must always remain**
* Implement seeding in `server/seed/seedUsers.ts`
* Run seeding before the server starts listening

### Seeded Users (example)

```json
{
  "users": [
    { "id": "admin", "name": "Tim", "email": "admin@example.com", "password": "admin123", "role": "admin" },
    { "id": "user1", "name": "Alice", "email": "alice@example.com", "password": "user123", "role": "HR" },
    { "id": "user2", "name": "Leo", "email": "leo@example.com", "password": "user234", "role": "IT" },
    { "id": "manager1", "name": "Eva", "email": "eva@example.com", "password": "manager123", "role": "manager" }
  ]
}
```
*(Passwords are plaintext for prototype only.)*

---

# **Authentication System**

### **General**

* Login + signup pages.  
* Protected dashboard only accessible after login.  
* Store **users** in `users.json`
* Store **sessions** in `sessions.json`
* Use HTTP-only cookies to store `sessionId`
* Update JSON dynamically for CRUD operations.  

## **Login Page Requirements**

* Must include Fields:

  * Username/email
  * Password
  * **Login** button
  * **Demo** button (auto-login using seeded admin)
* On login:

  * Validate credentials → Read the matching user from **`users.json`**
  * If valid, create a new session entry in **`sessions.json`**
  * Set cookie with `sessionId`
  * Redirect to the protected dashboard
* Invalid credentials must show a clear UI error state.

---

#### Signup Page Requirements

* Must include:

  * Name
  * Email
  * username
  * Password
  * Confirm password
  * Signup button
* On signup:

* Validate the payload
* Assign default role `"standard"`
* Add a new user entry to **`users.json`** under `"users"` array
* Generate unique `id`
* Must not overwrite seeded accounts

---

# Session Handling (`/data/sessions.json`)

Sessions must be stored **separately** from users.

### Required Structure

```json
{
  "sessions": [
    {
      "sessionId": "example-session-id-123",
      "userId": "user1",
      "createdAt": "2025-11-17T10:00:00Z",
      "demo": false
    }
  ]
}
```
### On Login

* Append new session
* Set cookie

### On Logout

* Remove session by `sessionId`

### Demo Session

* When the **Demo** button is clicked:

  * Auto-authenticate using the admin user in `users.json`
  * Create a session inside `sessions.json` with `"demo": true`
  * Redirect to dashboard

---

# **Protected Routes**

* Dashboard + internal pages require valid session
* Validate via cookie → check `sessions.json`
* Missing/invalid session 

---

# **Goal**

Deliver a **production-grade, scalable monorepo** with modular feature folders, JSON-based storage, authentication, session handling, and a visually polished dashboard + landing page, using React + Express fully aligned with the best practices.
