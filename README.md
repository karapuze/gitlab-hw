# Домашнее задание к занятию ««SQL»» - `Натетков Александр`



### Задание 1. 

Используя Docker, поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.

Приведите получившуюся команду или docker-compose-манифест.

### Ответ 1. 

```
sudo docker run -d     --name postgresql-instance     -v ./sql/data:/var/lib/postgresql/data     -v ./sql/backups:/var/lib/postgresql/backups     -e POSTGRES_PASSWORD=mysecretpassword     -p 5432:5432     postgres:12
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
