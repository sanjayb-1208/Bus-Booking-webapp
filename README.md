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



🛠️ Setup & InstallationYou can run this project using Docker (Recommended) or by setting up the services Manually.🐳 Method 1: Run with Docker (Recommended)This is the fastest way to orchestrate the full system including the database, cache, and workers.1. Clone the RepositoryBashgit clone <your-repository-url>
cd Bus-Booking-App
2. Environment ConfigurationThe project uses pre-configured environment files. Ensure the following values are set in backend/.env:VariableDescriptionDATABASE_URLYour PostgreSQL connection stringREDIS_URLredis://redis:6379/0 (Internal Docker DNS)SECRET_KEYYour JWT secret keyMAIL_PASSWORDYour App Password (for notifications)Frontend Environment:Ensure VITE_BACKEND_URL=http://localhost:8000 is set in your frontend config.3. Start All ServicesFrom the root directory, run:Bashdocker compose up --build
This command spins up:✅ Frontend: React + Vite✅ Backend: FastAPI✅ Redis: Message Broker✅ Celery Worker: Background Tasks💻 Method 2: Manual Setup (No Docker)Use this method if you want to develop individual components locally.✅ PrerequisitesPython: 3.12+ (using uv package manager)Node.js: LTS versionRedis: Installed and running locallyPostgreSQL: Active database instance🔴 Step 1: Start RedisRedis must be running for Celery to manage the task queue.Linux/Mac: redis-serverWindows: redis-server.exe🟢 Step 2: Backend SetupOpen a new terminal:Bashcd backend
uv sync
uv run uvicorn src.main:app --reload
API URL: http://localhost:8000Interactive Docs: http://localhost:8000/docs🔵 Step 3: Frontend SetupOpen a second terminal:Bashcd frontend
npm install
npm run dev
App URL: http://localhost:5173🟣 Step 4: Start Celery WorkerOpen a third terminal. This handles background processes like email confirmations.Bashcd backend
uv run celery -A src.celery_worker.celery_app worker --loglevel=info
[!TIP]Windows Users: If you encounter multiprocessing errors with Celery, use the solo pool:uv run celery -A src.celery_worker.celery_app worker --loglevel=info --pool=solo
