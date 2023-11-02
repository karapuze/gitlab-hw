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

	

---

### Задание 2. 

Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц.

```
 TABLE_NAME    	 COLUMN_NAME  
 actor         	 actor_id     
 address       	 address_id   
 category      	 category_id  
 city          	 city_id      
 country       	 country_id   
 customer      	 customer_id  
 film          	 film_id      
 film_actor    	 actor_id     
 film_actor    	 film_id      
 film_category 	 category_id  
 film_category 	 film_id      
 film_text     	 film_id      
 inventory     	 inventory_id 
 language      	 language_id  
 payment       	 payment_id   
 rental        	 rental_id    
 staff         	 staff_id     
 store         	 store_id


```



 
