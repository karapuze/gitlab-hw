# Домашнее задание к занятию «ELK» - `Натетков Александр`


### Задание 1. Elasticsearch

	Установите и запустите Elasticsearch, после чего поменяйте параметр cluster_name на случайный.
	Приведите скриншот команды 'curl -X GET 'localhost:9200/_cluster/health?pretty', сделанной на сервере с установленным Elasticsearch. Где будет виден нестандартный cluster_name.

![Скриншот-1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-10-15%20в%2011.01.01.png)


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


 
