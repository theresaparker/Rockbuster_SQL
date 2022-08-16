# Queries Overview
 1.  Descriptive Statistics
 2.  Top 10 countries with the most customers
 3.  Top 10 cities with the most customers
 4.  Top 5 customers
 5.  Top 10 customers globally
 6.  Top 10 movies
 7.  Bottom 10 movies
 8.  Revenue by genre
 9.  Revenue by rating
 10.  Global Revenue

## **1. Descriptive Statistics**
##### -- Query for descriptive statistics on numerical columns in the fim table--
    SELECT 	
        MIN(rental_duration) AS min_rental_time,
        AVG(rental_duration) AS avg_rental_time,
        MAX(rental_duration) AS max_rental_time,
        MIN(rental_rate) AS min_rental_rate,
        AVG(rental_rate) AS avg_rental_rate,
        MAX(rental_rate) AS max_rental_rate,
        MIN(replacement_cost) AS min_replacement_cost,
        AVG(replacement_cost) AS avg_replacement_cost,
        MAX(replacement_cost) AS max_replacement_cost
    FROM film	
## **2. Top 10 countries with most customers**
##### --Query for Top 10 countries with the most customers -- 
    SELECT	
        D.country,
        COUNT (customer_id) AS customer_count
    FROM Customer A	
    INNER JOIN address B on a.address_id = b.address_id	
    INNER JOIN city C on b.city_id = c.city_id	
    INNER JOIN country D on c.country_id = d.country_id	
    GROUP BY D.country	
    ORDER BY customer_count desc	
    LIMIT 10	
## **3. Top 10 cities with the most customers**
##### --Query for Top 10 Cities w/in the top 10 of the most customers -- 
    SELECT		
        C.City,	
        D.Country,	
        COUNT (customer_id) AS customer_count	
    FROM Customer A		
    INNER JOIN address B on a.address_id = b.address_id		
    INNER JOIN city C on b.city_id = c.city_id		
    INNER JOIN country D on c.country_id = d.country_id		
    WHERE D.Country IN ('India','China','United States','Japan','Mexico','Brazil',		
            'Russian Federation','Philippines,','Turkey','Indonesia')
    GROUP BY C.City, D.Country		
    ORDER BY customer_count desc		
    LIMIT 10		
## **4. Top 5 customers**
##### Query for top 5 customers in the top 10 cities with the most customers.
    SELECT				
        a.customer_id,			
        a.first_name,			
        a.last_name,			
        c.city,			
        d.country,			
            SUM(e.amount) AS total_amount_paid		
    FROM CUSTOMER A				
    INNER JOIN address B on a.address_id = b.address_id				
    INNER JOIN city C on b.city_id = c.city_id				
    INNER JOIN country D on c.country_id = d.country_id				
    INNER JOIN payment E on a.customer_id = e.customer_id				
    WHERE c.city IN ('Aurora','Pingxiang','Sivas','Dhule(Dhulia)','Kurashiki',				
                    'Xintai','Adoni','Celaya','Nezahualcyotl','Atlixco')
    GROUP BY 				
        a.customer_id,			
        C.City, 			
        D.Country			
    ORDER BY total_amount_paid desc				
    LIMIT 5				
## **5. Top 10 customers globally**
##### --Query for Top 10 Customers Globally--
    Select	
        a.customer_id,
        a.first_name,
        a.last_name,
        c.city,
        d.country,
        SUM(e.amount) AS total_amount_paid
    FROM customer A	
    INNER JOIN address B ON A.address_id = B.address_id	
    INNER JOIN city C ON B.city_id = C.city_id	
    INNER JOIN country D ON C.country_ID = D.country_ID	
    INNER JOIN payment E ON A.customer_id = E.customer_id	
    GROUP BY A.customer_id, C.city, D. country	
    ORDER BY total_amount_paid DESC	
    LIMIT 10;	
## **6. Top 10 movies**
##### -- Query for Top 10 movies, rating, genre, and revenue amount--
    SELECT 	
        A.title,
        A.rating,
        f.name,
        SUM (D.amount) AS movie_amount
    FROM Film A	
    INNER JOIN inventory B on a.film_id = b.film_id	
    INNER JOIN rental C on B.inventory_id = c.inventory_id	
    INNER JOIN payment D on C.rental_id = d.rental_id	
    INNER JOIN film_category E on a.film_id = E.film_id	
    INNER JOIN category F on e.category_id = f.category_id	
    GROUP BY a.title, a.rating, f.name	
    ORDER BY movie_amount desc	
    LIMIT 10	
## **7. Bottom 10 movies**
##### -- -- Query for Bottom 10 movies, rating, genre, and revenue amount--

    SELECT 	
        A.title,
        A.rating,
        f.name,
        SUM (D.amount) AS movie_amount
    FROM Film A	
    INNER JOIN inventory B on a.film_id = b.film_id	
    INNER JOIN rental C on B.inventory_id = c.inventory_id	
    INNER JOIN payment D on C.rental_id = d.rental_id	
    INNER JOIN film_category E on a.film_id = E.film_id	
    INNER JOIN category F on e.category_id = f.category_id	
    GROUP BY a.title, a.rating, f.name	
    ORDER BY movie_amount asc	
    LIMIT 10	
## **8. Revenue by genre** 
##### -- Query for Revenue by Genres --
    SELECT b.name,	
        SUM (e.amount) AS revenue_amount
    FROM film_category A	
    INNER JOIN Category B ON a.category_id = b.category_id	
    INNER JOIN Inventory C ON a.film_id = c.film_id	
    INNER JOIN Rental D ON c.inventory_id = d.inventory_id	
    INNER JOIN Payment E ON d.rental_id = e.rental_id	
    GROUP BY b.name	
    ORDER BY revenue_amount DESC	
## **9. Revenue by rating** 
##### -- Query for Revenue by ratings --
    SELECT a.rating,	
        SUM (d.amount) AS revenue_amount
    FROM film A	
    INNER JOIN inventory B on a.film_id = b.film_id	
    INNER JOIN rental C on b.inventory_id = c.inventory_id	
    INNER JOIN payment D on c.rental_id = d.rental_id	
    GROUP BY a.rating	
    ORDER BY revenue_amount desc	
## **10. Global Revenue** 
##### --Query for Revenue by Country & average country revenue
    SELECT 	
        D.country,
        COUNT (a.customer_ID) AS total_customers,
        SUM (e.amount) AS country_revenue,
        ROUND(avg(e.amount),2) AS average_country_revenue,
        mode() within group (order by f.rating)
    FROM Customer A	
    INNER JOIN address B on a.address_id = b.address_id	
    INNER JOIN city C on b.city_id = c.city_id	
    INNER JOIN country D on c.country_id = d.country_id	
    INNER JOIN payment E on a.customer_id = e.customer_id	
    INNER JOIN rental R on r.rental_id = e.rental_id	
    INNER JOIN inventory I on i.inventory_id = r.inventory_id	
    inner join film F on f.film_id=i.film_id	
    GROUP BY Country	
    ORDER BY country_revenue desc	

