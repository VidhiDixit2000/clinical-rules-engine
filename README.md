# Clinical Rules Engine

A FastAPI + React dashboard that computes a user's accessible clinical knowledge set using a graph-based permission pipeline.

The project traverses a healthcare knowledge graph using BFS from a user-specific entry point, injects Zone 2 nodes, applies five sequential permission checks, and visualizes the resulting candidate set.

---

## Features

- User-based pipeline execution
- Breadth First Search (BFS) graph traversal
- Permission compiler with O(1) lookups
- Zone 2 node injection
- Five sequential permission filters
- Candidate set generation
- Interactive dashboard
- Hierarchy (DAG) visualization
- Filter funnel visualization
- Pipeline timing metrics
- User comparison dashboard

---

## Tech Stack

### Backend
- FastAPI
- SQLAlchemy
- PostgreSQL (Supabase)

### Frontend
- React
- Vite
- Material UI
- Axios
- React Flow
- Recharts

---

## Project Structure

```
backend/
    app/
        api/
        models/
        schemas/
        services/
        main.py

frontend/
    clinical-rules-dashboard/
        src/
            components/
            hooks/
            pages/
            services/
```

---

## Setup

### 1. Clone repository

```bash
git clone https://github.com/VidhiDixit2000/clinical-rules-engine.git
cd clinical-rules-engine
```

### 2. Backend

Create a virtual environment.

```bash
python -m venv .venv
```

Activate it.

Windows

```bash
.venv\Scripts\activate
```

Install dependencies.

```bash
pip install -r backend/requirements.txt
```

Create a `.env` file inside `backend`.

Example:

```env
DATABASE_URL=postgresql+psycopg://<username>:<password>@<host>:<port>/<database_name>
```

Run the backend.

```bash
cd backend
uvicorn app.main:app --reload
```

Swagger UI:

```
http://localhost:8000/docs
```

---

### 3. Frontend

```bash
cd frontend/clinical-rules-dashboard
npm install
npm run dev
```

Frontend runs on

```
http://localhost:5173
```

---

## Deployment

### Backend on Render

- Root directory: `backend`
- Build command: `pip install -r requirements.txt`
- Start command: `uvicorn app.main:app --host 0.0.0.0 --port $PORT`
- Environment variables:
  - `DATABASE_URL` — your Supabase connection string
  - `CORS_ORIGINS` — comma-separated list of allowed origins (see below)

**DATABASE_URL must use Supabase's Session Pooler connection string, not the direct connection.** The direct endpoint is IPv6-only and Render does not support IPv6, so the app will fail to connect. The symptom is a connection timeout with nothing in the error mentioning IPv6.

The URL scheme must be `postgresql+psycopg://` — the project uses psycopg 3, and a plain `postgresql://` URL resolves to the psycopg 2 dialect, which is not installed.

**CORS_ORIGINS must be set to the Vercel URL** once the frontend is deployed. This is the step most likely to be missed: if it is left unset, the backend falls back to allowing only `http://localhost:5173` and the deployed frontend is blocked. The failure shows up as a generic network error in the browser console — nothing names CORS.

### Frontend on Vercel

- Root directory: `frontend/clinical-rules-dashboard`
- Framework: Vite
- Build command: `npm run build`
- Output directory: `dist`
- Environment variable: `VITE_API_BASE_URL` — set this to the Render URL

### Note on Render's free tier

Render's free tier spins down the service when it is idle, so the first request after a quiet period takes around 30 seconds.

## Database

The project uses PostgreSQL hosted on Supabase.

Database schema and sample data are available in

```
Supabase/Schema.sql
Supabase/seed.sql
```

---

## Pipeline Overview

1. Resolve user entry point
2. Traverse hierarchy using BFS
3. Inject Zone 2 nodes
4. Apply five sequential permission checks
5. Generate candidate set
6. Return analytics and visualization data

---

## API Endpoints

### Get Users

```
GET /users
```

### Run Candidate Pipeline

```
GET /candidate-set/{user_id}
```

---

## Visualization

- Funnel Chart
- Hierarchy DAG
- Candidate Table
- Timing Cards
- Summary Cards
- Multi-user Comparison

---

## Environment Variables

```
DATABASE_URL=postgresql+psycopg://<username>:<password>@<host>:<port>/<database_name>
```

Refer to `.env.example`.
