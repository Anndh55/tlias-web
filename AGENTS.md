# AGENTS.md

This file provides guidance to Codex (Codex.ai/code) when working with code in this repository.

## Commands

```bash
# Start dev server (port 3000, hot-reload enabled)
npm run dev

# Production build
npm run build

# Preview production build
npm run preview

# Start via entrypoint (dev mode)
bash entrypoint.sh

# Start via entrypoint (production mode)
bash entrypoint.sh production
```

## Architecture Overview

**Vue 3 SPA** — "智学云帆" teaching management system. Composition API (`<script setup>`) throughout.

### Tech Stack
- **Framework**: Vue 3.4.29 (no TypeScript, plain JS)
- **Build**: Vite 5.3, `@vitejs/plugin-vue`
- **UI**: Element Plus (with Chinese locale), `@element-plus/icons-vue`
- **Routing**: Vue Router 4 (hash history)
- **HTTP**: Axios (custom instance with interceptors)
- **Path alias**: `@/` → `src/`

### Project Structure

```
src/
├── api/              # Axios API modules (one per domain)
│   ├── request.js    # Axios instance with token injection & error handling
│   ├── login.js      # POST /api/login
│   ├── dept.js       # CRUD /api/depts
│   ├── emp.js        # CRUD + pagination /api/emps
│   ├── clazz.js      # CRUD + pagination /api/clazzs
│   ├── student.js    # CRUD + pagination /api/students
│   └── upload.js     # POST /api/upload (multipart)
├── layout/
│   └── MainLayout.vue  # Shell: sidebar nav + topbar + router-view outlet
├── router/
│   └── index.js      # Route config + navigation guard (redirect to /login if no token)
├── views/
│   ├── Login.vue     # Login page, stores token + user to localStorage
│   ├── Home.vue      # Dashboard with aggregate stats from all 4 modules
│   ├── dept/DeptList.vue      # Department CRUD (table + dialog form)
│   ├── emp/EmpList.vue        # Employee CRUD (search, pagination, batch delete, avatar upload)
│   ├── clazz/ClazzList.vue    # Class CRUD (similar pattern)
│   └── student/StudentList.vue # Student CRUD (similar pattern)
├── assets/style.css  # Global reset + shared utility classes
├── App.vue           # Root component (<router-view />)
└── main.js           # Entry point — registers Element Plus + icons + router
```

### Key Patterns

**Auth flow**: Login → JWT token stored in `localStorage('token')` → Axios request interceptor attaches it as header `token` → 401 response clears storage and redirects to `/login`. Route guard in `router/index.js` protects all routes except `/login`.

**API response convention**: All backend responses use `{ code: 1 (success) | 0 (fail), msg: string, data: any }`. The Axios response interceptor checks `code === 0` and shows `msg` as an error message via Element Plus notification.

**View pattern**: Each management view follows the same pattern — page header with title + "add" button, optional search form, table with data, dialog for create/edit, table selection for batch operations. Pagination uses Element Plus `<el-pagination>`.

**Form validation**: Uses Element Plus form rules (`required`, `pattern`, `max`). Validation is triggered on form submit via `formRef.value.validate()`.

### API Endpoints

| Module     | Base path      | Key operations                                |
|------------|----------------|-----------------------------------------------|
| Login      | `POST /login`  | Login                                         |
| Department | `/api/depts`   | list, get, add, update, delete                |
| Employee   | `/api/emps`    | page (paginated), list, get, add, update, delete (single & batch via `?ids=`) |
| Class      | `/api/clazzs`  | page, list, get, add, update, delete          |
| Student    | `/api/students`| page, list, get, add, update, delete (batch)  |
| Upload     | `POST /api/upload` | File upload (multipart/form-data)          |
