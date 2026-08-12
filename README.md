# Library Management System — Book Records Module

## Student Information
- **Name:** Fiel Mark Oronos
- **Section:** BSCS 3A
- **Subject:** Software Engineering 1
- **Module:** Module 7 — Design and Implementation
- **Instructor:** Patrick Jason L. Torres

---

## System Description
The Library Management System is a web-based frontend prototype that allows a school library to manage its book catalog digitally. This Module 7 prototype implements the **Book Records** entity selected from the Module 6 architectural design, replacing manual logbook processes with a responsive, interactive interface.

## Selected Module 6 Entity
**Entity:** Book Records  
**Fields:** Title, Author, Category, Status (Available / Borrowed), Borrowed By

---

## Implemented Features
1. **Add** a new book record with full form validation
2. **View** all records in a responsive table (desktop) and card layout (mobile)
3. **Edit** an existing record and save changes
4. **Delete** a record with a confirmation dialog
5. **Search** and filter records by title, author, or category
6. **Validation** — required fields are enforced before submission
7. **Persistence** — records survive page refresh via localStorage
8. **Summary** — live count of total, available, and borrowed books
9. **Feedback** — success and error messages after every action
10. **Responsive design** — usable on desktop and mobile screens

---

## Technologies Used
| Technology | Purpose |
|---|---|
| Vue.js 3 (Composition API) | Frontend framework and reactive components |
| Vite | Project scaffolding and build tool |
| Tailwind CSS v4 | Utility-first responsive styling |
| JavaScript (ES6+) | Application logic and CRUD functions |
| localStorage | Browser-based prototype data persistence |
| Git & GitHub | Version control and repository hosting |
| GitHub Actions | Automated production-build CI check |

---

## Vue Components
| Component | Responsibility |
|---|---|
| `App.vue` | Root component; owns state, CRUD logic, and coordinates children |
| `AppHeader.vue` | Displays the system title and navigation bar |
| `AppFooter.vue` | Displays student name, section, and module info |
| `BookForm.vue` | Handles add and edit form with validation |
| `BookList.vue` | Displays book records in a table (desktop) and cards (mobile) |

---

## Installation and Run Instructions

```bash
# 1. Clone the repository
git clone https://github.com/Fiel-Mark15/oronos-module7-vue-system.git
cd oronos-module7-vue-system

# 2. Install dependencies
npm install

# 3. Run the development server
npm run dev
# Open http://localhost:5173 in your browser

# 4. Create a production build
npm run build
```

---

## localStorage Explanation
This prototype uses the browser's built-in `localStorage` API to persist book records between page refreshes. When a record is added, updated, or deleted, the entire records array is serialized to JSON and saved under the key `module7-records`. On page load, `onMounted()` reads and parses this value to restore the saved state. localStorage stores data only in the same browser profile and device — it is not a server database and does not support multiple users or shared access.

---

## Connection Between Module 6 and Module 7

| Module 6 Architectural Design | Module 7 Implementation |
|---|---|
| Presentation layer (Vue.js) | Vue 3 SFCs with Composition API |
| System modules and entities | Book Records component set |
| User interactions | Forms, buttons, search, validation |
| Application logic (Node.js / Express) | JavaScript CRUD functions in App.vue |
| Data layer (MongoDB Atlas) | Simulated with browser localStorage |
| Backend and API | Future implementation boundary |

---

## Known Limitations
- Data is stored only in the browser — clearing browser data removes all records.
- No user authentication or role-based access (librarian vs. student).
- No backend API or real database connection (planned for future modules).
- localStorage is not shared across devices or browsers.

## Proposed Future Improvements
- Connect to a Node.js + Express backend with MongoDB Atlas (Module 8+).
- Add login for librarians and students with role-based views.
- Implement overdue tracking and email notifications.
- Add pagination for large book collections.
# Tailwind configured
# localStorage enabled
