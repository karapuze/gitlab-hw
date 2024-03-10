# Домашнее задание к занятию 3. «MySQL» - `Натетков Александр`



### Задание 1. 

1. Найдите команду для выдачи статуса БД и приведите в ответе из её вывода версию сервера БД.
2. Подключитесь к восстановленной БД и получите список таблиц из этой БД.
3. Приведите в ответе количество записей с price > 300.

### Ответ 1. 

1.
```
mysql> \s
--------------
mysql  Ver 8.3.0 for Linux on aarch64 (MySQL Community Server - GPL)

Connection id:		12
Current database:	test_db
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.3.0 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	latin1
Conn.  characterset:	latin1
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			40 min 30 sec

Threads: 4  Questions: 108  Slow queries: 0  Opens: 196  Flush tables: 3  Open tables: 114  Queries per second avg: 0.044
```
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

1. Создайте пользователя test в БД c паролем test-pass, используя:

 - плагин авторизации mysql_native_password
 - срок истечения пароля — 180 дней
 - количество попыток авторизации — 3
 - максимальное количество запросов в час — 100
 - аттрибуты пользователя:
 - Фамилия "Pretty"
 - Имя "James".
Предоставьте привелегии пользователю test на операции SELECT базы test_db.

2. Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES, получите данные по пользователю test и приведите в ответе к задаче.


### Ответ 2. 

1.
```
mysql> create user 'test'@'localhost' identified with 'mysql_native_password' by 'test-pass';
Query OK, 0 rows affected (2.51 sec)

mysql> ALTER USER 'test'@'localhost' PASSWORD EXPIRE INTERVAL 180 DAY;
Query OK, 0 rows affected (0.20 sec)

mysql> ALTER USER 'test'@'localhost' FAILED_LOGIN_ATTEMPTS 3;
Query OK, 0 rows affected (0.20 sec)

mysql> ALTER USER 'test'@'localhost' WITH MAX_QUERIES_PER_HOUR 100;
Query OK, 0 rows affected (0.05 sec)

mysql> ALTER USER 'test'@'localhost' ATTRIBUTE '{"first_name": "James", "last_name": "Pretty"}';
Query OK, 0 rows affected (0.05 sec)

mysql> GRANT SELECT ON test_db.* TO 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.02 sec)
```

2.
```
mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER = 'test';
+------+-----------+------------------------------------------------+
| USER | HOST      | ATTRIBUTE                                      |
+------+-----------+------------------------------------------------+
| test | localhost | {"last_name": "Pretty", "first_name": "James"} |
+------+-----------+------------------------------------------------+
1 row in set (0.04 sec)

```

### Задание 3.

1. Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.

2. Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.

3. Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:

 - на MyISAM,
 - на InnoDB.

### Ответ 3.

1.
```
mysql> set profiling = 1;
Query OK, 0 rows affected, 1 warning (0.03 sec)

mysql> show profiles;
Empty set, 1 warning (0.00 sec)
```
2.
```
mysql> SELECT ENGINE FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'test_db' AND TABLE_NAME = 'orders';
+--------+
| ENGINE |
+--------+
| InnoDB |
+--------+
1 row in set (0.05 sec)
```
3.
```
mysql> SHOW PROFILES;
+----------+------------+-------------------------------------------------------------------------------------------------------+
| Query_ID | Duration   | Query                                                                                                 |
+----------+------------+-------------------------------------------------------------------------------------------------------+
|        1 | 0.04362350 | SHOW TABLE STATUS LIKE 'orders'                                                                       |
|        2 | 0.03890300 | SELECT ENGINE FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'test_db' AND TABLE_NAME = 'orders' |
|        3 | 0.06766800 | ALTER TABLE orders ENGINE=MyISAM                                                                      |
|        4 | 0.02026725 | SELECT ENGINE FROM information_schema.TABLES WHERE TABLE_SCHEMA = 'test_db' AND TABLE_NAME = 'orders' |
|        5 | 0.02621150 | SHOW TABLE STATUS LIKE 'orders'                                                                       |
|        6 | 0.00330425 | SHOW TABLES                                                                                           |
|        7 | 0.01382050 | SHOW * FROM orders                                                                                    |
|        8 | 0.03136325 | SELECT * FROM orders                                                                                  |
|        9 | 0.05882475 | ALTER TABLE orders ENGINE = InnoDB                                                                    |
|       10 | 0.00129850 | SELECT * FROM orders                                                                                  |
|       11 | 0.02413725 | SHOW TABLES                                                                                           |
|       12 | 0.02502975 | SHOW TABLE STATUS LIKE 'orders'                                                                       |
+----------+------------+-------------------------------------------------------------------------------------------------------+
12 rows in set, 1 warning (0.01 sec)
```

### Задание 4.

Изучите файл my.cnf в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

 - скорость IO важнее сохранности данных;
 - нужна компрессия таблиц для экономии места на диске;
 - размер буффера с незакомиченными транзакциями 1 Мб;
 - буффер кеширования 30% от ОЗУ;
 - размер файла логов операций 100 Мб.
Приведите в ответе изменённый файл my.cnf.

### Ответ 4.

```
[mysqld]

default-storage-engine = InnoDB
innodb_flush_log_at_trx_commit = 2

#
# Remove leading # and set to the amount of RAM for the most important data
# cache in MySQL. Start at 70% of total RAM for dedicated server, else 10%.

innodb_buffer_pool_size = 4800M
#внутри контейнера после выполнения cat /proc/meminfo имеем MemTotal: 16380460 kB, выставляем 30% => 4 914 138kb => 4800M
#
innodb_file_per_table = 1
innodb_page_compression = ON

# Remove leading # to turn on a very important data integrity option: logging
# changes to the binary log between backups.
# log_bin
#
# Remove leading # to set options mainly useful for reporting servers.
# The server defaults are faster for transactions and fast SELECTs.
# Adjust sizes as needed, experiment to find the optimal values.
# join_buffer_size = 128M
# sort_buffer_size = 2M
# read_rnd_buffer_size = 2M

innodb_log_buffer_size = 1M
innodb_log_file_size = 100M

# Remove leading # to revert to previous value for default_authentication_plugin,
# this will increase compatibility with older clients. For background, see:
# https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html#sysvar_default_authentication_plugin
# default-authentication-plugin=mysql_native_password
skip-host-cache
skip-name-resolve
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
secure-file-priv=/var/lib/mysql-files
user=mysql

pid-file=/var/run/mysqld/mysqld.pid
```

