# Домашнее задание к занятию «SQL. Часть 2» - `Натетков Александр`



### Задание 1. 

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:

 - фамилия и имя сотрудника из этого магазина;
 - город нахождения магазина;
 - количество пользователей, закреплённых в этом магазине.

```
SELECT COUNT(*) AS customers_count, stf.first_name, stf.last_name, ci.city
FROM customer c
JOIN store s ON c.store_id = s.store_id
JOIN staff stf ON s.manager_staff_id = stf.staff_id
JOIN address a ON s.address_id = a.address_id
JOIN city ci ON a.city_id = ci.city_id
GROUP BY c.store_id
HAVING customers_count > 300;

```

### Задание 2.

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.


```
SELECT COUNT(*) AS count_of_records
FROM film
WHERE length > (SELECT AVG(length) FROM film);

```

### Задание 3.

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```

SELECT max_payment_month, COUNT(DISTINCT rental_id) AS unique_rental_count
FROM (
    SELECT DATE_FORMAT(payment_date, '%Y-%m') AS max_payment_month, rental_id
    FROM payment
) AS subquery
GROUP BY max_payment_month
ORDER BY unique_rental_count DESC
LIMIT 1;
```

