# SQL Queries for Rockbuster Data

## 1. Creating a new table
    CREATE TABLE EMPLOYEES (
        employee_id INT NOT NULL,
        name VARCHAR (50),
        contact_number VARCHAR (30),
        designation_id INT,
        last_update TIMESTAMP NOT NULL DEFAULT now(),
    CONTSTRAINT employee_pkey PRIMARY KEY (employee_id)
        )

## 2. Inserting values (name of genre) into dataframe
Using select command to find out what film genres exist in the category table. 
    
    SELECT name, 
        category_id
    FROM Category

Insert genres into the category table

    INSERT INTO public.category
        (name)
        VALUES ('Thriller');
    INSERT INTO public.category
        (name)
        VALUES ('Crime');
    INSERT INTO public.category
        (name)
        VALUES ('Mystery');
    INSERT INTO public.category
        (name)
        VALUES ('Romance');
    INSERT INTO public.category
        (name)
        VALUES ('War')

    SELECT name
    FROM category

## 3. Database Querying
###### Ordering Data - Run a query that selects every film sorted by title from A-Z, then by most recent release year. 
        SELECT title,
            release_year
        FROM film
        ORDER BY title,
            release_year ASC
###### Grouping Data  - What is the average rental rate for each rating? 
    SELECT rating,
        AVG (rental_rate)
    FROM film
    GROUP BY rating
    ORDER BY rating ASC
## 4. Filtering Data
###### Using LIKE to find a film that contains the word UPTOWN in any position
    SELECT film_id,
        title,
        description
    FROM film
    WHERE title LIKE '%Uptown%'
###### Using > to find film length that is more than 120 minutes and rental rate is more than $2.99. 
    SELECT film_id,
        title,
        description,
        length,
        rental_rate
    FROM film
    WHERE length > 120 AND rental_rate > 2.99
###### Using BETWEEN to find a rental duration between 3 and 7 days (where 3 and 7 aren't exclusive)
    SELECT film_id,
        title,
        description,
        rental_duration
    FROM film
    WHERE rental_duration BETWEEN 4 and 6
    ORDER BY rental_duration ASC
###### Using IN to find film ratings that are either PG or G rated. 
    SELECT film_id,
        title,
        description,
        rating
    FROM film
    WHERE rating IN ('PG', 'G')
    ORDER BY rating ASC
###### Using The query you wrote in step above returned a list of movies that meet certain criteria (film rating is either PG or G). The inventory team has asked for the following information about this list:  
* Count of movies
* average rental rate
* max rental duration and min rental duration

        SELECT rating,
            COUNT (film_id) AS count_of_movies,
            AVG (rental_rate) AS average_rental_rate
            MAX (rental_duration) AS max_rental_duration
            MIN (rental_duration) AS min_rental_duration
        FROM film
        WHERE rating IN ('PG', 'G')
        GROUP BY rating
## 5. Summarizing & Cleaning Data
### Checking for duplicates
    SELECT customer_id,
            store_id,
            first_name,
            last_name,
            email,
            address_id,
            COUNT (*)
    FROM customer
    GROUP BY customer id,
            store_id,
            first_name,
            last_name,
            email,
            address_id
    HAVING COUNT (*) > 1
### **Fixing Duplicates:** 
- If there were duplicates there are two ways of dealing with duplicate values: 
  - Create a new view, to where unique records can be selected. 
  - Delete the duplicate record from table or view.

- If you are NOT able to alter your table, you can use GROUP BY or DISTINCT in the query to select only unique records. 
### Checking for Non-Uniform/Incorrect Data: 
* Go through each column using the GROUP BY function to ensure the data is uniform.  Here is an example

        SELECT customer_id
        FROM customer
        GROUP BY customer_id
        ORDER BY customer_id asc
    
### **Fixing non-uniform/incorrect data:**
- All data appeared to be uniform; however, if I did need to correct the data, I would use the UPDATE function.  
- For example:  If you need to make the ratings column uniform for all "G" ratings, you can use this query. 

        UPDATE film
        SET rating = 'G'
        WHERE rating IN ('gen', 'g', 'General')
### **Checking for missing data:**

    SELECT * 
    FROM film,
        customer
    WHERE film_id is null
### **Fixing Missing Data:**
Here are two options to address missing data:

1. Ignore the column if it contains a high percentage of missing  values.  It is good practice to write a comment if you ignore a column.  


        Example of Proper Syntax: 
        SELECT col1, 
        col2, 
        col4... --col3 ignored in select because it has a lot of missing values
        FROM tablename

2. Impute missing values using a statistical method with the AVG or MODE function. 
Example of imputing missing values using the AVG function

        UPDATE tablename
        SET = AVG (col1)
        WHERE col1 IS NULL
## 6. Descriptive Statistics for numerical columns
    SELECT 
        MIN(rental_duration) AS min_rental_time,
        MAX(rental_duration) AS max_rental_time,
        AVG(rental_duration) AS avg_rental_time,

        MIN(rental_rate) AS min_rental_amount
        MAX(rental_rate) AS max_rental_amount
        AVG(renta_rate) AS avg_rental_amount

        MIN(replacement_cost) AS min_replacement_cost
        MAX(replacement_cost) AS max_replacement_cost
        AVG(replacement_cost) AS avg_replacement_cost

        FROM film
## 6. Using the mode for non-numerical columns
    SELECT mode() WITHIN GROUP (ORDER BY title)
        AS modal_title,
        mode() WITHIN GROUP (ORDER BY description)
        AS modal_description,
        mode() WITHIN GROUP (ORDER BY language_id)
        AS modal_language,
        mode() WITHING GROUP (ORDER BY rating)
        AS modal_rating,
        COUNT (*) as count_rows
    FROM fillm

    



    


