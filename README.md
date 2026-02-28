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



---
🛠️ Setup & Installation

This project can be run using Docker (Recommended) or manually without Docker.

🐳 Method 1: Run with Docker (Recommended)

This is the fastest and easiest way to start the full system.

📁 1. Clone the Repository
git clone <your-repository-url>
cd Bus-Booking-App
🔐 2. Environment Configuration

The required environment file is already included:

backend/.env

It contains the necessary configuration:

DATABASE_URL=your_postgresql_url
REDIS_URL=redis://redis:6379/0
SECRET_KEY=your_jwt_secret_key
MAIL_USERNAME=your_email@gmail.com
MAIL_PASSWORD=your_app_password
ADMIN_EMAIL=admin@example.com

Frontend environment (if applicable):

VITE_BACKEND_URL=http://localhost:8000
▶️ 3. Start All Services

From the root directory:

docker compose up --build

This will automatically start:

✅ Frontend (React + Vite)

✅ Backend (FastAPI)

✅ Redis (Message Broker)

✅ Celery Worker (Background Tasks)

🌐 Application URLs

Frontend → http://localhost:5173

Backend → http://localhost:8000

API Docs → http://localhost:8000/docs

💻 Method 2: Run Without Docker (Manual Setup)

If you prefer running each service manually, follow these steps.

✅ Prerequisites

Make sure you have installed:

Python 3.12

Node.js (LTS)

Redis (installed locally)

PostgreSQL database

🔴 Step 1: Start Redis

Start Redis locally:

redis-server

On Windows:

redis-server.exe

Redis must be running before starting Celery.

🟢 Step 2: Start Backend

Open Terminal 1:

cd backend
uv sync
uv run uvicorn src.main:app --reload

Backend will run at:

http://localhost:8000
🔵 Step 3: Start Frontend

Open Terminal 2:

cd frontend
npm install
npm run dev

Frontend will run at:

http://localhost:5173
🟣 Step 4: Start Celery Worker

Open Terminal 3:

cd backend
uv run celery -A src.celery_worker.celery_app worker --loglevel=info
⚠️ If You Encounter Issues (Especially on Windows)

Run Celery using solo mode:

uv run celery -A src.celery_worker.celery_app worker --loglevel=info --pool=solo

--pool=solo avoids multiprocessing issues and is recommended for development environments.

