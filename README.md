# Домашнее задание к занятию 5. «Практическое применение Docker» - `Натетков Александр`



## Задание 1. 

1. Сделайте в своем github пространстве fork репозитория https://github.com/netology-code/shvirtd-example-python/blob/main/README.md.
2. Создайте файл с именем Dockerfile.python для сборки данного проекта(для 3 задания изучите https://docs.docker.com/compose/compose-file/build/ ). Используйте базовый образ python:3.9-slim.
3. Протестируйте корректность сборки. Не забудьте dockerignore.
4. (Необязательная часть, *) Изучите инструкцию в проекте и запустите web-приложение без использования docker в venv. (Mysql БД можно запустить в docker run).
5. (Необязательная часть, *) По образцу предоставленного python кода внесите в него исправление для управления названием используемой таблицы через ENV переменную.



## Задание 3.

1. Изучите файл "proxy.yaml"
2. Создайте в репозитории с проектом файл compose.yaml. С помощью директивы "include" подключите к нему файл "proxy.yaml".
3. Опишите в файле compose.yaml следующие сервисы:
 - web. Образ приложения должен ИЛИ собираться при запуске compose из файла Dockerfile.python ИЛИ скачиваться из yandex cloud container registry(из задание №2 со *). Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.5. Сервис должен всегда перезапускаться в случае ошибок. Передайте необходимые ENV-переменные для подключения к Mysql базе данных по сетевому имени сервиса web
 - db. image=mysql:8. Контейнер должен работать в bridge-сети с названием backend и иметь фиксированный ipv4-адрес 172.20.0.10. Явно перезапуск сервиса в случае ошибок. Передайте необходимые ENV-переменные для создания: пароля root пользователя, создания базы данных, пользователя и пароля для web-приложения.Обязательно используйте уже существующий .env file для назначения секретных ENV-переменных!
4. Запустите проект локально с помощью docker compose , добейтесь его стабильной работы: команда curl -L http://127.0.0.1:8090 должна возвращать в качестве ответа время и локальный IP-адрес. Если сервисы не стартуют воспользуйтесь командами: docker ps -a  и docker logs <container_name>
5. Подключитесь к БД mysql с помощью команды docker exec <имя_контейнера> mysql -uroot -p<пароль root-пользователя> . Введите последовательно команды (не забываем в конце символ ; ): show databases; use <имя вашей базы данных(по-умолчанию example)>; show tables; SELECT * from requests LIMIT 10;.
6. Остановите проект. В качестве ответа приложите скриншот sql-запроса.

## Ответ 3. 

![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-05-03%20в%2015.28.09.png)




## Задание 4.

1. Запустите в Yandex Cloud ВМ (вам хватит 2 Гб Ram).
2. Подключитесь к Вм по ssh и установите docker.
3. Напишите bash-скрипт, который скачает ваш fork-репозиторий в каталог /opt и запустит проект целиком.
4. Зайдите на сайт проверки http подключений, например(или аналогичный): https://check-host.net/check-http и запустите проверку вашего сервиса http://<внешний_IP-адрес_вашей_ВМ>:8090. Таким образом трафик будет направлен в ingress-proxy.
5. (Необязательная часть) Дополнительно настройте remote ssh context к вашему серверу. Отобразите список контекстов и результат удаленного выполнения docker ps -a
6. В качестве ответа повторите sql-запрос и приложите скриншот с данного сервера, bash-скрипт и ссылку на fork-репозиторий.


## Ответ 4.

#### bash script
```
#!/bin/bash

cd /opt
git clone https://github.com/karapuze/shvirtd-example-python.git
cd /opt/shvirtd-example-python/
docker compose up -d
```
Выдал права пользователю на запись в /opt


[fork-репозитория](https://github.com/karapuze/shvirtd-example-python)


![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-05-03%20в%2017.31.04.png)



## Задание 6.

Скачайте docker образ hashicorp/terraform:latest и скопируйте бинарный файл /bin/terraform на свою локальную машину, используя dive и docker save. Предоставьте скриншоты действий .

## Ответ 6.

 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-05-03%20в%2019.01.53.png)

 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-05-03%20в%2019.02.28.png)

## Задание 5. 

1. Создайте отдельную директорию(например /tmp/netology/docker/task5) и 2 файла внутри него. 
"compose.yaml" с содержимым:

```
version: "3"
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

"docker-compose.yaml" с содержимым:

```
version: "3"
services:
  registry:
    image: registry:2
    network_mode: host
    ports:
    - "5000:5000"
```   
И выполните команду "docker compose up -d". Какой из файлов был запущен и почему?

2. Отредактируйте файл compose.yaml так, чтобы были запущенны оба файла.

3. Выполните в консоли вашей хостовой ОС необходимые команды чтобы залить образ custom-nginx как custom-nginx:latest в запущенное вами, локальное registry. 

4. Откройте страницу "https://127.0.0.1:9000" и произведите начальную настройку portainer.(логин и пароль адмнистратора)

5. Откройте страницу "http://127.0.0.1:9000/#!/home", выберите ваше local окружение. Перейдите на вкладку "stacks" и в "web editor" задеплойте следующий компоуз:

```
version: '3'

services:
  nginx:
    image: 127.0.0.1:5000/custom-nginx
    ports:
      - "9090:80"
```
6. Перейдите на страницу "http://127.0.0.1:9000/#!/2/docker/containers", выберите контейнер с nginx и нажмите на кнопку "inspect". В представлении <> Tree разверните поле "Config" и сделайте скриншот от поля "AppArmorProfile" до "Driver".

7. Удалите любой из манифестов компоуза(например compose.yaml). Выполните команду "docker compose up -d". Прочитайте warning, объясните суть предупреждения и выполните предложенное действие. Погасите compose-проект ОДНОЙ(обязательно!!) командой.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод, файл compose.yaml , скриншот portainer c задеплоенным компоузом.

### Ответ 5.

1. Был запущен compose.yml т.к. он считается предпочтительным, а docker-compose поддерживается для обратной совместимости.
 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2011.24.56.png)
 
2. Добавил include
 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2011.58.00.png)


3. 
 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2012.12.13.png)

4. Было бы здорово если бы вы проверяли задачи, потому что по умолчанию необходимо использовать не https, а http

6.  
![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2012.18.29.png)

7. После удаления compose.yml и выполнения docker compose up -d, получил предупреждение об "осиротевших" контейнерах, а именно portainer, было предложено выпонлить команду используя флаг --remove-orphans для удаления "осиротевших" контейнеров.
![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2012.38.51.png)

Portainer с задеплоеным nginx
![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-29%20в%2012.42.29.png)

Compose.yml
```
version: "3"
include:
  - docker-compose.yaml
services:
  portainer:
    image: portainer/portainer-ce:latest
    network_mode: host
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```

