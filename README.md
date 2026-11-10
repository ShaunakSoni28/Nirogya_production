# Nirogya: Healthcare Appointment and Prescription Management

A full-stack web app for booking doctor appointments and managing prescriptions. React frontend, Node.js/Express REST API, MongoDB, and JWT authentication.

<!-- TODO: add one sentence on who uses it (patients, doctors, admins) and one screenshot. -->

## Problem

Booking a doctor's appointment and keeping track of prescriptions usually means phone calls and paper. Nirogya puts both in one place, behind a login.

## Architecture

```
React client (client/)
      │  HTTP + JSON, JWT in Authorization header
      ▼
Express server (server.js)
  ├─ routes/        URL → controller mapping
  ├─ middlewares/   JWT verification before protected routes
  ├─ controllers/   request handling and business logic
  └─ models/        Mongoose schemas
      │
      ▼
MongoDB (connection set up in config/)
```

A request flows through the layers like this:

1. The user logs in. The server checks the password against the bcrypt hash and returns a signed JWT.
2. The client sends that token with every later request.
3. The auth middleware verifies the token and attaches the user to the request, or rejects it with 401.
4. The controller runs the logic and reads or writes MongoDB through a Mongoose model.

## Tech

| Layer | Technology |
|---|---|
| Frontend | React |
| API | Node.js, Express |
| Database | MongoDB, Mongoose |
| Auth | JSON Web Tokens (`jsonwebtoken`), password hashing (`bcryptjs`) |
| Utilities | `dotenv` (config), `morgan` (request logging), `moment` (dates), `colors` (console output) |
| Dev tooling | `nodemon`, `concurrently` |

## Project structure

```
client/         React frontend
config/         database connection
controllers/    route handlers
middlewares/    auth middleware
models/         Mongoose schemas
routes/         API route definitions
server.js       Express entry point
```

## Setup

Requires Node.js and a MongoDB instance (local or MongoDB Atlas).

```bash
git clone https://github.com/ShaunakSoni28/Nirogya_production.git
cd Nirogya_production
npm install
npm install --prefix client
```

Create a `.env` file in the project root:

```
PORT=8080
MONGO_URL=mongodb://127.0.0.1:27017/nirogya
JWT_SECRET=replace-with-a-long-random-string
```

<!-- TODO: confirm these variable names and the default port match what server.js and config/ actually read. -->

Never commit `.env`.

## Run

| Command | What it does |
|---|---|
| `npm run dev` | Starts the API and the React client together |
| `npm run server` | API only, auto-restarts on changes (nodemon) |
| `npm start` | API only (node) |
| `npm run client` | React client only |

## API

<!-- TODO: fill one row per route from routes/. -->

| Method | Endpoint | Auth required | Description |
|---|---|---|---|
|  |  |  |  |

## Limitations

- No automated tests yet (`npm test` is a placeholder).
- `nodemon` and `concurrently` are listed as runtime dependencies. They belong in `devDependencies`.
- Not built for real patient data: no audit logging, encryption at rest, or HIPAA controls.
- `.DS_Store` is committed and should be removed and added to `.gitignore`.

## Possible next steps

- Tests for the auth middleware and the appointment booking flow.
- Validation on request bodies (for example, with `express-validator` or `zod`).
- Prevent double-booking the same doctor time slot.
- Map appointments and prescriptions to FHIR `Appointment` and `MedicationRequest` resources for interoperability.
