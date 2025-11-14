# Vendor & Contract Management Platform

# Overview

VCMP is a full-stack learning and development project designed to mirror commercial-grade architecture and engineering practices. It implements a Vendor & Contract Management Platform built using:

* Angular (Frontend)
* Go (API Layer)
* PostgreSQL (Database)
* Liquibase (Database migrations)
* Docker + Docker Compose (Local environment)
* TailwindCSS (Styling)
* Kubernetes (Future deployment)
* AWS/Azure (Optional cloud integrations)

The goal is to create a realistic, production-style system that demonstrates scalable architecture, clean development workflows, automated migrations, and DevOps-friendly practices.

# Architecture Overview

High-Level Components:

```
Angular UI  →  Go API  →  PostgreSQL (HA-ready)
                     ↓
                Liquibase
           (DB Schema Migrations)

Docker Compose → Local Dev Environment

Future:
  Kubernetes (GKE/EKS/AKS)
  AWS/Azure integrations
```

# Tech Stack

**Frontend:** Angular, TypeScript, RxJS, TailwindCSS, Angular Material (optional)

**Backend:** Go, Echo/Fiber (TBD), Clean Architecture, Unit & Integration Testing

**Database:** PostgreSQL, Liquibase migrations, Dockerised environment

**Infrastructure:** Docker/Compose, Kubernetes (future), AWS S3/SNS/Lambda (future), Azure KV/Storage (future)

# Project Structure

```
VCMP/
│
├── api/                     # Golang API source
├── ui/                      # Angular frontend
├── db/
│   ├── changelog-master.xml
│   ├── changelogs/
│   │   ├── 001_create_users_table.xml
│   │   ├── 002_create_vendors_table.xml
│   │   ├── 003_create_contacts_table.xml
│   │   └── 004_create_contracts_table.xml
│   ├── liquibase.properties
│   └── docker-compose.yml
└── README.md
```

# Database & Migrations

## PostgreSQL

Start the DB with:

```bash
docker-compose up -d postgres
```

Default development database values:

| Variable | Value        |
| -------- | ------------ |
| Host     | localhost    |
| Port     | 5432         |
| Database | vcm_db    |
| Username | dev_user     |
| Password | dev_password |

## Liquibase

### PostgreSQL JDBC Driver

* Version: 42.7.8
* Download: [https://jdbc.postgresql.org/download/postgresql-42.7.8.jar](https://jdbc.postgresql.org/download/postgresql-42.7.8.jar)

Place it inside `db/`.

### Master Changelog

All migration files inside `changelogs/` are automatically included via:

```xml
<includeAll path="changelogs" relativeToChangelogFile="true"/>
```

### Running Migrations

**Option 1 — CLI:**

```bash
cd db
liquibase \
  --changeLogFile=changelog-master.xml \
  --url=jdbc:postgresql://localhost:5432/project_a \
  --username=dev_user \
  --password=dev_password \
  --classpath=postgresql-42.7.8.jar \
  update
```

**Option 2 — liquibase.properties:**

```properties
changeLogFile=changelog-master.xml
url=jdbc:postgresql://localhost:5432/project_a
username=dev_user
password=dev_password
classpath=postgresql-42.7.8.jar
driver=org.postgresql.Driver
```

Then run:

```bash
liquibase update
```

**Option 3 — Docker:**

```bash
docker run --rm \
  -v $(pwd):/liquibase/changelog \
  --network=project_a_network \
  liquibase/liquibase \
  --changeLogFile=changelog-master.xml \
  --url=jdbc:postgresql://postgres:5432/project_a \
  --username=dev_user \
  --password=dev_password \
  update
```

# Testing Strategy

**Backend:** Go unit and integration tests

**Frontend:** Jasmine/Karma, Cypress (future)

**Database:** Liquibase `status` & `validate` commands, test DB resets

# Docker Setup

**Start all services:**

```bash
docker-compose up -d
```

**Stop all services:**

```bash
docker-compose down
```

**Reset DB volume:**

```bash
docker volume rm project-a_postgres_data
```

# Roadmap

**Phase 1 – Foundations:** DB schema, Liquibase, Docker DB, Go API scaffolding, Angular setup

**Phase 2 – Core Features:** Authentication, Vendor CRUD, Contacts, Contracts, File uploads

**Phase 3 – Integrations:** AWS, Azure, Email/Notifications, Metrics & logging

**Phase 4 – Deployment:** Kubernetes, Helm, CI/CD

# Contribution Workflow

1. Create a feature branch
2. Add new Liquibase changeset (never modify existing)
3. Write Go/Angular code
4. Run tests
5. Commit + push
6. Merge to main

# License

This project is for learning and professional portfolio development. Use and modify freely.