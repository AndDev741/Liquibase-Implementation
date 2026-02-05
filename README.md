# Liquibase-Implementation

Spring Boot project to practice Liquibase database migrations with PostgreSQL.

## Goal

Learn how Liquibase integrates with Spring Boot, how changelogs are organized, and
how schema changes are applied automatically on application startup.

## Tech Stack

- Java 21
- Spring Boot 4.0.2
- Liquibase
- PostgreSQL
- Maven (wrapper included)

## Prerequisites

- Java 21 installed
- PostgreSQL running locally (or update the datasource URL)
- Database credentials available via environment variables

## Quick Start

1. Create a database and user (example):

```sql
CREATE DATABASE liquibase_demo;
CREATE USER liquibase_user WITH PASSWORD 'secret';
GRANT ALL PRIVILEGES ON DATABASE liquibase_demo TO liquibase_user;
```

2. Create a `.env` file (based on `envSample`):

```bash
DB_USER=liquibase_user
DB_PASS=secret
```

3. Run the app:

```bash
./mvnw spring-boot:run
```

Or use the helper script:

```bash
chmod +x start.sh
./start.sh
```

## Liquibase Layout

Liquibase is enabled in `src/main/resources/application.yaml` and uses this
master changelog:

- `src/main/resources/db/changelog/db.changelog-master.yaml`

That file includes all changelogs under:

- `src/main/resources/db/changelog/schemas` (schema creation/changes)
- `src/main/resources/db/changelog/indexes` (indexes, constraints)

Example change files in this repo:

- `schemas/001-init-schema.yaml` creates the `users` table
- `schemas/002-add-user-status.yaml` adds a `status` column
- `indexes/003-add-email-index.yaml` creates an email index

There are also example placeholders:

- `data/seedData.example`
- `releases/release.example`

## Adding a New ChangeSet

Create a new YAML file under `schemas/` or `indexes/`. The master changelog
uses `includeAll`, so new files are picked up automatically.

Minimal example:

```yaml
databaseChangeLog:
  - changeSet:
      id: 004-add-user-phone
      author: your-name
      changes:
        - addColumn:
            tableName: users
            columns:
              - column:
                  name: phone
                  type: VARCHAR(20)
```

## Useful Commands

```bash
./mvnw spring-boot:run
./mvnw test
./mvnw clean package
```

## Notes

- Hibernate DDL auto is set to `none` so Liquibase is the source of truth.
- Update `spring.datasource.url` in `src/main/resources/application.yaml` if
  your database runs elsewhere.
