### <center>DVD Rental Co<center> 
# <u><center>Insights & Recommendations<center></u>
<br>


# <u>Customer Activation List (task1f2)</u> 
```sql activation_list  
    SELECT
        cust.customer_id, 
        CONCAT(cust.first_name,' ', cust.last_name) AS customer_name, 
        MIN(r.rental_date) as first_rental, 
        MAX(r.rental_date) as last_rental, 
        cust.email, 
    FROM
        customer cust 
    LEFT JOIN rental r ON cust.customer_id =  r.customer_id 
    WHERE cust.active = '0' 
    GROUP BY ALL 
    ORDER BY customer_name;
```

# <u>Rating: ↓ Adult (NC-17) And ↑ Family-Friendly (G & PG)</u>
```sql my_text
    SELECT 
        'According to my research, we have too many NC-17 rated movies compare to the revenues they generate i the Box Office' AS description
```
source: https://www.the-numbers.com/market/mpaa-ratings

# <u>STP (Segmentation, Targeting, Positioning)</u>
### 10M French Speakers in Canada
```sql language_list
    SELECT 
        l.language_id,
        name,
        COUNT(film_id) AS "Film Count" 
    FROM 
        language l
    LEFT JOIN 
        film f ON l.language_id = f.language_id
    GROUP BY
        l.language_id, name;
```