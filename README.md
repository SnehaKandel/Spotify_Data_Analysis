# Spotify_Data_Analysis

**Overview**
This project is all about exploring and analyzing a Spotify dataset that includes information about tracks, albums, and artists using SQL. I worked on cleaning and organizing the data, writing queries of different difficulty levels and optimizing them for better performance. The main goal was to strengthen my SQL skills while uncovering useful insights from the dataset.

**Project Steps**
**1. Data Exploration**
 The dataset contains attributes such as:

Artist: The performer of the track.
Track: The name of the song.
Album: The album to which the track belongs.
Album_type: The type of album (e.g., single or album).
Various metrics such as danceability, energy, loudness, tempo, and more.

**Advanced Analysis & Optimization**

Find the top 3 most-viewed tracks for each artist using window functions.
```sql
WITH ranking_artist
	AS
(SELECT 
artist,
track,
SUM(views) as total_view
DENSE_RANK() OVER(PARTITION BY artist ORDER BY SUM(views)DESC as rank )
FROM spotify
GROUP BY 1,2
ORDER BY 1,3 DESC 
)
SELECT * FROM ranking_artist 
WHERE rank <= 3
```

Write a query to find tracks where the liveness score is above the average.
```sql
SELECT 
track,artist,liveness
FROM spotify
WHERE liveness > (SELECT AVG(liveness)FROM spotify)
SELECT AVG (liveness)FROM spotify --0.19
```

Use a WITH clause to calculate the difference between the highest and lowest energy values for tracks in each album.
```sql
WITH energy_stats AS (
    SELECT
        album,
        MAX(energy) as highest_energy,
        MIN(energy) as lowest_energy
    FROM spotify
    GROUP BY album
)
SELECT
    album,
    highest_energy - lowest_energy as energy_difference
FROM energy_stats;
```

**Query Optimization Technique**

Initial Query Performance Analysis Using EXPLAIN

began by analyzing the performance of a query using the EXPLAIN function.
The query retrieved tracks based on the artist column, and the performance metrics were as follows:
Execution time (E.T.): 7 ms
Planning time (P.T.): 0.17 ms
Below is the screenshot of the EXPLAIN result before optimization:

<img width="870" height="343" alt="image" src="https://github.com/user-attachments/assets/a6cfc647-c894-41b1-89ea-eda7aa9e005f" />


**Index Creation on the artist Column**

To optimize the query performance, created an index on the artist column. 
SQL command for creating the index:
```sql
CREATE INDEX idx_artist ON spotify_tracks(artist);
```
Performance Analysis After Index Creation

After creating the index,  ran the same query again and observed significant improvements in performance:
Execution time (E.T.): 0.153 ms
Planning time (P.T.): 0.152 ms
Below is the screenshot of the EXPLAIN result after index creation:

<img width="867" height="345" alt="image" src="https://github.com/user-attachments/assets/44963c45-d3d0-470a-9276-07f54c4c92fa" />

**Graphical Performance Comparison**

A graph illustrating the comparison between the initial query execution time and the optimized query execution time after index creation.
<img width="1245" height="327" alt="image" src="https://github.com/user-attachments/assets/83cb93ba-088a-4960-86c2-474dd97807a2" />

<img width="932" height="212" alt="image" src="https://github.com/user-attachments/assets/6d49df8a-2e19-4c51-b38f-34e99690dbc5" />

<img width="1319" height="221" alt="image" src="https://github.com/user-attachments/assets/fc5c158f-08e4-4872-81a7-bbe763888d2d" />

**Technology Stack**
Database: PostgreSQL
SQL Queries: DDL, DML, Aggregations, Joins, Subqueries, Window Functions
Tools: pgAdmin 4 



