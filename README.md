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
