# Домашнее задание к занятию 4 «Оркестрация группой Docker контейнеров на примере Docker Compose» - `Натетков Александр`



## Задание 1. 

Сценарий выполнения задачи:

 - Установите docker и docker compose plugin на свою linux рабочую станцию или ВМ.
 - Зарегистрируйтесь и создайте публичный репозиторий с именем "custom-nginx" на https://hub.docker.com;
 - скачайте образ nginx:1.21.1;
 - Создайте Dockerfile и реализуйте в нем замену дефолтной индекс-страницы(/usr/share/nginx/html/index.html), на файл index.html с содержимым:
```
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I will be DevOps Engineer!</h1>
</body>
</html>
```
 - Соберите и отправьте созданный образ в свой dockerhub-репозитории c tag 1.0.0 .
 - Предоставьте ответ в виде ссылки на https://hub.docker.com/<username_repo>/custom-nginx/general .


## Ответ 1. 

https://hub.docker.com/repository/docker/karapuze/custom-nginx/general


## Задание 2.

1. Запустите ваш образ custom-nginx:1.0.0 командой docker run в соответвии с требованиями:
 - имя контейнера "ФИО-custom-nginx-t2"
 - контейнер работает в фоне
 - контейнер опубликован на порту хост системы 127.0.0.1:8080
2. Переименуйте контейнер в "custom-nginx-t2"
3. Выполните команду date +"%d-%m-%Y %T.%N %Z" ; sleep 0.150 ; docker ps ; ss -tlpn | grep 127.0.0.1:8080  ; docker logs custom-nginx-t2 -n1 ; docker exec -it custom-nginx-t2 base64 /usr/share/nginx/html/index.html
4. Убедитесь с помощью curl или веб браузера, что индекс-страница доступна.
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Ответ 2. 

![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-23%20в%2008.47.38.png)

![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-23%20в%2008.47.56.png)


## Задание 3.

1. Воспользуйтесь docker help или google, чтобы узнать как подключиться к стандартному потоку ввода/вывода/ошибок контейнера "custom-nginx-t2".
2. Подключитесь к контейнеру и нажмите комбинацию Ctrl-C.
3. Выполните docker ps -a и объясните своими словами почему контейнер остановился.
4. Перезапустите контейнер
5. Зайдите в интерактивный терминал контейнера "custom-nginx-t2" с оболочкой bash.
6. Установите любимый текстовый редактор(vim, nano итд) с помощью apt-get.
7. Отредактируйте файл "/etc/nginx/conf.d/default.conf", заменив порт "listen 80" на "listen 81".
8. Запомните(!) и выполните команду nginx -s reload, а затем внутри контейнера curl http://127.0.0.1:80 ; curl http://127.0.0.1:81.
9. Выйдите из контейнера, набрав в консоли  exit или Ctrl-D.
10. Проверьте вывод команд: ss -tlpn | grep 127.0.0.1:8080 , docker port custom-nginx-t2, curl http://127.0.0.1:8080. Кратко объясните суть возникшей проблемы.
 * Это дополнительное, необязательное задание. Попробуйте самостоятельно исправить конфигурацию контейнера, используя доступные источники в интернете. Не изменяйте конфигурацию nginx и не удаляйте контейнер.    Останавливать контейнер можно. пример источника
11. Удалите запущенный контейнер "custom-nginx-t2", не останавливая его.(воспользуйтесь --help или google)
В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.


## Ответ 3.

1-6. ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-24%20в%2015.07.36.png)

Контейнер остановился т.к. в терминале был отправлен сигнал на завершение процесса.

7-9. ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-24%20в%2015.09.13.png)

10-11. ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-24%20в%2015.14.44.png)


## Задание 4.

1. Запустите первый контейнер из образа centos c любым тегом в фоновом режиме, подключив папку текущий рабочий каталог $(pwd) на хостовой машине в /data контейнера, используя ключ -v.
2. Запустите второй контейнер из образа debian в фоновом режиме, подключив текущий рабочий каталог $(pwd) в /data контейнера.
3. Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data.
4. Добавьте ещё один файл в текущий каталог $(pwd) на хостовой машине.
5. Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

В качестве ответа приложите скриншоты консоли, где видно все введенные команды и их вывод.

## Ответ 4.

 ![Alt text](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202024-04-24%20в%2017.39.11.png)

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

