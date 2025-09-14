# Netflix Sample Database

Sample database with movies and TV shows based on the data from the [Netflix Engagement Report](https://about.netflix.com/en/news/what-we-watched-the-first-half-of-2024) and the [Netflix Global Top 10](https://www.netflix.com/tudum/top10) weekly list, it includes movies and TV shows for learning and practice purposes.

## Supported Database Servers

* MySQL
* Oracle
* PostgreSQL
* SQLite
* SQL Server

## Download
Download the SQL scripts from the [latest release](../../releases) assets. One or more SQL script files are provided for each database vendor supported. You can run these SQL scripts with your preferred database tool.

## Data Sources

* https://about.netflix.com/en/news/what-we-watched-the-first-half-of-2024
* https://about.netflix.com/en/news/what-we-watched-the-second-half-of-2023
* https://www.netflix.com/tudum/top10



## Sample Queries - Postgres

```sql
-- Movies released since 2024-01-01
select id, title, runtime from movie where release_date >= '2024-01-01';
```

```sql
-- TV Show Seasons released since 2024-01-01
select s.id, s.title as season_title, s.season_number, t.title as tv_show, s.runtime
from season s left join tv_show t on t.id = s.tv_show_id
where s.release_date >= '2024-01-01';
```

```sql
-- Top 10 movies (English)
select v.view_rank, m.title, v.hours_viewed, m.runtime, v.views, v.cumulative_weeks_in_top10
from view_summary v
inner join movie m on m.id = v.movie_id
where duration = 'WEEKLY'
  and end_date = '2025-06-29'
  and m.locale = 'en'
order by v.view_rank;
```

```sql
-- Engagement report
select m.title, m.original_title, m.available_globally, m.release_date, v.hours_viewed, m.runtime, v.views
from view_summary v
inner join movie m on m.id = v.movie_id
where duration = 'SEMI_ANNUALLY'
  and start_date = '2024-01-01'
order by v.view_rank asc;
```

