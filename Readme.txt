======= THIS query to find out the most popular music Genre for each country ordered by country.

WITH table1 AS
  (SELECT c.Country country,
          COUNT(*) AS sellings,
          g.Name genre,
          g.GenreId Genre_id
   FROM Track t
   JOIN Genre g ON t.GenreId = g.GenreId
   JOIN InvoiceLine il ON il.TrackId = t.TrackId
   JOIN Invoice i ON i.InvoiceId = il.InvoiceId
   JOIN Customer c ON c.CustomerId = i.CustomerId
   GROUP BY 3,
            1
   ORDER BY 1)
SELECT country,
       genre,
       MAX(sellings) AS 'Most popular Genre'
FROM table1
GROUP BY 1
ORDER BY 1;
-----------------------------------------------------------
======= This query to identify first name, last name, email and genre of all rock listeners AND Latin genres.

SELECT COUNT(i.InvoiceId)'number of orders',
                         c.FirstName,
                         c.LastName,
                         c.Email,
                         g.Name genre
FROM Track t
JOIN Genre g ON t.GenreId = g.GenreId
JOIN InvoiceLine il ON il.TrackId = t.TrackId
JOIN Invoice i ON i.InvoiceId = il.InvoiceId
JOIN Customer c ON c.CustomerId = i.CustomerId
WHERE genre = 'Rock'
  OR genre = 'Latin'
GROUP BY 3,
         4,
         5
ORDER BY 4;
-----------------------------------------------------
======This query is to Return all the track names that have a song length longer than the average song length and their genre

SELECT t.Milliseconds AS 'song time',
       t.name track,
       g.name genre
FROM Track t
JOIN Genre g ON t.GenreId = g.GenreId
GROUP BY 2
HAVING t.Milliseconds >
  (SELECT AVG(milliseconds) AS avg_all
   FROM Track)
ORDER BY 1 DESC;

--------------------------------------------------------
=========== This query to determine the customer that has spent the most on music for each country.

SELECT country,
       MAX(sum_of_total) AS 'highest paying',
       name
FROM
  (SELECT SUM(i.total) sum_of_total,
          i.BillingCountry country,
          c.FirstName name
   FROM invoice i
   JOIN Customer c ON c.CustomerId = i.CustomerId
   GROUP BY 2)t1
GROUP BY 1
ORDER BY 1;
-------------------------------------------------