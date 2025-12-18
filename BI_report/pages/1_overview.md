### <center>DVD Rental Co<center> 
# <center><u>Exploratory Data Analysis</u><center>
#### <center>*Dive into the legacy data: Migration, Analysis and Key Insights*<center>
<br>
Data Analyst: Christophe Giraud | December 2025 <br>
Public repo: https://github.com/giraudcgc-cpu/DE25_sql_lab1_christophe_giraud
<br>
<br>

## <u>Revenue Per Year</u>
```sql revenue_year
    SELECT
        YEAR(payment_date) AS Year,
        SUM(amount) AS "Yearly Revenue"
    FROM
        payment 
    GROUP BY 
        YEAR
````

## <u>Revenue Per Film Category (task2b)</u>
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
    title="Revenue Per Film Category"
    x="Category name"
    y="Revenues"
    labels=true
    swapXY=true
    />

## <u>The Films Database</u>
```sql collection
    SELECT
        COUNT(DISTINCT f.film_id) AS "Count Film (Unique value)",
        COUNT(i.inventory_id) AS "Total Copies Available",
        SUM(CASE WHEN i.inventory_id IS NULL THEN 1 ELSE 0 END) AS "Films With No Copies"
    FROM
        film f;  
    LEFT JOIN 
        inventory i ON f.film_id = i.film_id
```

## <u>Film Duration Statistics (task1c)</u>
```sql duration
    SELECT
        MIN(length) as "Shortest Duration",
        ROUND(AVG(length)) as "Mean Duration",
        MEDIAN(length) as "Median Duration", 
        MAX(length) as "Longest Duration",
    FROM
        film;   
```

## <u>Films Ratings (task1f4)</u>
```sql ratings
SELECT
    rating as "Film Ratings",
    COUNT(*) AS "Film Count"
FROM 
    film 
GROUP BY 
    "Film Ratings"
ORDER BY 
    "Film Count" DESC
```
<ECharts config={{
title:{text:"Films Content Rating Sytem"},
tooltip:{formatter:'{b}:{c}({d}%)'},
series:[{type:'pie',
data:ratings.map(row =>({name:row["Film Ratings"],value:row["Film Count"]}))}]
}}/>

