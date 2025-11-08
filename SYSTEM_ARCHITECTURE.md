# MyWoman - System Architecture

## Overview
This document describes the technical architecture for MyWoman. It outlines the frontend, backend, database, and how components communicate. The goal is a modular, maintainable system that can be deployed incrementally.

## High level components
1. Frontend
   - React web app for browsers.
   - Optionally React Native for mobile.
   - Responsibilities: UI, input validation, client-side routing, basic offline handling.

2. Backend API
   - Node.js with Express.
   - REST endpoints for authentication, user profile, goals, resources, groups.
   - Business logic lives here.

3. Database
   - PostgreSQL for relational data: users, goals, groups, transactions.
   - Use migrations with a tool like Knex or Prisma.

4. File storage
   - Amazon S3 or any object store for profile images and attachments.

5. Authentication & security
   - JWT for session tokens.
   - Passwords hashed with bcrypt.
   - Rate limiting and input validation on critical endpoints.
   - HTTPS enforced in production.

6. Devops / Deployment
   - Frontend: Vercel or Netlify.
   - Backend: Render or Heroku.
   - Database: Managed Postgres (Heroku Postgres, RDS, or Supabase).
   - CI: GitHub Actions for tests and deploy.

## Component interaction
- The frontend calls the backend API over HTTPS.
- The backend queries the PostgreSQL database for persistent data.
- For file uploads, frontend uploads to the backend which signs S3 uploads, or uploads directly with presigned URLs.
- Notifications: use a worker queue (Bull with Redis) to send emails, push or SMS via providers.

## API examples
- `POST /api/auth/register` -> register user
- `POST /api/auth/login` -> returns JWT
- `GET /api/users/:id/goals` -> returns user goals
- `POST /api/goals` -> create a target goal

## Data model (high level)
- User: id, name, email, phone, country, password_hash, created_at
- Goal: id, user_id, title, description, target_amount, frequency, start_date, end_date
- Group: id, name, type, admin_id
- Resource: id, title, url, category

## Scalability and performance
- Start with a single web instance and managed DB.  
- Use caching for high read endpoints with Redis.  
- Use background jobs for heavy tasks like sending emails or generating reports.

## Security considerations
- Input validation and sanitization at backend.
- Secure storage of secrets using environment variables.
- Limit rate for authentication endpoints to prevent brute force.

## Feasibility and roadmap
- Phase 1: Minimal viable product with core onboarding, goal creation, and resource browsing.
- Phase 2: Groups, mentors, file uploads, scheduling.
- Phase 3: Analytics, localization and offline sync.

## Optional diagrams
You can add a diagram here. Example Mermaid diagram:

```mermaid
graph TD
  Client[Frontend React] -->|HTTPS| API[Node.js API]
  API --> DB[Postgres]
  API --> S3[File Storage]
  API --> Queue[Redis/Bull]
  Queue --> Worker[Background Worker]
  Worker --> Email[Email / SMS Provider]
