### <center>DVD Rental Co<center> 
# <center><u>Exploratory Data Analysis</u><center>
#### <center>*Dive into the legacy data: Migration, Analysis and Key Insights*<center>
<br>
Data Analyst: Christophe Giraud | December 2025 <br>
Public repo: https://github.com/giraudcgc-cpu/DE25_sql_lab1_christophe_giraud

## <u>Movies Over 180 min</u>
```sql films
    SELECT 
        title as Title,
        length as "Length (minutes)"
    FROM 
        film
    WHERE 
        "Length (minutes)" > 180
    ORDER BY 
        "Length (minutes)" DESC
    LIMIT 5;
```

## <u>Movies Including "love"</u>
```sql love
    SELECT
        title AS Title, rating AS Rating, length AS "Length (min)", description AS Description
    FROM 
        film
    WHERE 
        regexp_matches(title, '(?i)\\blove\\b');
```

## <u>Film Duration Statistics (minutes)</u>
```sql duration
    SELECT
        MIN(length) as "Shortest duration",
        ROUND(AVG(length)) as "Mean duration",
        MEDIAN(length) as "Median duration", 
        MAX(length) as "Longest duration" 
    FROM
        film;
```


## <u>10 Most Expensive Rentals (per day)</u>
```sql rentals
    SELECT  
        title AS Title,
        rental_duration AS "Rental duration",
        rental_rate AS "Rental rate",
        (rental_rate/rental_duration) AS "Price/Day"
    FROM
        film
    ORDER BY
        "Price/Day" DESC
    LIMIT 10;
```

## <u>Top 10 Most Prolific Actors (Number of movies)</u>
```sql actors
    SELECT
        a.actor_id AS "Actor ID",
        a.first_name || ' ' || a.last_name AS "Actor Name",
        COUNT(fa.film_id) AS "Movie count"
    FROM
        actor a
    JOIN 
        film_actor fa ON a.actor_id = fa.actor_id
    GROUP BY
        a.actor_id, a.first_name, a.last_name
    HAVING COUNT(fa.film_id) > 0 
    ORDER BY
        "Movie count" DESC
    LIMIT 10;
```


## <u>Own findings 1/4: Top 3 most rented categories</u>
```sql categories
    SELECT
        c.name as Category,
        COUNT(r.rental_id) AS "Total rentals" 
    FROM
        category c 
    JOIN film_category fc ON c.category_id = fc.category_id 
    JOIN film f ON fc.film_id = f.film_id 
    JOIN inventory inv ON f.film_id = inv.film_id
    JOIN rental r ON inv.inventory_id = r.inventory_id 
    GROUP BY
        c.name 
    ORDER BY
        "Total rentals" DESC 
    LIMIT 3;
```

## <u>Own findings 2/4: Churn customers list<u>
```sql churn
    SELECT
        cust.customer_id AS "Customer nber",
        CONCAT(cust.first_name,' ', cust.last_name) AS "Customer Name", 
        MIN(r.rental_date) as "First rental", 
        MAX(r.rental_date) as "Last rental", 
        cust.email as Email, 
    FROM
        customer cust 
    LEFT JOIN rental r ON cust.customer_id =  r.customer_id 
    WHERE cust.active = '0' 
    GROUP BY ALL 
    ORDER BY "Customer Name";
```


## <u>Own findings 3/4: Stores revenues in 2006<u>
```sql stores
    SELECT 
        s.store_id AS "Store ID",
        CONCAT(ad.address, ', ', ci.city, ', ', cou.country) AS "Full address", 
        SUM(p.amount) AS "Total revenue", 
    FROM 
        payment p 
    JOIN rental r ON p.rental_id = r.rental_id 
    JOIN inventory i ON r.inventory_id = i.inventory_id 
    JOIN store s ON i.store_id = s.store_id 
    JOIN address ad ON s.address_id = ad.address_id 
    JOIN city ci ON ad.city_id = ci.city_id 
    JOIN country cou ON ci.country_id = cou.country_id 
    WHERE
        EXTRACT(YEAR FROM p.payment_date) = 2006 
    GROUP BY 
        s.store_id, ad.address, ci.city, cou.country  
    ORDER BY
        "Total revenue" ASC;
```


## <u>Own findings 4/4: What ratings have our films?</u>
```sql ratings
SELECT
    rating AS name,
    COUNT(*) AS value
FROM film 
GROUP BY rating
ORDER BY value DESC
```
<ECharts config={{
title:{text: 'Films Content Rating Sytem'},
tooltip:{formatter: '{b}: {c} ({d}%)'},
series:[{type: 'pie', data: [...ratings]}]
}}/>
Source: https://docs.evidence.dev/components/charts/custom-echarts


## <u>Top 5 Customers Per Total Spend</u>
```sql top5
    SELECT 
        cust.customer_id AS "Customer nber", 
        cust.first_name || ' ' || cust.last_name AS "Customer name", 
        SUM(p.amount) AS "Total spend"
    FROM
        customer cust 
    JOIN payment p ON cust.customer_id = p.customer_id 
    GROUP BY
        cust.customer_id, "Customer name" 
    ORDER BY
        "Total spend" DESC 
    LIMIT 5;
```

<BarChart
    data={top5}
    title="Top 5 Customers Per Total Spend"
    x="Customer name"
    y="Total spend"
    labels=true
    fillColor="green"
/>


## <u>Revenue Per Film Category</u>
```sql revenue
    SELECT
        c.name AS "Category name",
        SUM(p.amount) AS Revenues                                              
    FROM category c
    LEFT JOIN film_category fc ON c.category_id = fc.category_id
    LEFT JOIN film f ON fc.film_id = f.film_id
    LEFT JOIN inventory inv ON f.film_id = inv.film_id
    LEFT JOIN rental r ON inv.inventory_id = r.inventory_id
    LEFT JOIN payment p ON r.rental_id = p.rental_id
    GROUP BY c.name
    ORDER BY Revenues DESC;
```

<BarChart
    data={revenue}
    title="Total Revenue Per Film Category"
    x="Category name"
    y=Revenues
    labels=true
    fillColor="green"
/>

