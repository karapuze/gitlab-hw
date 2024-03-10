# Домашнее задание к занятию 4. «PostgreSQL» - `Натетков Александр`



### Задание 1. 

Используя Docker, поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.
Подключитесь к БД PostgreSQL, используя psql.
Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

 - вывода списка БД,
 - подключения к БД,
 - вывода списка таблиц,
 - вывода описания содержимого таблиц,
 - выхода из psql.

### Ответ 1. 

 - вывод списка БД - \l
 - подключение к БД - \c dbname
 - вывод списка таблиц - \dt
 - вывод описания содержимого таблиц - \d tablename
 - выход из psql - \q


### Задание 2.

1. Используя psql, создайте БД test_database.
2. Восстановите бэкап БД в test_database.
3. Перейдите в управляющую консоль psql внутри контейнера.
4. Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
5. Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления, и полученный результат.

### Ответ 2. 

```
test_database=# SELECT attname, avg_width FROM pg_stats WHERE tablename='orders' ORDER BY avg_width DESC LIMIT 1;
 attname | avg_width 
---------+-----------
 title   |        16
(1 row)


```

### Задание 3.

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам как успешному выпускнику курсов DevOps в Нетологии предложили провести разбиение таблицы на 2: шардировать на orders_1 - price>499 и orders_2 - price<=499.

1. Предложите SQL-транзакцию для проведения этой операции.

2. Можно ли было изначально исключить ручное разбиение при проектировании таблицы orders?

### Ответ 3.

1.
```
test_database=# BEGIN TRANSACTION;
BEGIN
test_database=*# CREATE TABLE orders_1 AS SELECT * FROM orders WHERE price > 499;
SELECT 3
test_database=*# CREATE TABLE orders_2 AS SELECT * FROM orders WHERE price <= 499;
SELECT 5
test_database=*# DROP TABLE orders;
DROP TABLE
test_database=*# COMMIT;
COMMIT
```
2. Vожно было изначально исключить ручное разбиение при проектировании таблицы orders. Это можно было сделать с помощью партиционирования. Мы могли бы использовать партиционирование по столбцу price, чтобы автоматически разделить данные на разные таблицы в зависимости от значения цены
```
PARTITION BY RANGE (price) (
    PARTITION p0 VALUES LESS THAN (500),
    PARTITION p1 VALUES LESS THAN MAXVALUE
```

### Задание 4.

1. Используя утилиту pg_dump, создайте бекап БД test_database.

2. Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

### Ответ 4.

1.
```
root@117cab25e716:/# pg_dump -U postgres -W -F t test_database > /var/lib/postgresql/data/test_database.bak
```
2.
```
test_database=# ALTER TABLE orders_1
test_database-# ADD CONSTRAINT title_unique UNIQUE (title);
```
