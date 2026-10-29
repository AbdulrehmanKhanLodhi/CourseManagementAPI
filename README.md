# Course Management REST API

REST API built with **Express.js** using a **Modular MVC architecture**, with JWT authentication, Category CRUD and Course CRUD.

## Tech stack
- Node.js, Express.js
- MongoDB (Atlas) + Mongoose
- JWT (jsonwebtoken) + bcryptjs

## Project structure
```
CourseManagementAPI/
├── config/db.js                    # MongoDB connection
├── controllers/                    # Request handling logic
│   ├── authController.js
│   ├── categoryController.js
│   └── courseController.js
├── middleware/
│   ├── authMiddleware.js           # JWT verification
│   └── errorMiddleware.js          # 404 + error handler
├── models/                         # Mongoose schemas
│   ├── User.js
│   ├── Category.js
│   └── Course.js
├── routes/                         # URL -> controller mapping
│   ├── authRoutes.js
│   ├── categoryRoutes.js
│   └── courseRoutes.js
├── .env.example
├── package.json
└── server.js
```

## Setup
1. Install dependencies:
   ```
   npm install
   ```
2. Copy `.env.example` to `.env` and fill in your values:
   ```
   PORT=5000
   MONGODB_URI=your MongoDB Atlas connection string
   JWT_SECRET=a long private random string
   ```
3. Start the server:
   ```
   npm run dev      # with nodemon
   npm start        # plain node
   ```
   You should see `MongoDB Connected Successfully` and `Server running on port 5000`.

## Endpoints

### Authentication (`/api/auth`) - public
| Method | URL | Body |
|--------|-----|------|
| POST | `/api/auth/register` | `name`, `email`, `password` (min 6 chars) |
| POST | `/api/auth/login` | `email`, `password` -> returns `token` |

### Categories (`/api/categories`) - requires `Authorization: Bearer <token>`
| Method | URL | Body |
|--------|-----|------|
| POST | `/api/categories` | `name`, `description` |
| GET | `/api/categories` | - |
| GET | `/api/categories/:id` | - |
| PUT | `/api/categories/:id` | `name` and/or `description` |
| DELETE | `/api/categories/:id` | - |

### Courses (`/api/courses`) - requires `Authorization: Bearer <token>`
| Method | URL | Body |
|--------|-----|------|
| POST | `/api/courses` | `title`, `description`, `instructor`, `category` (category ID) |
| GET | `/api/courses` | - |
| GET | `/api/courses/:id` | - |
| PUT | `/api/courses/:id` | any of `title`, `description`, `instructor`, `category` |
| DELETE | `/api/courses/:id` | - |

## Status codes used
| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request (missing / invalid field) |
| 401 | Unauthorized (missing / invalid token, wrong login) |
| 404 | Route or resource not found |
| 409 | Conflict (email already registered) |
| 500 | Internal server error |
