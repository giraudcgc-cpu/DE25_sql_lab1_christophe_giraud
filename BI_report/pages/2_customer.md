### <center>DVD Rental Co<center> 
# <u><center>Our customer base<center></u>
<br>

## <u>The Customer Count</u>
```sql Customers_Overview
    SELECT 
        COUNT(DISTINCT customer_id) AS "Customer Count",
        COUNT(DISTINCT CASE WHEN active = '1' THEN customer_id END) AS "Active Customers",
        COUNT(DISTINCT CASE WHEN active = '0' THEN customer_id END) AS "Non Active Customers"
    FROM
        customer;
```

```sql Non_Active_Customers
    SELECT 
        co.country AS "Country",
        COUNT(*) AS "Total Customers",
        COUNT(CASE WHEN c.active = 0 THEN 1 END) AS "Non-Active Customers",
        COUNT(CASE WHEN c.active = 1 THEN 1 END) AS "Active Customers"
    FROM 
        customer c
    JOIN 
        store s ON c.store_id = s.store_id
    JOIN 
        address a ON s.address_id = a.address_id
    JOIN 
        city ci ON a.city_id = ci.city_id
    JOIN 
        country co ON ci.country_id = co.country_id
    GROUP BY 
        co.country
    ORDER BY 
        co.country;
```
## <u>Top 5 Customers Per Total Spend (task2a)</u>
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
    
/>


## <u>Customer Average Spent & Location</u>
```sql spent
    SELECT
        SUM(p.amount) / COUNT(DISTINCT c.customer_id) AS "AVG Customer Spent (incl. 0 rental)",
        co.country as Location
    FROM
        customer c
    JOIN 
        store s ON c.store_id = s.store_id
    JOIN 
        address a ON s.address_id = a.address_id
    JOIN 
        city ci ON a.city_id = ci.city_id
    JOIN
        country co ON ci.country_id = co.country_id
    LEFT JOIN
        payment p ON c.customer_id = p.customer_id
    GROUP BY
        co.country;
```
<ECharts config={{
  title:{text:'Average Customer Spent Per Location'},
  tooltip:{formatter:'{b}:${c}({d}%)'},
  series:[{type:'pie',data:[{value:111,name:'Australia'},{value:114,name:'Canada'}]}]
}} />

## <u>What Do They Watch? (task1f1)</u>
```sql category_Australia
    SELECT
        cat.name AS Category,
        COUNT(*) AS Rentals
    FROM 
        rental r
    JOIN 
        customer c ON r.customer_id = c.customer_id
    JOIN 
        store s ON c.store_id = s.store_id
    JOIN    
        address a ON s.address_id = a.address_id
    JOIN    
        city ci ON a.city_id = ci.city_id
    JOIN    
        country co ON ci.country_id = co.country_id
    JOIN    
        inventory i ON r.inventory_id = i.inventory_id
    JOIN
        film_category fc ON i.film_id = fc.film_id
    JOIN
        category cat ON fc.category_id = cat.category_id
    WHERE 
        co.country = 'Australia'
    GROUP BY 
        cat.name, 
    ORDER BY 
        Rentals DESC
    LIMIT 5;
```

```sql category_Canada
    SELECT
        cat.name AS Category,
        COUNT(*) AS Rentals
    FROM 
        rental r
    JOIN 
        customer c ON r.customer_id = c.customer_id
    JOIN 
        store s ON c.store_id = s.store_id
    JOIN    
        address a ON s.address_id = a.address_id
    JOIN    
        city ci ON a.city_id = ci.city_id
    JOIN    
        country co ON ci.country_id = co.country_id
    JOIN    
        inventory i ON r.inventory_id = i.inventory_id
    JOIN
        film_category fc ON i.film_id = fc.film_id
    JOIN
        category cat ON fc.category_id = cat.category_id
    WHERE 
        co.country = 'Canada'
    GROUP BY 
        cat.name, 
    ORDER BY 
        Rentals DESC
    LIMIT 5;
```

<BarChart
    data={category_Australia}
    title="Most Rented Categories in Australia"
    x="Category"
    y="Rentals"
    xAxisTitle="Category"
    yAxisTitle="Total Rentals"
    sort="desc" 
    labels=true 
/>


<BarChart
    data={category_Canada}
    title="Most Rented Categories in Canada"
    x="Category"
    y="Rentals"
    xAxisTitle="Category"
    yAxisTitle="Total Rentals"
    sort="desc"
    labels=true
/>

