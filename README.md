# Домашнее задание к занятию «Работа с данными (DDL/DML) - `Натетков Александр`


### Задание 1. 

1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.
```
sudo apt isntall mysql
```
1.2. Создайте учётную запись sys_temp.
```
CREATE USER 'sys_temp'@'localhost' IDENTIFIED BY 'password';
```
1.3. Выполните запрос на получение списка пользователей в базе данных.
```
SELECT User,Host FROM mysql.user;
```
![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-11-01%20в%2010.40.42.png)

1.4. Дайте все права для пользователя sys_temp.
```
grant all privileges on *.* to 'sys_temp'@'localhost';
```
1.5. Выполните запрос на получение списка прав для пользователя sys_temp.
```
show grants for 'sys_temp'@'localhost';
```
![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-11-01%20в%2010.48.54.png)

1.6. Переподключитесь к базе данных от имени sys_temp.
```
mysql -u sys_temp -p
```
1.7. Восстановите дамп в базу данных.
```
sudo mysql db -uroot -p < /home/parallels/sakila-db/sakila-schema.sql sudo mysql db -uroot -p < /home/parallels/sakila-db/sakila-data.sql
```
1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных.
```
show tables from sakila;
```
![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-11-01%20в%2014.28.56.png)

	Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.
	Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.




---

### Задание 2. Kibana

	Установите и запустите Kibana.
 	Приведите скриншот интерфейса Kibana на странице http://<ip вашего сервера>:5601/app/dev_tools#/console, где будет выполнен запрос GET /_cluster/health?pretty.

![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-10-15%20в%2011.34.50.png)


---

### Задание 3. Logstash

	Установите и запустите Logstash и Nginx. С помощью Logstash отправьте access-лог Nginx в Elasticsearch.
 	Приведите скриншот интерфейса Kibana, на котором видны логи Nginx.


![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-10-15%20в%2015.37.39.png)


---

### Задание 4. Filebeat

	Установите и запустите Filebeat. Переключите поставку логов Nginx с Logstash на Filebeat.
 	Приведите скриншот интерфейса Kibana, на котором видны логи Nginx, которые были отправлены через Filebeat.

![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-10-17%20в%2012.17.46.png)


 
