# PaintCorp ERP - Sarathi Format DevOps Exercise 10

This package keeps the **actual PaintCorp ERP frontend and backend functionality** from the supplied project, but separates them into `frontend` and `backend` folders so the project can follow the structure and Docker workflow shown in the Sarathi Exercise 10 reference.

## Structure

```text
PaintCorp-Sarathi-Format/
├── frontend/          # Original React/Vite PaintCorp UI
├── backend/            # Original Express API and business logic
├── paint_db.sql        # Original database schema/data
├── docker-compose.yml  # MySQL + backend + frontend
└── PROJECT_DOCUMENTATION.md
```

## Run locally without Docker

### Backend
```powershell
cd backend
npm install
npm start
```

### Frontend (new terminal)
```powershell
cd frontend
npm install
npm run dev
```

Open `http://localhost:5173`.

The frontend uses `VITE_API_URL=http://localhost:5000` by default.

## Run with Docker Compose

From the project root:

```powershell
docker compose up --build
```

Open:
- Frontend: http://localhost:3000
- Backend: http://localhost:5000
- MySQL: localhost:3306

## Docker images

```powershell
docker images
```

## Stop containers

```powershell
docker compose down
```

To also remove the MySQL volume:

```powershell
docker compose down -v
```

## Docker Hub

```powershell
docker login

docker tag paintcorp-sarathi-format-frontend YOUR_USERNAME/paintcorp-frontend:v1
docker tag paintcorp-sarathi-format-backend YOUR_USERNAME/paintcorp-backend:v1

docker push YOUR_USERNAME/paintcorp-frontend:v1
docker push YOUR_USERNAME/paintcorp-backend:v1
```

If Docker Compose generates different local image names, use `docker images` and tag the corresponding image IDs.
