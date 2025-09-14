

# Sample Databases

This repository contains containerized PostgreSQL sample databases for development, testing, and learning purposes.

## Databases Included

### 🎵 Chinook Database
A digital media store database featuring:
- Music catalog with artists, albums, and tracks
- Customer and employee management
- Sales transactions and invoicing
- Playlist management

**Schema highlights:** Artist, Album, Track, Customer, Employee, Invoice, InvoiceLine, Playlist, Genre, MediaType

### 🎬 Netflix Database
A streaming content database featuring:
- Movies and TV shows catalog
- Season information for TV series
- Viewing statistics and engagement metrics
- Runtime and release information

**Schema highlights:** Movie, TV_Show, Season, View_Summary

## Prerequisites

- Docker and Docker Compose installed
- PostgreSQL client (optional, for direct database access)

## Quick Start

### 1. Clone the repository
```bash
git clone <repository-url>
cd sample-dbs
```

### 2. Configure environment (optional)
The `.env` file contains default PostgreSQL credentials:
```env
POSTGRES_USER=postgres
POSTGRES_PASSWORD=Mm13231323
POSTGRES_PORT=5432  # Default for Chinook
```

### 3. Start the databases

**Chinook Database:**
```bash
cd chinook
docker-compose up -d
```
- Runs on port **5432**
- Database name: `chinook`

**Netflix Database:**
```bash
cd netflix
docker-compose up -d
```
- Runs on port **5433**
- Database name: `netflix`

## Connecting to the Databases

### Using psql
```bash
# Connect to Chinook
psql -h localhost -p 5432 -U postgres -d chinook

# Connect to Netflix
psql -h localhost -p 5433 -U postgres -d netflix
```

### Connection Strings
```
# Chinook
postgresql://postgres:Mm13231323@localhost:5432/chinook

# Netflix
postgresql://postgres:Mm13231323@localhost:5433/netflix
```

## Sample Queries

### Chinook Database
```sql
-- Find all albums by a specific artist
SELECT ar.name as artist, al.title as album
FROM artist ar
JOIN album al ON ar.artist_id = al.artist_id
WHERE ar.name LIKE '%Beatles%';

-- Top 10 customers by total purchases
SELECT c.first_name || ' ' || c.last_name as customer,
       SUM(i.total) as total_spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
GROUP BY c.customer_id
ORDER BY total_spent DESC
LIMIT 10;

-- Most popular genres by track count
SELECT g.name as genre, COUNT(*) as track_count
FROM track t
JOIN genre g ON t.genre_id = g.genre_id
GROUP BY g.genre_id
ORDER BY track_count DESC;
```

### Netflix Database
```sql
-- Movies released in 2024
SELECT title, runtime, release_date
FROM movie
WHERE release_date >= '2024-01-01'
ORDER BY release_date DESC;

-- Top 10 most viewed movies (weekly)
SELECT v.view_rank, m.title, v.hours_viewed, v.views
FROM view_summary v
JOIN movie m ON m.id = v.movie_id
WHERE v.duration = 'WEEKLY'
  AND v.end_date = '2025-06-29'
ORDER BY v.view_rank
LIMIT 10;

-- TV shows with their seasons
SELECT t.title as show, s.season_number, s.title as season_title
FROM tv_show t
JOIN season s ON t.id = s.tv_show_id
ORDER BY t.title, s.season_number;
```

## Managing the Databases

### Stop containers
```bash
# Stop Chinook
cd chinook && docker-compose down

# Stop Netflix
cd netflix && docker-compose down
```

### Remove containers and data
```bash
# Remove Chinook (including data)
cd chinook && docker-compose down -v

# Remove Netflix (including data)
cd netflix && docker-compose down -v
```

### View logs
```bash
# Chinook logs
cd chinook && docker-compose logs -f

# Netflix logs
cd netflix && docker-compose logs -f
```

### Check container status
```bash
docker ps
```

## Data Sources

- **Chinook Database**: Originally created by Luis Rocha as an alternative to the Northwind database. Contains real iTunes library data for media-related information.
  - [GitHub Repository](https://github.com/lerocha/chinook-database)

- **Netflix Database**: Based on Netflix's public engagement reports and global top 10 data.
  - [Netflix Engagement Report](https://about.netflix.com/en/news/what-we-watched-the-first-half-of-2024)
  - [Netflix Global Top 10](https://www.netflix.com/tudum/top10)

## Troubleshooting

### Port already in use
If you get a "port already in use" error, either:
1. Stop the conflicting service
2. Or modify the port in the respective `docker-compose.yaml` file

### Cannot connect to database
1. Ensure the container is running: `docker ps`
2. Check logs for errors: `docker-compose logs`
3. Verify credentials in `.env` file

### Reset database to initial state
```bash
# For Chinook
cd chinook
docker-compose down -v
docker-compose up -d

# For Netflix
cd netflix
docker-compose down -v
docker-compose up -d
```

## License

- Chinook Database: [MIT License](https://github.com/lerocha/chinook-database/blob/master/LICENSE.md)
- Netflix Database: [License](https://github.com/lerocha/netflixdb/blob/main/LICENSE)