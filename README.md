# Bus-Booking-App

# Bus Booking & Management System

A full-stack, enterprise-ready bus booking application. This project features a containerized microservices architecture with automated background tasks and Role-Based Access Control (RBAC).

## 🚀 Key Features
- **Real-time Booking**: Seamless seat selection and reservation logic.
- **Role-Based Access Control (RBAC)**: 
    - **User**: Search buses and book tickets.
    - **Admin**: Access to a dedicated Dashboard to manage bus schedules and view all bookings.
- **Admin Data Seeding**: Special administrative routes to seed the database with initial bus and route data.
- **Automated Notifications**: Celery workers process background email confirmations via Redis.
- **Modern Dev-X**: Utilizing `docker compose watch` for instant code synchronization.

## 🛠️ Tech Stack
- **Frontend**: React (Vite) with Type Module support.
- **Backend**: FastAPI (Python 3.12).
- **Package Manager**: [uv](https://github.com/astral-sh/uv) (for ultra-fast dependency management).
- **Database**: PostgreSQL (hosted on Neon DB).
- **Task Queue**: Celery + Redis.
- **Containerization**: Docker & Docker Compose.

## 📦 System Architecture
The application is orchestrated using four primary services:
1. **Frontend**: The React UI.
2. **Backend**: The FastAPI server handling business logic and RBAC.
3. **Redis**: The message broker for asynchronous tasks.
4. **Worker**: The Celery instance dedicated to email and background processing.



# 🚌 Bus Booking Application

This project is a full-stack bus reservation system. It leverages a modern tech stack to handle real-time bookings, background task processing for emails, and a responsive user interface.

---

## 🛠️ Setup & Installation

You can run this project using **Docker** (Recommended) or by setting up the services **Manually**.

---

### 🐳 Method 1: Run with Docker (Recommended)

This is the fastest way to orchestrate the full system including the database, cache, and workers.

#### 1. Clone the Repository

```bash
git clone <your-repository-url>
cd Bus-Booking-App
```

#### 2. Environment Configuration

Ensure the following values are set in `backend/.env`:

| Variable        | Description |
|---------------|------------|
| DATABASE_URL  | Your PostgreSQL connection string |
| REDIS_URL     | redis://redis:6379/0 |
| SECRET_KEY    | Your JWT secret key |
| MAIL_PASSWORD | Your App Password (for notifications) |

Frontend: Ensure `VITE_BACKEND_URL=http://localhost:8000` is set in your frontend configuration.

#### 3. Start All Services

From the root directory, run:

```bash
docker compose up --build
```

This spins up:

✅ Frontend (React)  
✅ Backend (FastAPI)  
✅ Redis  
✅ Celery Worker  

---

## 💻 Method 2: Manual Setup (No Docker)

### ✅ Prerequisites

- Python 3.12+ (using `uv`)
- Node.js (LTS version)
- Redis & PostgreSQL installed and running locally

---

### 🔴 Step 1: Start Redis

**Linux / Mac**
```bash
redis-server
```

**Windows**
```bash
redis-server.exe
```

---

### 🟢 Step 2: Backend Setup

```bash
cd backend
uv sync
uv run uvicorn src.main:app --reload
```

API: http://localhost:8000  
Docs: http://localhost:8000/docs  

---

### 🔵 Step 3: Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

URL: http://localhost:5173  

---

### 🟣 Step 4: Start Celery Worker

```bash
cd backend
uv run celery -A src.celery_worker.celery_app worker --loglevel=info
```

> 💡 **Windows Users:** If Celery fails, use:

```bash
uv run celery -A src.celery_worker.celery_app worker --loglevel=info --pool=solo
```

