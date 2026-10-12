# 🛠️ Handy Jobs Hub (Riverpod)

[![Flutter](https://img.shields.io/badge/Flutter-3.x-02569B?logo=flutter&logoColor=white)](https://flutter.dev)
[![Dart](https://img.shields.io/badge/Dart-3.2+-0175C2?logo=dart&logoColor=white)](https://dart.dev)
[![State Management](https://img.shields.io/badge/State%20Management-Riverpod%202.5-00D2B4?logo=flutter&logoColor=white)](https://riverpod.dev)
[![NestJS](https://img.shields.io/badge/NestJS-10.3-E0234E?logo=nestjs&logoColor=white)](https://nestjs.com)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org)
[![Prisma](https://img.shields.io/badge/Prisma-5.10-2D3748?logo=prisma&logoColor=white)](https://www.prisma.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-13+-4169E1?logo=postgresql&logoColor=white)](https://www.postgresql.org)
[![Docker](https://img.shields.io/badge/Docker-Supported-2496ED?logo=docker&logoColor=white)](https://www.docker.com)

> **Handy Jobs Hub** is a robust, full-stack cross-platform mobile marketplace connecting employers and handy service seekers. Built with a modular **NestJS + Prisma + PostgreSQL** backend API and a reactive **Flutter + Riverpod** mobile client.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Key Features](#-key-features)
- [System Architecture](#-system-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [1. Database Setup with Docker](#1-database-setup-with-docker)
  - [2. Backend Setup (NestJS)](#2-backend-setup-nestjs)
  - [3. Frontend Setup (Flutter Riverpod)](#3-frontend-setup-flutter-riverpod)
- [Environment Variables](#-environment-variables)
- [REST API Reference](#-rest-api-reference)
- [Admin Portal & Security](#-admin-portal--security)
- [Team Members](#-team-members)
- [License](#-license)

---

## 🌟 Overview

**Handy Jobs Hub** bridges the gap between employers seeking skilled manual labor or handy services and job seekers looking for local and specialized job vacancies. The application incorporates role-based access control, secure JWT-based authentication, a comprehensive job posting and application workflow, and an integrated feedback/review mechanism to promote trust and accountability.

The mobile client leverages **Riverpod** for declarative, compile-safe, reactive state management and clean architectural separation of concerns.

---

## ✨ Key Features

- **🔐 Authentication & Authorization:**
  - Secure signup and login powered by JSON Web Tokens (JWT).
  - Passwords encrypted with **Argon2** and **Bcrypt**.
  - Role-based permissions differentiating standard `USER` accounts and elevated `ADMIN` accounts.
  - Dedicated admin registration portal protected by an access passcode (`12345`).

- **💼 Job Vacancy & Seeking Postings:**
  - Distinct workflows for **Employers** (`EMPLOYEE`) and **Job Seekers** (`JOB_SEEKER`).
  - Full CRUD capabilities: create new job offers/requests, edit listings, browse filtered feeds, and delete postings.
  - Quick contact direct phone integration for rapid communication.

- **⭐ Feedback & Rating System:**
  - Integrated 5-star review mechanism for jobs and services.
  - Users can post feedback, view evaluations from others, and edit or remove their own reviews.
  - Transparent rating metric to ensure service quality and mutual trust.

- **🛡️ Administration & Moderation:**
  - Dedicated administrative dashboard to view all platform users and job listings.
  - Moderation capabilities to **block / unblock** abusive accounts.
  - Authority to prune violating job listings and manage community health.

- **⚡ Reactive Riverpod Architecture:**
  - Unidirectional data flow and immutable state handling.
  - Auto-disposing providers, state notifiers, and clean decoupled repository layer.
  - Smooth bottom navigation (`curved_navigation_bar`) and dynamic routing with `go_router`.

---

## 🏗️ System Architecture

```mermaid
graph TD
    subgraph Client ["Mobile Client (Flutter)"]
        UI["Flutter UI Screens"]
        State["Riverpod State Providers / Notifiers"]
        Repo["Data Repositories / Dio & HTTP Services"]
        UI <--> State
        State <--> Repo
    end

    subgraph Server ["Backend API (NestJS)"]
        Controllers["Controllers (Auth / Jobs / Users / Reviews)"]
        Guards["Guards (JwtAuthGuard, RolesGuard)"]
        Services["Domain Services & Business Logic"]
        PrismaService["Prisma ORM Service"]
        Controllers --> Guards --> Services --> PrismaService
    end

    subgraph Storage ["Database Layer"]
        DB[(PostgreSQL 13+)]
        PrismaService <--> DB
    end

    Repo -- "REST API (JSON / JWT Bearer)" --> Controllers
```

---

## 💻 Tech Stack

### Mobile Frontend
- **Framework:** [Flutter](https://flutter.dev) (v3.x) & [Dart](https://dart.dev) (v3.2+)
- **State Management:** [Riverpod](https://riverpod.dev) (`flutter_riverpod: ^2.5.1`, `riverpod: ^2.5.1`, `state_notifier: ^1.0.0`)
- **Navigation:** [GoRouter](https://pub.dev/packages/go_router) & [Curved Navigation Bar](https://pub.dev/packages/curved_navigation_bar)
- **HTTP Client:** [Dio](https://pub.dev/packages/dio) & [Http](https://pub.dev/packages/http)
- **Local Persistence:** [SharedPreferences](https://pub.dev/packages/shared_preferences)
- **Widgets & UI:** [Flutter Rating Bar](https://pub.dev/packages/flutter_rating_bar), Material 3, Cupertino Icons

### Backend Server
- **Framework:** [NestJS 10](https://nestjs.com) (TypeScript 5.x)
- **Runtime:** Node.js (v18+)
- **ORM:** [Prisma 5.10](https://www.prisma.io)
- **Database:** [PostgreSQL 13+](https://www.postgresql.org)
- **Security & Crypto:** `@nestjs/jwt`, `passport-jwt`, `argon2`, `bcrypt`
- **Validation:** `class-validator`, `class-transformer`
- **Containerization:** Docker & Docker Compose

---

## 📁 Project Structure

```text
handy-jobs-hub-riverpod/
├── mobilebackend/                   # NestJS Backend API
│   ├── docker-compose.yml           # PostgreSQL container definition
│   ├── package.json                 # Backend dependencies & npm scripts
│   ├── prisma/
│   │   ├── schema.prisma            # Prisma schema (User, Job, Review models)
│   │   └── migrations/              # Database migration history
│   └── src/
│       ├── auth/                    # Auth module (JWT, Sign-in, Sign-up, Guards)
│       │   ├── decorator/           # @Public(), @Roles(), @GetUser() decorators
│       │   ├── dto/                 # Auth & registration DTOs
│       │   ├── guard/               # JwtAuthGuard, RolesGuard
│       │   └── strategy/            # Passport JWT strategy
│       ├── job/                     # Job postings module (CRUD, user type filtering)
│       ├── user/                    # User module (Profile, Admin block/unblock)
│       ├── review/                  # Reviews & ratings module
│       ├── prisma/                  # Prisma service provider
│       ├── app.module.ts            # Root module
│       └── main.ts                  # NestJS bootstrap with CORS & Global Pipes
└── mobileriverpod/                  # Flutter Mobile Client (Riverpod)
    ├── android/                     # Android platform build files
    ├── analysis_options.yaml        # Dart linting & analysis configuration
    └── .gitignore                   # Flutter gitignore rules
```

---

## 🚀 Getting Started

### Prerequisites

Ensure you have the following installed on your machine:
- [Flutter SDK](https://docs.flutter.dev/get-started/install) (3.22+ recommended)
- [Dart SDK](https://dart.dev/get-dart) (bundled with Flutter)
- [Node.js](https://nodejs.org/) (v18.x or v20.x LTS) & [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (for PostgreSQL) or a local PostgreSQL instance
- [Git](https://git-scm.com/)

---

### 1. Database Setup with Docker

You can spin up the PostgreSQL database in seconds using the included `docker-compose.yml`:

```bash
cd mobilebackend
docker-compose up -d
```

This starts a PostgreSQL 13 instance accessible on `localhost:5432` with:
- **Database:** `nest`
- **User:** `postgres`
- **Password:** `123`

---

### 2. Backend Setup (NestJS)

1. **Navigate to the backend directory:**
   ```bash
   cd mobilebackend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   # or with yarn:
   yarn install
   ```

3. **Configure Environment Variables:**
   Create or verify the `.env` file in `mobilebackend/.env`:
   ```env
   DATABASE_URL="postgresql://postgres:123@localhost:5432/nest?schema=public"
   JWT_SECRET="password123"
   PORT=3000
   ```

4. **Run Prisma Migrations:**
   ```bash
   npx prisma migrate dev
   ```

5. **Start the NestJS Development Server:**
   ```bash
   npm run start:dev
   # or with yarn:
   yarn start:dev
   ```

   The backend will start and listen at `http://localhost:3000`.

---

### 3. Frontend Setup (Flutter Riverpod)

1. **Navigate to the mobile app directory:**
   ```bash
   cd mobileriverpod
   ```

2. **Install Flutter packages:**
   ```bash
   flutter pub get
   ```

3. **Backend Host Configuration:**
   - When running on an **Android Emulator**, use `http://10.0.2.2:3000` to reach your machine's `localhost`.
   - When running on a **Physical Device**, use your machine's local network IP (e.g., `http://192.168.x.x:3000`).
   - When running on **Flutter Web / Desktop / iOS Simulator**, use `http://localhost:3000`.

4. **Run the Application:**
   ```bash
   flutter run
   ```

---

## ⚙️ Environment Variables

The backend uses the following environment variables (defined in `mobilebackend/.env`):

| Variable | Description | Default / Example |
| :--- | :--- | :--- |
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://postgres:123@localhost:5432/nest?schema=public` |
| `JWT_SECRET` | Secret key used for signing JWT tokens | `password123` |
| `PORT` | API server listening port | `3000` |

---

## 📡 REST API Reference

### 🔐 Authentication (`/auth`)

| Method | Endpoint | Auth | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/signup` | Public | Register a new user (`UserDto`) |
| `POST` | `/auth/signin` | Public | Authenticate user & return JWT token |
| `GET` | `/auth/profile` | JWT (`USER`, `ADMIN`) | Fetch current authenticated user's profile |
| `POST` | `/auth/logout` | JWT | Log out and revoke active user session |

### 💼 Jobs (`/jobs`)

| Method | Endpoint | Auth | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/jobs` | JWT | Create a new job vacancy or job request |
| `GET` | `/jobs/forEmployees` | JWT | Retrieve all jobs posted for employees |
| `GET` | `/jobs/forJobSeekers` | JWT | Retrieve all jobs posted for job seekers |
| `GET` | `/jobs/user-jobs/:userId` | JWT | Retrieve all jobs posted by a specific user ID |
| `PATCH` | `/jobs/:id` | JWT | Update job details (`title`, `description`, `salary`) |
| `DELETE` | `/jobs/:id` | JWT | Delete a specific job listing |
| `GET` | `/jobs/getAllJobs` | JWT (`ADMIN`) | Retrieve all jobs across the platform (Admin) |

### 👥 Users (`/users`)

| Method | Endpoint | Auth | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/users` | JWT (`ADMIN`) | Retrieve all registered users |
| `GET` | `/users/profile/:userId` | JWT | Retrieve user profile details by user ID |
| `PATCH` | `/users/:id` | JWT | Update user profile information |
| `DELETE` | `/users/:id` | JWT | Delete own account / user account |
| `DELETE` | `/users/:id/block` | JWT (`ADMIN`) | Toggle block / unblock status for a user |

### ⭐ Reviews & Ratings (`/review`)

| Method | Endpoint | Auth | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/review` | JWT | Post a review and star rating (`1-5`) for a job |
| `GET` | `/review/job/:jobId` | JWT | Fetch all reviews and ratings for a job |
| `GET` | `/review/user/:userId` | JWT | Fetch all reviews authored by a specific user |
| `PATCH` | `/review` | JWT | Update an existing review |
| `DELETE` | `/review/:id` | JWT | Remove a review |

---

## 🔒 Admin Portal & Security

- **Admin Registration Passcode:** The navigation code to access the admin registration page is `'12345'`.
- **JWT Protection:** Secured endpoints require an `Authorization` header with a Bearer token:
  ```http
  Authorization: Bearer <your_access_token>
  ```
- **Role Verification:** Critical endpoints utilize `@UseGuards(JwtAuthGuard, RolesGuard)` combined with `@Roles(Role.ADMIN)` to restrict access to authorized administrators only.

---


