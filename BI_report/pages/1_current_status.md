### <center>DVD Rental Co<center> 
# <center><u>Exploratory Data Analysis</u><center>
#### <center>*Dive into the legacy data: Migration, Analysis and Key Insights*<center>
<br>
Data Analyst: Christophe Giraud | December 2025 <br>
Public repo: https://github.com/giraudcgc-cpu/DE25_sql_lab1_christophe_giraud
<br>
<br>

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
    y="Revenues"
    labels=true
    swapXY=true
    />

# <u>The Films Database</u>
```sql collection
    SELECT
        COUNT(film_id) AS "Total Titles"
    FROM
        film;  
```