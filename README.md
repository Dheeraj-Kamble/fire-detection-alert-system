# fire-detection-alert-system
Full-stack fire detection web app: monitors a live camera feed in the browser, scores frames for fire, and triggers an audio siren and on-screen alert above 60% confidence. JWT-secured login, alerts logged to MongoDB. Built with Node.js, Express and Vite. Published as research (IJSRSET 2025, IJSRCSEIT 2026).
# 🔥 Fire Detection and Alert System

A full-stack web app that watches a live camera feed in the browser, scores sampled frames for fire, and raises an **audio siren + on-screen alert** when confidence stays above **60%**. Every alert is saved to MongoDB (timestamp, confidence score, snapshot) behind **JWT-secured login**.

> Final-year B.E. (AI & ML) project, Savitribai Phule Pune University, 2026.



## Features

- Live camera capture in the browser
- Fire confidence score per sampled frame; alert only when it crosses the 60% threshold, to reduce false alarms
- Loud audio siren + modal alert for operators, with a cooldown between alerts
- User registration and login with JWT; the dashboard and alert API are private
- Alert history stored in MongoDB with timestamp, confidence score, detection source and snapshot

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vite, JavaScript, browser camera API |
| Backend | Node.js, Express |
| Database | MongoDB (Mongoose) |
| Auth | JSON Web Tokens (JWT) |
| Detection | Heuristic scoring in the browser, with an optional ML model |

## Project Structure

```
fire-detection-alert-system/
├── backend/
│   ├── src/
│   │   ├── config/        # MongoDB connection
│   │   ├── controllers/   # auth + alert logic
│   │   ├── middleware/    # JWT auth guard
│   │   ├── models/        # User, Alert schemas
│   │   ├── routes/        # authRoutes, alertRoutes
│   │   ├── utils/
│   │   ├── app.js
│   │   └── server.js
│   └── .env.example
├── frontend/              # Vite app (landing, login, dashboard)
│   └── .env.example
├── images/                # README screenshots
├── docs/                  # papers / extra material
└── package.json           # runs backend + frontend together
```

## Getting Started

### Prerequisites
- Node.js 18+
- MongoDB running locally (or a MongoDB Atlas connection string)

### 1. Clone and install

```bash
git clone https://github.com/Dheeraj-Kamble/fire-detection-alert-system.git
cd fire-detection-alert-system
npm install
```

If the backend or frontend has its own dependencies, also run `npm install` inside `backend/` and `frontend/`.

### 2. Configure environment variables

Copy the example files and fill in your own values:

```bash
copy backend\.env.example backend\.env
copy frontend\.env.example frontend\.env
```

**backend/.env**
```
PORT=5000
MONGO_URI=mongodb://localhost:27017/fire_detection
JWT_SECRET=replace_with_a_long_random_string
```

**frontend/.env**
```
VITE_API_URL=http://localhost:5000/api
VITE_FIRE_MODEL_URL=/models/fire-detector/model.json
VITE_FIRE_THRESHOLD=0.6
VITE_DETECTION_INTERVAL_MS=800
VITE_DETECTION_COOLDOWN_MS=10000
```

| Variable | Meaning |
|---|---|
| `VITE_FIRE_THRESHOLD` | Confidence (0–1) needed to trigger an alert |
| `VITE_DETECTION_INTERVAL_MS` | How often a frame is sampled |
| `VITE_DETECTION_COOLDOWN_MS` | Minimum gap between two alerts |

### 3. Run

```bash
npm run dev
```

- Frontend: http://localhost:5173
- API: http://localhost:5000

Open the site, create an account, log in, click **Start Monitoring** and allow camera access when the browser asks.

## API

Base URL: `http://localhost:5000/api`. Alert routes require a valid JWT (`Authorization: Bearer <token>`).

| Method | Route | Purpose |
|---|---|---|
| POST | `/auth/register` | Create an account |
| POST | `/auth/login` | Log in and receive a token |
| GET | `/alerts` | List the logged-in user's alerts |
| POST | `/alerts` | Save a new alert |

<!-- CHECK: confirm the auth route names in backend/src/routes/authRoutes.js and the mount paths in backend/src/app.js -->

## Alert Data Model

```
user, timestamp, confidenceScore, snapshotUrl, source, createdAt, updatedAt
```

## Research Publications

This work was published as *Fire Detection Using Deep Learning*:

- IJSRSET, Vol 12, Issue 6 (Nov–Dec 2025), DOI: [10.32628/IJSRSET2513860](https://doi.org/10.32628/IJSRSET2513860)
- IJSRCSEIT, Vol 12, Issue 3 (May–Jun 2026), [article link](https://ijsrcseit.com/home/article/view/CSEIT26123313)

## Limitations and Future Work

- Add smoke detection
- Train and plug in a CNN / YOLOv8 model for higher accuracy
- SMS / email / mobile push notifications
- IoT integration (automatic sprinklers and alarms)
- Cloud deployment

## Author

**Dheeraj Kamble** — Data Analyst | SQL · Power BI · Python | AI/ML Engineering graduate, Pune

[LinkedIn](https://www.linkedin.com/in/your-profile) · [GitHub](https://github.com/Dheeraj-Kamble)
