# Fire Detection and Alert System

A full-stack fire monitoring app with MongoDB-backed authentication, a private dashboard, live camera capture, configurable fire detection, alert popups, alarm playback, and alert history.

## Folder Structure

```text
fire-detection-alert-system/
  backend/
    src/
      config/
      controllers/
      middleware/
      models/
      routes/
      utils/
      app.js
      server.js
  frontend/
    public/
    src/
      api/
      components/
      context/
      hooks/
      pages/
      App.jsx
      main.jsx
      styles.css
  package.json
  README.md
```

## Tech Stack

- MongoDB: `mongodb://localhost:27017/fire_detection`
- Backend: Node.js + Express + Mongoose + JWT
- Frontend: React + Vite
- Fire detection: TensorFlow.js model loader with a built-in heuristic fallback for demo mode

## Setup

1. Copy the example environment files into real `.env` files.
2. Start MongoDB locally.
3. Install dependencies from the repository root.
4. Run the backend and frontend.

### Backend `.env`

```bash
MONGO_URI=mongodb://localhost:27017/fire_detection
JWT_SECRET=replace_this_with_a_long_random_secret
JWT_EXPIRES_IN=7d
PORT=5000
CORS_ORIGIN=http://localhost:5173
```

### Frontend `.env`

```bash
VITE_API_URL=http://localhost:5000/api
VITE_FIRE_MODEL_URL=/models/fire-detector/model.json
VITE_FIRE_THRESHOLD=0.6
VITE_DETECTION_INTERVAL_MS=800
VITE_DETECTION_COOLDOWN_MS=10000
```

## Run

```bash
npm install
npm run dev:backend
npm run dev:frontend
```

If you prefer one command, install the root dev dependency and use:

```bash
npm run dev
```

## Alarm Sound

Place a sound file at `frontend/public/alarm-sound.mp3`.
The dashboard plays it with the browser `Audio` API when confidence crosses the threshold.

## Fire Detection Logic

The frontend captures frames from the live camera, resizes them, and sends them into a TensorFlow.js model when a model URL is configured. If no custom fire model is present, the app falls back to a simple heuristic so the UI remains usable for local testing.

When confidence rises above the threshold:

- A fire alert modal appears.
- The alarm sound plays immediately.
- The event is posted to MongoDB through the backend alerts API.
- The alert history updates in the dashboard.

## Model Integration

For production, replace the fallback heuristic with a custom trained model that outputs a single fire-confidence score between 0 and 1.
Put the exported TensorFlow.js files under `frontend/public/models/fire-detector/` or point `VITE_FIRE_MODEL_URL` at a hosted `model.json`.

## Notes

- User credentials are stored in MongoDB with hashed passwords.
- JWT is used to protect the dashboard and alert routes.
- The alert history stores the timestamp, confidence score, and optional snapshot data.
