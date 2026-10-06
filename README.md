# Todo App (React + Node + Express + MongoDB)

A full-stack todo app. The frontend is React (Vite), the backend is Node + Express, and data is stored in MongoDB Atlas.

## Live Demo

**https://todo-app-gnits-qm6b.onrender.com**

> Hosted on Render's free plan, so the first load after a period of inactivity can take about 30 seconds while the service wakes up.

## Features

- Add, edit, complete and delete todos
- Double-click a todo title to edit it
- Filter by All tasks, Active or Completed
- Progress ring showing how many tasks are done
- Clear all completed todos in one click
- Todos are saved in MongoDB Atlas and stay after a refresh
- Input validation and clear error messages on both client and server

## Tech Stack

| Layer      | Technology                     |
| ---------- | ------------------------------ |
| Frontend   | React 19, Vite                 |
| Backend    | Node.js, Express 5             |
| Database   | MongoDB Atlas, Mongoose        |
| Deployment | Render (single web service)    |

## Goals

By the end, your app should:

1. **Be complete.** The missing pieces in the server and client are filled in, and every API route creates, reads, updates or deletes todos correctly and returns the right response and status code.
2. **Run locally.** The frontend at http://localhost:5173 talks to your backend and saves todos to MongoDB Atlas.
3. **Run on Render.** The same app is deployed as a single Render service, reachable at a public URL.

## Folder structure

```
todo-app-gnits/
├── client/                  # React (Vite)
│   ├── src/
│   │   ├── components/
│   │   │   ├── TodoForm.jsx
│   │   │   └── TodoItem.jsx
│   │   ├── api.js           # all fetch calls
│   │   ├── App.jsx
│   │   ├── index.css
│   │   └── main.jsx
│   ├── index.html
│   └── vite.config.js       # proxies /api to port 5001 in dev
├── server/                  # Node + Express
│   ├── controllers/
│   │   └── todoController.js
│   ├── models/
│   │   └── Todo.js
│   ├── routes/
│   │   └── todoRoutes.js
│   ├── .env.example
│   └── index.js
└── package.json             # root scripts
```

## Step 1: Set up the project

1. Copy `server/.env.example` to `server/.env` and put in your MongoDB Atlas URL.
2. Install everything:
   ```bash
   npm install
   npm install --prefix server
   npm install --prefix client
   ```

## Step 2: Find and complete the missing pieces

> ⚠️ **The app is not finished yet.** A couple of pieces are missing or incorrect in both the **server** and the **client**. Study the code, find what's missing, and complete it so the app works end to end.

- **Server:** Some functions in `server/controllers/todoController.js` are empty or incorrect. Finish and fix them so every route below works as described, and make sure each one returns relevant responses with the appropriate status codes (e.g. `200`, `201`, `400`, `404`, `500`).
- **Client:** Some parts of the React app don't do their job yet. Follow the flow from the UI to `api.js` and back, and fill in what's missing. Look for `TODO` comments as a starting point, but not every gap is marked.

| Method | Route            | What it does      |
| ------ | ---------------- | ----------------- |
| GET    | /api/todos       | Get all todos     |
| POST   | /api/todos       | Create a todo     |
| PUT    | /api/todos/:id   | Update a todo     |
| DELETE | /api/todos/:id   | Delete a todo     |

## Step 3: Run locally

1. Start both frontend and backend:
   ```bash
   npm run dev
   ```
2. Open http://localhost:5173 and check that you can add, edit, complete and delete todos, and that they are still there after a page refresh.

> The API runs on port 5001 (set by `PORT` in `server/.env`). Port 5000 is avoided because macOS AirPlay Receiver uses it. If you change `PORT`, update the proxy target in `client/vite.config.js` too.

## Step 4: Deploy on Render (one service)

1. Push your code to GitHub.
2. On Render, create a new **Web Service** from your repo with these settings:
   - Build command: `npm run build`
   - Start command: `npm start`
   - Environment variable: `MONGO_URI` = your Atlas URL
3. In Atlas → Network Access, allow `0.0.0.0/0` so Render can connect.
4. Once the deploy finishes, open your Render URL and test the app the same way you did locally.

## API Reference

Base URL: `/api/todos`

| Method | Route          | Body                                  | Success                | Errors                          |
| ------ | -------------- | ------------------------------------- | ---------------------- | ------------------------------- |
| GET    | /api/todos     | none                                  | `200` array of todos   | `500`                           |
| POST   | /api/todos     | `{ "title": "Buy milk" }`             | `201` created todo     | `400` missing title, `500`      |
| PUT    | /api/todos/:id | `{ "title": "..." }` or `{ "completed": true }` | `200` updated todo | `400` invalid id or empty title, `404` not found, `500` |
| DELETE | /api/todos/:id | none                                  | `200` `{ "message": "Todo deleted" }` | `400` invalid id, `404` not found, `500` |

Example todo:

```json
{
  "_id": "6523f1c2a1b2c3d4e5f6a7b8",
  "title": "Buy milk",
  "completed": false,
  "createdAt": "2026-10-06T08:15:52.000Z",
  "updatedAt": "2026-10-06T08:15:52.000Z"
}
```

## Environment Variables

Create `server/.env` (copy it from `server/.env.example`):

| Variable    | Description                                   |
| ----------- | --------------------------------------------- |
| `MONGO_URI` | Your MongoDB Atlas connection string          |
| `PORT`      | Server port, `5001` locally (Render sets its own) |

`.env` is listed in `.gitignore` and must never be committed.

## Available Scripts

Run these from the project root:

| Command         | What it does                                          |
| --------------- | ----------------------------------------------------- |
| `npm run dev`   | Starts the server and the Vite client together        |
| `npm run build` | Installs dependencies and builds the React app        |
| `npm start`     | Starts the Express server, which also serves the build |

