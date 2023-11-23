# Домашнее задание к занятию «Индексы» - `Натетков Александр`



### Задание 1. 

Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.



```
SELECT
    ROUND(SUM(index_length) / SUM(data_length + index_length) * 100, 2) AS index_percentage
FROM
    information_schema.tables
WHERE
    table_schema = 'sakila';

```

### Задание 2.

Выполните explain analyze следующего запроса:

```
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```
 - перечислите узкие места;
 - оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.

1. Использование неявных соединений через запятую может привести к кросс-джойнам (декартовым произведениям), что делает запрос менее эффективным. Лучше использовать явные операторы JOIN для улучшения читаемости и производительности.
2. Вместо использования функции date(), лучше сравнивать напрямую с датой без времени.
3. Отсутствие явного указания типа JOIN:
В оригинальном запросе отсутствует явное указание типа JOIN. В зависимости от контекста, это может привести к неявным LEFT JOIN или INNER JOIN. Рекомендуется всегда явно указывать тип JOIN для ясности.


Оптимизированный запрос:
```
EXPLAIN ANALYZE
SELECT DISTINCT
       CONCAT(c.last_name, ' ', c.first_name) AS customer_name,
       SUM(p.amount) OVER (PARTITION BY c.customer_id, f.title) AS total_amount
FROM payment p
JOIN rental r ON p.payment_date = r.rental_date
JOIN customer c ON r.customer_id = c.customer_id
JOIN inventory i ON r.inventory_id = i.inventory_id
JOIN film f ON i.film_id = f.film_id
WHERE DATE(p.payment_date) = '2005-07-30';

```

