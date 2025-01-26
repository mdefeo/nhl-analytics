# Step-by-Step Instructions for Setup

## Step 1: Initialize Repositories

1. Create a Git repository for the backend and frontend

```bash
mkdir nhl-live-tracker-backend nhl-live-tracker-frontend
cd nhl-live-tracker-backend
git init
cd ../nhl-live-tracker-frontend
git init
```

2. Set up basic .gitignore files to exclude environment files, node modules, and build artifacts.

## Step 2: Set Up the Backend

1. Backend Boilerplate:

- If using Python FastAPI:

```bash
pip install fastapi uvicorn psycopg2
mkdir app
touch app/main.py
```

- If using Node.js with Express or NestJS:

```bash
mkdir src
npm init -y
npm install express graphql apollo-server
```

2. Database Connection:

- Install necessary database clients:

```bash
pip install sqlalchemy alembic # Python
npm install pg sequelize # Node.js
```

3. Define Schemas:

- Create models for live game data, player stats, and team stats.
- Use ORM tools like SQLAlchemy (Python) or Sequelize (Node.js).

4. API Setup:

- Add routes for basic data fetching from the NHL API.
- Expose a GraphQL schema for querying data.

## Step 3: Set Up the Frontend

1. Initialize a React or Next.js project:

```bash
npx create-react-app nhl-live-tracker-frontend
cd nhl-live-tracker-frontend
```

Or, for Next.js:

```bash
npx create-next-app@latest nhl-live-tracker-frontend
cd nhl-live-tracker-frontend
```

2. Install UI and charting libraries:

```bash
npm install @mui/material chart.js
```

3. Create components for:

- Live game tracking.
- Player and team statistics.
- Historical trends and visualizations.

## Step 4: Set Up Docker and Containerization

1. Create Dockerfiles for backend and frontend:

- Backend Dockerfile:

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Or for Node.js:

```dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
CMD ["node", "src/index.js"]
```

- Frontend Dockerfile:

```dockerfile
FROM node:14
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
CMD ["npm", "start"]
```

3. Create a docker-compose.yml file to run both services:

```yml
version: '3.8'
services:
  backend:
    build: ./nhl-live-tracker-backend
    ports:
      - "8000:8000"
  frontend:
    build: ./nhl-live-tracker-frontend
    ports:
      - "3000:3000"
```

## Step 5: Connect Backend to Database

1. Spin up a Postgres container using Docker:

```yml
services:
  db:
    image: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: nhluser
      POSTGRES_PASSWORD: nhlpassword
      POSTGRES_DB: nhldb
```

2. Configure the backend to connect to the database using environment variables.

## Step 6: NHL API Integration

1. Set up an API client in the backend to fetch data from the NHL API.
2. Create background tasks for periodic data fetching (e.g., using Celery or Node cron jobs).
3. Cache frequently accessed data with Redis.

## Step 7: Finalize GraphQL API

1. Define GraphQL schemas for:

- Live game tracking.
- Player and team statistics.
- Historical trends and analytics.

2. Implement resolvers to fetch and return data.

## Step 8: Frontend Integration

1. Fetch data from the GraphQL API.
2. Build live game, stats, and analytics pages.
3. Test the UI for responsiveness and usability.
