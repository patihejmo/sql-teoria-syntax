# sql-teoria-syntax 

select statement - wybiera, distinct daje tylko odmienne dane ktore nie moga sie powtarzac; unique values 

select count() liczy

and/or/not, where condition, capitalization matter
i.e. select * from customer
where first_name = 'Jared'


comparison operators;
=
!= not equal to
<
>
>=
=<

W SQL UZYWAMY ' ' NIE " "

order by - sorting by a column value, desc/asc; numerical or alphabetical - NA KONCU, ASC na default

LIMIT - uzywamy JAKO OSTATNIA KOMEMNDA 

LIMIT 10 (do 10)

BEETWEN low AND high - to ssamo co >= AND <= ------> inclusive
NOT BETWEEN low AND high - to samo co > AND <  ---------> exclusive

ISO 8601 - dates are written as follows YYYY-MM-DD

SELECT amount from payment
where amount in (0.99, 1.98, 1.99)

wildcards - like/ilike

SELECT first_name from customer
where first_name like 'A%' AND first_name like '%a'
its very sensitive - capital letter matter

'_ cos _'

# JOINS

# inner join
inner join allows you to join two tables which share common columns

SELECT * FROM Registrations                   >>  SELECT something FROM table1
INNER JOIN Logins                                 INNER JOIN table2
ON Registrations.name = Logins.name               ON table1.column_match = table2.column_match

you can use select statement to avoid duplications.

SELECT reg_id, Logins.name, log_id
FROM Registrations
INNER JOIN Logins
ON Registrations.name = Logins.name

in postgresql its prettty normal to write only JOIN nstead of INNER JOIN - the syntax treats it as the same

select payment_id, customer.customer_id from payment
INNER JOIN customer
ON payment.customer_id=customer.customer_id

select customer.customer_id, first_name, payment_id from customer
inner join payment
on customer.customer_id = payment.customer_id 

# outer join 
it grabs everything from both tables, whether it is in both or in only one

SELECT * FROM Registrations
FULL OUTER JOIN Logins 
ON Registrations.name = Logins.name >>>>> it will give us null values as we ask to fill data only with logins table

SELECT * FROM Registrations
FULL OUTER JOIN Logins
ON Registrations.name = Logins.name
WHERE Registrations.reg_id IS NULL OR Logins.log_id IS NULL   >>>> searches only for the unique values !!!!!!

SELECT * FROM customer
FULL OUTER JOIN payment
ON customer.customer_id=payment.customer_id
WHERE customer.customer_id IS null 
OR payment.payment_id IS null

select count (distinct customer_id) from customer

# left outer join

so if the left table is table A, we grab everything exclusive from table A + matching columns from both A and B tables

SELECT * FROM Registrations
LEFT OUTER JOIN Logins
ON Registrations.name = Logins.name >>> will give us everything from Registration table and matching things from Logins

mozna tez napisac
WHERE Logins.id IS NULL zeby byly tylko rzdezczy z A i nie powtarzajace sie, unique for A


SELECT film.film_id, film.title, inventory_id from film
left join inventory on inventory.film_id = film.film_id
where inventory.film_id is null     >>>>>>> we do not have any of these movies in the inventory


BETWEEN statement is INCLUSIVE, meaning that if i have a formula:
WHERE value BETWEEN 8 AND 10,
8 and 10 are also considered as valid.

NOT BETWEEN statement is EXCLUSIVE, so its as follows
WHERE value NOT BETWEEN 8 AND 10,
-> 8, 10, and nothing what is in between these numbers is included 

we can use BETWEEN also for dates, formatted in the ISO 8601 standard (YYYY-MM-DD)
date BETWEEN 2002-10-08 AND 2002-12-24

select count (*) from payment
where amount between 8 and 9

select * from payment
where payment_date between '2005-10-01' and '2007-10-01'
order by payment_date asc

# WAZNE !!! 
jeśli tak jak w przykladzie wyzej jest end date jako 01/10/2007, to 1/10 NIE JEST INCLUDED kiedy mamy godziny. ten format umozliwia zwrot danych do 31/09

# IN 
select * from customer
where first_name in ('David')

IN ('something')

select * from payment
where amount in/NOT IN (0.99, 1.98) -> tutaj to nie jest string tylko int wiec nie trzeba ' ' 

# LIKE AND ILIKE
 SELECT * FROM customer
 WHERE first_name LIKE 'D%' AND last_name LIKE 'D%'; 

SELECT * FROM customer
WHERE first_name LIKE '%a' AND last_name LIKE '%a';

# LIKE STATMENET IS CASE SENSITIVE, however ILIKE is not 

using % is for a bunch of characters, using _ underscore is only FOR A SINGLE CHARACTER

we can also combine these functions, i.e:
 SELECT * FROM customer
 WHERE first_name LIKE '_her%' -> it returns names like Theresa, Cheryl etc


select count (distinct(district)) from address


# AGGREGATE FUNCTIONS - GROUP BY, HAVING

the main idea is to take multiple inputs and return one single output
AVG - average value; retuns a FLOATING point (so many decimal places i.e. 1.5437) - to change it we can use ROUND ()
   select round (avg (replacement_cost), 2) from film --------> 2 here is the3 number of decimal places we want to return
COUNT 
MAX - SELECT MAX (replacement_cost) FROM film
MIN 
SUM - select sum (replacement_cost) from film 


# GROUP BY
1. first we choose the categorical column that is not continuous 
2. it must appear after FROM or WHERE statement
3. if we choose to retrieve something at the SELECT statement stage, we must include it in the GROUP BY statement
4. WHERE swtatement should refer to SELECT/GROUP BY result, not an aggregate function like sum, avg etc

SELECT customer_id, SUM (amount)
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) desc

SELECT customer_id, SUM (amount)
FROM payment
GROUP BY customer_id
ORDER BY SUM(amount) desc

# select DATE(payment_date) from payment
funckja date usuwa timestamp z daty, czyli fromatuje date na iso 8601 YYYY-MM-DD 

 SELECT staff_id, COUNT(payment_id) from payment
 WHERE staff_id IN (1,2)
 GROUP BY staff_id
 ORDER BY COUNT(payment_id) desc

 select rating, ROUND(AVG(replacement_cost),2) from film 
GROUP BY rating
order by AVG(replacement_cost) desc

select customer_id, SUM(amount) from payment
GROUP BY customer_id
ORDER BY SUM(amount) DESC
LIMIT 5 -----> retrieving only the top 5 customers who spent the most moeny in the store

# HAVING
select customer_id, sum(amount) from payment
group by customer_id
having sum(amount) > 100 --------> tutaj HAVING działa jak where i odnosi sie do tej aggregate function, where nie bedzie dzialac 


select customer_id, count (payment_id) from payment
group by customer_id
having count(payment_id) >= 40

select customer_id, sum (amount) from payment
where staff_id IN (2)
group by customer_id
having sum(amount) > 100 

AS - alias AFTER SELECT - makes the query more readable for the analyst and others

select count(amount) as num_transactions
from payment

select customer_id, sum (amount) as total_spend
from payment
group by customer_id

ALIAS dziala dopiero na koncu wykonywanego quesry. Dlatego NIEMOZLIWE jest uzycie go np. po WHERE, GROUP BY i innych warunkowych statements, bo po prostu nie znajdzie odwolania,

select customer_id, sum (amount) as total_spend
from payment
group by customer_id
having sum(amount) >100 -------->> tutaj having sum(total_spend) by nie działało
