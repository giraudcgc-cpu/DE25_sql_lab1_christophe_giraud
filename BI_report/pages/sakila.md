# Exploring Sakila Database

 ## Movies longer than 180min
```sql films
    SELECT 
        title, 
        length
    FROM 
        film
    WHERE 
        length > 180
    ORDER BY 
        length DESC
    LIMIT 5;
```

## Movies Including "love"
```sql film
    SELECT
        title, rating, length, description
    FROM 
        film
    WHERE 
        regexp_matches(title, '(?i)\\blove\\b');
```

## Film Duration Statistics
```sql duration
    SELECT
        MIN(length) as shortest_duration,
        ROUND(AVG(length)) as mean_duration,
        MEDIAN(length) as median_duration, 
        MAX(length) as longest_duration 
    FROM
        film;
```

## 10 most expensive rentals (per day)
```sql film
    SELECT  
        MIN(length) as shortest_duration,
        ROUND(AVG(length)) as mean_duration,
        MEDIAN(length) as median_duration, 
        MAX(length) as longest_duration 
    FROM
        film;
```
