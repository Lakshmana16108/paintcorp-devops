# EX NO: 10 - Building, Running and Publishing PaintCorp ERP in Docker Hub

## AIM
To containerize and deploy the PaintCorp ERP full-stack application using Docker. The React frontend and Node.js/Express backend are separated into individual folders and Docker images, executed locally as containers, and prepared for publication to Docker Hub and GitHub.

## REQUIREMENTS
1. PaintCorp ERP React frontend and Node.js/Express backend.
2. Docker Desktop.
3. Docker Hub account.
4. GitHub account.

## Step 1: Project Setup

The project is organized as:
- `/frontend` -> original PaintCorp React/Vite application
- `/backend` -> original PaintCorp Node.js/Express API
- `paint_db.sql` -> MySQL schema/data

For a local non-Docker test:

### Backend
```powershell
cd backend
npm install
npm start
```

### Frontend
Open another terminal:
```powershell
cd frontend
npm install
npm run dev
```

Frontend: `http://localhost:5173`
Backend: `http://localhost:5000`

## Step 2: Create Dockerfiles

### Frontend - `frontend/Dockerfile`

```dockerfile
FROM node:20 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
ARG VITE_API_URL=http://localhost:5000
ENV VITE_API_URL=$VITE_API_URL
RUN npm run build
FROM nginx:alpine
COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Backend - `backend/Dockerfile`

```dockerfile
FROM node:20
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 5000
CMD ["npm", "start"]
```

## Step 3: Create `.dockerignore`

Both frontend and backend exclude `node_modules`, logs, `.env` files and Git metadata.

## Step 4: Build Docker Images

Backend:
```powershell
cd backend
docker build -t paintcorp/backend-app .
```

Frontend:
```powershell
cd ../frontend
docker build --build-arg VITE_API_URL=http://localhost:5000 -t paintcorp/frontend-app .
```

## Step 5: Run Containers Locally

Start MySQL first if running individual containers, or use Docker Compose for the complete stack.

Recommended complete-stack command from the project root:

```powershell
docker compose up --build
```

Open:
- `http://localhost:3000` -> PaintCorp React UI
- `http://localhost:5000` -> PaintCorp Backend API

## Step 6: Push Project to GitHub

```powershell
git init
git add .
git commit -m "Dockerized PaintCorp ERP"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/paintcorp-devops.git
git push -u origin main
```

## Step 7: Push Images to Docker Hub

```powershell
docker login

docker tag paintcorp/frontend-app YOUR_USERNAME/paintcorp-frontend:v1
docker push YOUR_USERNAME/paintcorp-frontend:v1

docker tag paintcorp/backend-app YOUR_USERNAME/paintcorp-backend:v1
docker push YOUR_USERNAME/paintcorp-backend:v1
```

## Step 8: Testing and Validation

```powershell
docker compose ps
docker ps
```

Verify the PaintCorp UI and test the main implemented modules:
- Authentication
- Admin dashboard
- Product/Paint management
- Available stock and stock transactions
- Orders
- Billing-related workflows
- Sales dashboard/reports
- User management
- CorpAI
- Database-backed operations

## Result

Thus, the PaintCorp ERP application was separated into frontend and backend components, containerized using Docker, connected to MySQL, and prepared for local deployment, GitHub version control and Docker Hub publication.
