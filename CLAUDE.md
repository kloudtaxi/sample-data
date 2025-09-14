# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains sample PostgreSQL databases for development and testing purposes. It includes two main databases:
- **Chinook**: A digital media store database with artists, albums, tracks, invoices, and customers
- **Netflix**: A database with movies and TV shows based on Netflix engagement reports

## Database Setup Commands

### Start PostgreSQL Container
```bash
docker-compose up -d
```

### Stop PostgreSQL Container
```bash
docker-compose down
```

### Connect to PostgreSQL
```bash
psql -h localhost -p 5432 -U postgres -d chinook
# Password is in .env file
```

### Load Sample Databases
```bash
# Load Chinook database
psql -h localhost -p 5432 -U postgres -d chinook < chinook/Chinook_PostgreSql.sql

# Create and load Netflix database
psql -h localhost -p 5432 -U postgres -c "CREATE DATABASE netflix;"
psql -h localhost -p 5432 -U postgres -d netflix < netflix/netflixdb-postgres.sql
```

## Database Schemas

### Chinook Database
- Core tables: Artist, Album, Track, MediaType, Genre, Customer, Employee, Invoice, InvoiceLine, Playlist, PlaylistTrack
- Represents a digital media store with full sales tracking

### Netflix Database
- Core tables: movie, tv_show, season, view_summary
- Contains viewing statistics, runtime data, and release information
- Includes both weekly and semi-annual engagement data

## Environment Configuration

The `.env` file contains PostgreSQL connection parameters:
- POSTGRES_HOST: localhost
- POSTGRES_PORT: 5432
- POSTGRES_DB: chinook (default)
- POSTGRES_USER: postgres
- POSTGRES_PASSWORD: (configured in .env)

## Docker Configuration

The `docker-compose.yaml` file configures:
- PostgreSQL container named `chinook-db`
- Persistent volume `chinook-data` for database storage
- Port 5432 exposed for database connections
- Environment variables loaded from `.env` file