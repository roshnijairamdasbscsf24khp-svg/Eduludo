# EduLudo — Educational Strategy Board Game

A full-stack educational board game combining strategy, luck, and learning. Roll dice, land on tiles, and answer questions to advance across a 100-tile board.

## Tech Stack

| Layer | Tech |
|-------|------|
| Frontend | React 18, Vite, Tailwind CSS, React Router |
| Backend | Node.js, Express.js, JWT |
| Database | MongoDB (Atlas or local) |

## Features

- JWT authentication (register / login / protected routes)
- 100-tile board with Quiz, Bonus, Trap, Challenge, Reward tiles
- 6 subjects with 300+ questions (Easy / Medium / Hard)
- AI opponent with turn-based logic
- XP & Level system (Beginner → Master)
- Badge system (5 badges)
- Global leaderboard
- Dashboard with stats
- Dark mode, sound effects, dice animation

---

## Quick Start

### Prerequisites
- Node.js 18+
- MongoDB Atlas account (free) or local MongoDB

---

### 1. Clone & configure server

```bash
cd server
cp .env.example .env
# Edit .env — add your MONGODB_URI and a JWT_SECRET
```

### 2. Install & seed server

```bash
cd server
npm install
npm run seed     # Seeds 300+ questions into MongoDB
npm run dev      # Starts on http://localhost:5000
```

### 3. Configure & run client

```bash
cd client
cp .env.example .env
# .env already has VITE_API_URL=http://localhost:5000/api
npm install
npm run dev      # Starts on http://localhost:5173
```

### 4. Open the app

Visit [http://localhost:5173](http://localhost:5173), register an account, pick a subject, and play!

---

## Environment Variables

### `server/.env`

| Variable | Example |
|----------|---------|
| `PORT` | `5000` |
| `MONGODB_URI` | `mongodb+srv://user:pass@cluster.mongodb.net/eduludo` |
| `JWT_SECRET` | `some_long_random_string` |
| `JWT_EXPIRES_IN` | `7d` |
| `CLIENT_URL` | `http://localhost:5173` |

### `client/.env`

| Variable | Example |
|----------|---------|
| `VITE_API_URL` | `http://localhost:5000/api` |

---

## Project Structure

```
EduLudo/
├── client/
│   ├── public/
│   │   └── favicon.svg
│   ├── src/
│   │   ├── components/
│   │   │   ├── board/
│   │   │   │   ├── GameBoard.jsx      ← 10×10 snake-pattern board
│   │   │   │   ├── GameSidebar.jsx    ← Player/AI status + game log
│   │   │   │   ├── QuizModal.jsx      ← MCQ modal with timer
│   │   │   │   └── GameResult.jsx     ← Win/lose screen
│   │   │   └── ui/
│   │   │       ├── Navbar.jsx
│   │   │       └── LoadingSpinner.jsx
│   │   ├── context/
│   │   │   ├── AuthContext.jsx
│   │   │   ├── ThemeContext.jsx
│   │   │   └── SoundContext.jsx
│   │   ├── pages/
│   │   │   ├── Home.jsx               ← Subject picker + quick stats
│   │   │   ├── Game.jsx               ← Main game logic
│   │   │   ├── Dashboard.jsx          ← Profile, stats, badges
│   │   │   ├── Leaderboard.jsx
│   │   │   ├── Login.jsx
│   │   │   └── Register.jsx
│   │   └── utils/
│   │       ├── api.js                 ← Axios instance + interceptors
│   │       └── gameHelpers.js         ← Board gen, XP, tile helpers
│   ├── index.html
│   ├── vite.config.js
│   ├── tailwind.config.js
│   └── package.json
│
└── server/
    ├── config/db.js
    ├── controllers/
    │   ├── authController.js
    │   ├── questionController.js
    │   ├── gameController.js
    │   └── leaderboardController.js
    ├── middleware/auth.js
    ├── models/
    │   ├── User.js
    │   ├── Question.js
    │   └── GameProgress.js
    ├── routes/
    │   ├── auth.js
    │   ├── questions.js
    │   ├── game.js
    │   └── leaderboard.js
    ├── data/questions.js              ← 300+ questions across 6 subjects
    ├── scripts/seed.js
    ├── index.js
    └── package.json
```

---

## API Endpoints

### Auth
| Method | Route | Auth | Description |
|--------|-------|------|-------------|
| POST | `/api/auth/register` | ❌ | Register new user |
| POST | `/api/auth/login` | ❌ | Login |
| GET | `/api/auth/profile` | ✅ | Get profile |
| PUT | `/api/auth/profile` | ✅ | Update username |

### Questions
| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/questions/subjects` | List subjects + counts |
| GET | `/api/questions/random?subject=&difficulty=` | Random question |
| GET | `/api/questions/:subject?difficulty=&limit=` | Questions by subject |

### Game
| Method | Route | Auth | Description |
|--------|-------|------|-------------|
| POST | `/api/game/save-progress` | ✅ | Save current game state |
| POST | `/api/game/complete` | ✅ | Complete game + award XP |
| GET | `/api/game/active` | ✅ | Get active game |

### Leaderboard
| Method | Route | Description |
|--------|-------|-------------|
| GET | `/api/leaderboard` | Top 50 players by XP |

---

## Game Rules

| Tile | Effect |
|------|--------|
| Quiz ❓ | Answer MCQ — correct: +10 XP & move +2 tiles; wrong: lose turn |
| Bonus 🎲 | Extra dice roll |
| Trap 💀 | Move back 3 tiles |
| Challenge ⚡ | Hard timed question (15s) — +20 XP if correct |
| Reward 🏆 | Medium question — +10 XP, no position penalty |

## XP & Levels

| Action | XP |
|--------|----|
| Correct answer (Easy/Medium) | +10 |
| Correct answer (Hard) | +20 |
| Win game | +100 |

| Level | XP Range |
|-------|----------|
| Beginner | 0–99 |
| Learner | 100–299 |
| Explorer | 300–599 |
| Scholar | 600–999 |
| Master | 1000+ |

---

## Deployment

### Frontend → Vercel
1. Push `client/` to GitHub
2. Import in [vercel.com](https://vercel.com), set root to `client`
3. Add env: `VITE_API_URL=https://your-api.onrender.com/api`

### Backend → Render
1. Create Web Service, root dir = `server`
2. Build: `npm install` · Start: `npm start`
3. Add all env vars from `.env.example`

### Database → MongoDB Atlas
1. Create free cluster at [mongodb.com/atlas](https://mongodb.com/atlas)
2. Whitelist `0.0.0.0/0`, create DB user
3. Copy URI to `MONGODB_URI`, then `npm run seed`

---

## License

MIT
