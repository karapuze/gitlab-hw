# Домашнее задание к занятию 3. «MySQL» - `Натетков Александр`



### Задание 1. 

1. Найдите команду для выдачи статуса БД и приведите в ответе из её вывода версию сервера БД.
2. Подключитесь к восстановленной БД и получите список таблиц из этой БД.
3. Приведите в ответе количество записей с price > 300.

### Ответ 1. 

1. \s Server version:		8.3.0 MySQL Community Server - GPL
2.
```
mysql> SHOW TABLES;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.01 sec)
```
3.
```
mysql> select count(*) as totalorders
    -> from orders
    -> where price > 300;
+-------------+
| totalorders |
+-------------+
|           1 |
+-------------+
1 row in set (0.02 sec)
```



### Задание 2.

В БД из задачи 1:

1. создайте пользователя test-admin-user и БД test_db;
2. в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже);
3. предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db;
4. создайте пользователя test-simple-user;
5. предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE этих таблиц БД test_db.

Приведите:

1. итоговый список БД после выполнения пунктов выше;
2. описание таблиц (describe);
3. SQL-запрос для выдачи списка пользователей с правами над таблицами test_db;
4. список пользователей с правами над таблицами test_db.

### Ответ 2. 

1. ![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-02-04%20в%2008.56.49.png)
2. ![Скриншот-2](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-02-04%20в%2009.02.00.png)
3.
    ```
   SELECT grantee, privilege_type 
   FROM information_schema.table_privileges 
   WHERE table_schema = 'public' AND table_name IN ('orders', 'clients');
   ```
5. ![Скриншот-3](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-02-04%20в%2009.06.01.png)


### Задание 3.

Используя SQL-синтаксис, наполните таблицы следующими тестовыми данными, вычислите количество записей для каждой таблицы.

Приведите в ответе:

- запросы,
- результаты их выполнения.

### Ответ 3.

```
INSERT INTO orders (name, price)
VALUES 
    ('Шоколад', 10),
    ('Принтер', 3000),
    ('Книга', 500),
    ('Монитор', 7000),
    ('Гитара', 4000);
```
```
INSERT INTO clients (last_name, country)
VALUES 
    ('Иванов Иван Иванович', 'USA'),
    ('Петров Петр Петрович', 'Canada'),
    ('Иоганн Себастьян Бах', 'Japan'),
    ('Ронни Джеймс Дио', 'Russia'),
    ('Ritchie Blackmore', 'Russia');
```
```
SELECT COUNT(*) FROM orders;
```
```
SELECT COUNT(*) FROM clients;
```
![Скриншот-4](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-02-04%20в%2009.16.58.png)

### Задание 4.

Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys, свяжите записи из таблиц

Приведите SQL-запросы для выполнения этих операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод этого запроса.

### Ответ 4.

```
ALTER TABLE orders
ADD COLUMN client_id INTEGER;
```
```
UPDATE orders
SET client_id = (SELECT id FROM clients WHERE last_name = 'Иванов Иван Иванович')
WHERE name = 'Книга';

UPDATE orders
SET client_id = (SELECT id FROM clients WHERE last_name = 'Петров Петр Петрович')
WHERE name = 'Монитор';

UPDATE orders
SET client_id = (SELECT id FROM clients WHERE last_name = 'Иоганн Себастьян Бах')
WHERE name = 'Гитара';
```

![Скриншот-5](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-02-04%20в%2009.19.56.png)

### Задание 5. 
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните, что значат полученные значения.

### Ответ 5.
```
EXPLAIN SELECT c.last_name
FROM clients c
WHERE c.id IN (SELECT client_id FROM orders);
                             QUERY PLAN                              
---------------------------------------------------------------------
 Hash Semi Join  (cost=13.15..25.52 rows=100 width=516)
   Hash Cond: (c.id = orders.client_id)
   ->  Seq Scan on clients c  (cost=0.00..11.00 rows=100 width=520)
   ->  Hash  (cost=11.40..11.40 rows=140 width=4)
         ->  Seq Scan on orders  (cost=0.00..11.40 rows=140 width=4)
(5 rows)
```
Этот вывод EXPLAIN предоставляет план выполнения запроса, включая информацию о том, как база данных будет извлекать и объединять данные для выполнения запроса.

1. "Hash Semi Join" - это тип соединения используемый для объединения двух таблиц. 
2. "cost=13.15..25.52 rows=100 width=516" - это оценка стоимости выполнения операции. 
3. "Hash Cond: (c.id = orders.client_id)" - это условие объединения таблиц по значениям полей c.id и orders.client_id.
4. "Seq Scan on clients c" - это операция последовательного сканирования таблицы clients для выполнения запроса.
5. "Seq Scan on orders" - это операция последовательного сканирования таблицы orders для выполнения запроса.

### Задание 6. 
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. задачу 1).

Остановите контейнер с PostgreSQL, но не удаляйте volumes.

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

### Ответ 6.

1. ``` pg_dump test_db > /var/lib/postgresql/backups/backup.sql```
2. ```sudo docker stop postgresql-instance```
3. ```sudo docker run -d     --name postgresql-new     -v ./sql/data:/var/lib/postgresql/data     -v ./sql/backups:/var/lib/postgresql/backups     -e POSTGRES_PASSWORD=mysecretpassword     -p 5432:5432     postgres:12```
4. ```docker exec -i postgresql-new psql -U postgres test_db < /var/lib/postgresql/backups/backup.sql```




