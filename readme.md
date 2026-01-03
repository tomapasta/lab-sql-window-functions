-- ## Challenge 1
-- This challenge consists of three exercises that will test your ability to use the SQL RANK() function. 
-- You will use it to rank films by their length, their length within the rating category, and by the actor or actress who has acted in the greatest number of films.
-- 1. Rank films by their length and create an output table that includes the title, length, and rank columns only. Filter out any rows with null or zero values in the length column.
CREATE OR REPLACE VIEW length_view AS SELECT title, length, RANK() OVER (ORDER BY length DESC) AS Film_Rank FROM sakila.film
WHERE  length <> ' ';
-- checked if it worked : SELECT * from length_view LIMIT 15; 

-- 2. Rank films by length within the rating category and create an output table that 
-- includes the title, length, rating and rank columns only. 
-- Filter out any rows with null or zero values in the length column.
CREATE OR REPLACE VIEW ranking_view AS SELECT f.length, f.title, f.rating, RANK() OVER (partition by f.rating ORDER BY l.Film_Rank) as Per_Rating  
FROM sakila.film as f
JOIN length_view as l 
on f.length = l.length 
WHERE  f.length <> ' ';
-- checked if it worked : SELECT * FROM ranking_view LIMIT 30 ; 

-- 3. Produce a list that shows for each film in the Sakila database, (film_id, film) the actor or actress (actor, actor_id) who has acted in the greatest number of films, 
-- as well as the total number of films in which they have acted (film_actor (actor_id, film_id), acted most, total number of films they have acted 
-- *Hint: Use temporary tables, CTEs, or Views when appropiate to simplify your queries.*

SELECT * FROM sakila.film_actor; 
CREATE OR REPLACE VIEW film_actor_view AS SELECT OVER (PARTITION BY actor_id ORDER BY film_id)af.actor_id, COUNT(af.actor_id) as n_of_film, f.film_id
FROM sakila.film as f 
JOIN sakila.film_actor as af
on f.film_id = af.film_id 
GROUP BY f.film_id;
SELECT * FROM film_actor_view; 
## Challenge 2
-- This challenge involves analyzing customer activity and retention in the Sakila database to gain insight into business performance. 
-- By analyzing customer behavior over time, businesses can identify trends and make data-driven decisions to improve customer retention and increase revenue.
-- The goal of this exercise is to perform a comprehensive analysis of customer activity and retention 
-- by conducting an analysis on the monthly percentage change in the number of active customers and the number of retained customers. 
-- Use the Sakila database and progressively build queries to achieve the desired outcome. 
-- Step 1. Retrieve the number of monthly active customers, i.e., the number of unique customers who rented a movie in each month.
-- Step 2. Retrieve the number of active users in the previous month.
-- Step 3. Calculate the percentage change in the number of active customers between the current and previous month.
-- Step 4. Calculate the number of retained customers every month, i.e., customers who rented movies in the current and previous months.




