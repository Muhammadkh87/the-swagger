# The Swagger 
 
> Design & construction project management — a full-stack DevOps learning project
 
![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)
![NestJS](https://img.shields.io/badge/NestJS-11-red?logo=nestjs)
![Prisma](https://img.shields.io/badge/Prisma-6-2D3748?logo=prisma)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-blue?logo=postgresql)
![TailwindCSS](https://img.shields.io/badge/Tailwind-v4-38bdf8?logo=tailwindcss)
![Docker](https://img.shields.io/badge/Docker-blue?logo=docker)
![AWS ECS](https://img.shields.io/badge/AWS-ECS-orange?logo=amazon-aws)
![GitHub Actions](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-2088FF?logo=github-actions)
 
A full-stack web application built to practice real-world DevOps skills end to end. The app itself is a project management tool for design and construction teams. The focus of this project is not just the app — it's everything around it: containerisation, cloud infrastructure, and automated deployments.
 
---
 
##  Live Demo
 
**URL:** http://swagger-lb-589969688.ap-southeast-2.elb.amazonaws.com
 
| Role | Email | Password |
|------|-------|----------|
| Admin | admin@devopscookie.com | Password123! |
| Manager | manager@devopscookie.com | Password123! |
| Client | client@devopscookie.com | Password123! |
| Contractor | contractor@devopscookie.com | Password123! |
| Viewer | viewer@devopscookie.com | Password123! |
 
---
 
##  Tech Stack
 
| Layer | Technology |
|-------|------------|
| Frontend | Next.js 16, React 19, Tailwind CSS v4, Radix UI |
| Backend | NestJS 11, Passport.js, JWT (HTTP-only cookies) |
| Database | PostgreSQL + Prisma ORM |
| Monorepo | pnpm workspaces + Nx |
| Containerisation | Docker + Docker Compose |
| Container Registry | AWS ECR |
| Hosting | AWS ECS (Fargate) |
| Load Balancer | AWS ALB |
| CI/CD | GitHub Actions |
| Linting | Biome |
 
---
 
##  Features
 
- JWT authentication via HTTP-only cookies
- Role-based access (Admin, Design Manager, Client, Contractor, Viewer)
- Project management with status tracking and budget monitoring
- Task management per project with comments
- Team/user management
- Swagger API documentation
- Monorepo with shared types between frontend and backend
---
 
##  Project Structure
 
```
the-swagger/
├── apps/
│   ├── api/          # NestJS backend (port 4200)
│   └── web/          # Next.js frontend (port 4000)
├── packages/
│   └── shared/       # Shared types & constants
├── .github/
│   └── workflows/    # GitHub Actions CI/CD pipelines
├── docker-compose.yml
├── pnpm-workspace.yaml
└── nx.json
```
 
---
 
## DevOps Journey — Phase by Phase
 
### Phase 1 — App Running on EC2
Manually deployed the app on an EC2 instance to understand the basics of cloud hosting without any automation.
 
### Phase 2 — Dockerized with Docker Compose
Containerised both the frontend and backend using Docker. Used Docker Compose to run them together locally with a single command.
 
### Phase 3 — Live on AWS ECS + RDS
Moved from EC2 to a production-grade setup using ECS Fargate for containers and RDS for a managed database. Set up an Application Load Balancer to route traffic.
 
### Phase 4 — CI/CD Pipelines
Automated the entire build and deployment process using GitHub Actions. Every push to `main` automatically builds, pushes to ECR, and deploys to ECS.
 
### Phase 5 — Seed Data + Documentation
Updated seed data, cleaned up codebase, and documented the project.
 
---
 
##  Running Locally
 
### Prerequisites
 
- Node.js >= 18
- pnpm >= 9 (`npm install -g pnpm`)
- PostgreSQL running locally
### 1. Clone the repo
 
```bash
git clone https://github.com/Muhammadkh87/the-swagger.git
cd the-swagger
```
 
### 2. Install dependencies
 
```bash
pnpm install
```
 
### 3. Set up environment variables
 
Create `apps/api/.env`:
 
```env
DATABASE_URL="postgresql://postgres:postgres@localhost:5432/the_swagger_dev?schema=public"
JWT_SECRET="your-super-secret-jwt-key-change-in-production"
JWT_EXPIRES_IN="7d"
PORT=4200
NODE_ENV=development
COOKIE_SECRET="your-cookie-secret-change-in-production"
CORS_ORIGIN=http://localhost:4000
```
 
### 4. Set up the database
 
```bash
cd apps/api
npx prisma migrate dev
cd ../..
pnpm db:seed
```
 
### 5. Start the development servers
 
```bash
pnpm dev
```
 
This starts both the API (port 4200) and the web app (port 4000) concurrently.
 
Open [http://localhost:4000](http://localhost:4000) in your browser.
 
---
 
##  Available Scripts
 
| Command | Description |
|---------|-------------|
| `pnpm dev` | Start both API and web in dev mode |
| `pnpm build` | Build both apps for production |
| `pnpm db:seed` | Seed the database with test data |
| `pnpm lint` | Lint with Biome |
| `pnpm format` | Format with Biome |
| `pnpm check:fix` | Lint + format fix |
 
---
 
##  API
 
- Base URL: `http://localhost:4200/api/v1`
- Swagger Docs: `http://localhost:4200/api/docs`
- Auth: HTTP-only cookie (`access_token`) set on login
### Key Endpoints
 
| Method | Route | Description |
|--------|-------|-------------|
| POST | `/api/v1/auth/login` | Login |
| POST | `/api/v1/auth/logout` | Logout |
| GET | `/api/v1/auth/me` | Current user |
| GET | `/api/v1/projects` | List projects |
| POST | `/api/v1/projects` | Create project |
| GET | `/api/v1/projects/:id` | Get project |
| GET | `/api/v1/projects/:id/tasks` | List tasks |
| GET | `/api/v1/users` | List users |
 
---
 
##  Running with Docker Compose
 
```bash
docker-compose up --build
```
 
---
 
##  AWS Architecture
 
> Architecture diagram coming soon (draw.io)
 
```
Internet
    │
    ▼
Application Load Balancer (swagger-lb)
    │
    ├── /api/*  ──▶  ECS Service: the-swagger-api  (port 4200)
    │                      │
    │                      ▼
    │               Amazon RDS (PostgreSQL)
    │
    └── /*      ──▶  ECS Service: the-swagger-web  (port 4000)
```
 
### AWS Details
 
| Resource | Value |
|----------|-------|
| Account ID | YOUR_AWS_ACCOUNT_ID |
| Region | ap-southeast-2 |
| ECR | <account-id>.dkr.ecr.ap-southeast-2.amazonaws.com |
| ECS Cluster | the-swagger-cluster |
| RDS | <rds-endpoint>.ap-southeast-2.rds.amazonaws.com |
 
---
 
##  CI/CD Pipeline
 
Two workflows run on push to `main`:
 
- **Build & Deploy API** — builds the NestJS backend image, pushes to ECR, deploys to ECS
- **Build & Deploy Web** — builds the Next.js frontend image, pushes to ECR, deploys to ECS
### GitHub Secrets Required
 
| Secret | Description |
|--------|-------------|
| `AWS_ACCESS_KEY_ID` | IAM user access key |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret key |
| `DATABASE_URL` | RDS connection string |
 
---
 
##  Key Fixes & Lessons Learned
 
### Removed `@nx/nest/plugin` from `nx.json` + Regenerated Prisma Client
 
The `@nx/nest/plugin` entry in `nx.json` was causing build failures. Nx was trying to infer project configuration using the plugin, which conflicted with the explicit project setup. Removing it from the plugins array was the first step, but `pnpm dev` still didn't work after that.
 
The second fix was to regenerate the Prisma client:
 
```bash
npx prisma generate
```
 
This recreated the auto-generated Prisma client code which had become out of sync during the broken build. After both fixes the app ran successfully.
 
```json
// Before (broken)
{
  "plugins": [
    "@nx/nest/plugin"
  ]
}
 
// After (fixed)
{
  "plugins": []
}
```
 
### Added `.dockerignore`
Without a `.dockerignore`, Docker was copying `node_modules` and other unnecessary files into the image, causing slow builds and bloated image sizes.
 
### Fixed AWS Region in Workflows
The GitHub Actions workflows had the wrong AWS region configured. Updated to `ap-southeast-2` to match where the ECS cluster and ECR registry are hosted.
 
### Added Task Definition Cleanup Step
ECS task definitions accumulate old registered versions over time. Added a cleanup step in both workflows to deregister old task definitions and keep things tidy.
 
---
 
##  Screenshots
 
### Login Page — Live on AWS ECS
![App Live](./docs/screenshots/app-live.png)
 
### Dashboard
![Dashboard](./docs/screenshots/app-live-on-ecs.png)
 

### CI/CD Pipeline — GitHub Actions
![Pipeline](./docs/screenshots/pipeline.png)
 
---
 
*Built by [@Muhammadkh87](https://github.com/Muhammadkh87) as a hands-on DevOps learning project*