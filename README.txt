USE sakila;

-- 

#1.a
SELECT first_name, last_name

FROM actor;



-- #1.b

SELECT concat(first_name," ", last_name) 

AS Actor_Name

FROM actor;



-- #2a

SELECT actor_id, first_name, last_name

FROM actor

WHERE first_name = 'Joe';



-- #2b

SELECT last_name

FROM actor

WHERE last_name LIKE '%GEN%';



-- #2c

SELECT last_name, first_name

FROM actor

WHERE last_name LIKE '%LI%'

ORDER BY last_name, first_name;



-- #2d

SELECT country_id, country

FROM country

WHERE country IN ('Afghanistan', 'Bangladesh', 'China');



-- #3a

ALTER TABLE actor

ADD COLUMN middle_name VARCHAR(50);



-- #3b

ALTER TABLE actor

MODIFY middle_name BLOB;



-- #3c

ALTER TABLE actor

DROP COLUMN middle_name;



-- #4a 

SELECT last_name, 
COUNT(last_name) as "Count_Last Name"

FROM actor

GROUP BY last_name;




-- #4b

SELECT last_name, 
COUNT(last_name) as "Count_Last_Name"

FROM actor

GROUP BY last_name

HAVING COUNT(last_name) >=2;



-- #4c

UPDATE actor

SET first_name = 'HARPO'

WHERE first_name = 'GROUCHO' AND last_name = 'WILLIAMS';



-- #4d
 
UPDATE actor
 
SET first_name = 

CASE 
 
WHEN first_name = 'HARPO' 
 
THEN 'GROUCHO'
 
ELSE 'MUCHO GROUCHO'
 
END
 WHERE actor_id = 172;



-- #5a

SHOW CREATE TABLE sakila.address;



-- #6a

SELECT first_name, last_name, address

FROM staff s

INNER JOIN address a

ON s.address_id = a.address_id;



-- #6b

SELECT first_name, last_name, 
SUM(amount)
FROM staff s

INNER JOIN payment p

ON s.staff_id = p.staff_id

GROUP BY p.staff_id

ORDER BY last_name ASC;



-- #6c

SELECT title, COUNT(actor_id)

FROM film f

INNER JOIN film_actor fa

ON f.film_id = fa.film_id

GROUP BY title;



-- #6d

SELECT title, COUNT(inventory_id)

FROM film f

INNER JOIN inventory i 

ON f.film_id = i.film_id

WHERE title = "Hunchback Impossible";



-- #6e

SELECT last_name, first_name, SUM(amount)

FROM payment p

INNER JOIN customer c

ON p.customer_id = c.customer_id

GROUP BY p.customer_id

ORDER BY last_name ASC;



-- #7a

SELECT title FROM film

WHERE language_id in
 (SELECT language_id 
FROM language
 WHERE name = "English")

AND (title LIKE "K%") OR (title LIKE "Q%");



-- #7b

SELECT last_name, first_name

FROM actor

WHERE actor_id in
 (SELECT actor_id FROM film_actor
 WHERE film_id in 
		
	(SELECT film_id FROM film
	
	WHERE title = "Alone Trip")
);
        


-- #7c

SELECT country, last_name, first_name, email

FROM country c

LEFT JOIN customer cu

ON c.country_id = cu.customer_id

WHERE country = 'Canada';



-- #7d

SELECT title, category

FROM film_list

WHERE category = 'Family';
		



-- #7e

SELECT i.film_id, f.title, 
COUNT(r.inventory_id)

FROM inventory i

INNER JOIN rental r

ON i.inventory_id = r.inventory_id

INNER JOIN film_text f 

ON i.film_id = f.film_id

GROUP BY r.inventory_id

ORDER BY COUNT(r.inventory_id) DESC;



-- #7f

SELECT store.store_id, SUM(amount)

FROM store

INNER JOIN staff

ON store.store_id = staff.store_id

INNER JOIN payment p 

ON p.staff_id = staff.staff_id

GROUP BY store.store_id

ORDER BY SUM(amount);



-- #7g

SELECT s.store_id, city, country

FROM store s

INNER JOIN customer cu

ON s.store_id = cu.store_id

INNER JOIN staff st

ON s.store_id = st.store_id

INNER JOIN address a

ON cu.address_id = a.address_id

INNER JOIN city ci

ON a.city_id = ci.city_id

INNER JOIN country coun

ON ci.country_id = coun.country_id;

WHERE country_id = 75;




-- #7h

SELECT name, SUM(p.amount)

FROM category c

INNER JOIN film_category fc

INNER JOIN inventory i

ON i.film_id = fc.film_id

INNER JOIN rental r

ON r.inventory_id = i.inventory_id

INNER JOIN payment p

GROUP BY name
LIMIT 5;



-- #8a

CREATE VIEW top_5_revenue_genres AS

SELECT name, SUM(p.amount)

FROM category c

INNER JOIN film_category fc

INNER JOIN inventory i

ON i.film_id = fc.film_id

INNER JOIN rental r

ON r.inventory_id = i.inventory_id

INNER JOIN payment p

GROUP BY name
LIMIT 5;



-- #8b

SELECT * FROM top_5_revenue_genres;



-- #8c

DROP VIEW top_5_revenue_genres;



