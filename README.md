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






