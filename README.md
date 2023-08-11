# Домашнее задание к занятию "Система мониторинга Zabbix" - `Натетков Александр`


### Задание 1

Установите Zabbix Server с веб-интерфейсом.

1. Cкриншот авторизации в админке
<img src = "img/Снимок экрана 2023-08-11 в 14.31.51.png" width = 100%>
<img src = "img/Снимок экрана 2023-08-11 в 14.31.40.png" width = 100%>

2. Текст использованных команд в GitHub
   Не очень понял задание, какие команды тут нужны? Команды которые использовались на машине для выполнения задания или команды для работы с Git'ом?
```git
   git add
   git commit -m
   git push origin
```
```bash
   98 sudo apt update
   99  sudo apt install postgresql
  100  wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
  101  sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb.1
  102  sudo apt update
  103  sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-nginx-conf zabbix-sql-scripts zabbix-agent
  104  sudo -u postgres createuser --pwprompt zabbix
  105  sudo -u postgres createdb -O zabbix zabbix
  106  su - postgres
  107  sudo -u postgres createdb -O zabbix zabbix
  108  zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
  109  sudo vim /etc/zabbix/zabbix_server.conf
  110  sudo systemctl restart zabbix-server.service zabbix-agent.service nginx.service php8.1-fpm.service
  111  sudo systemctl status nginx.service
  112  sudo systemctl restart nginx.service
  113  sudo systemctl status nginx.service
  114  sudo systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm
  115  cat /etc/zabbix/zabbix_server.conf
  116  sudo cat /etc/zabbix/zabbix_server.conf
  117  sudo cat /etc/zabbix/nginx.conf
  118  sudo cat /etc/zabbix/nginx.conf | grep port
  119  sudo cat /etc/zabbix/nginx.conf
  120  sudo vim /etc/zabbix/nginx.conf
  121  sudo systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
  122  sudo vim /etc/zabbix/nginx.conf
```


---

### Задание 2

С помощью Yandex Monitoring сделайте 2 алерта на загрузку процессора: WARN и ALARM. Создайте уведомление по e-mail.

1. Скриншот уведомления в Yandex Monitoring
<img src = "img/Снимок экрана 2023-08-08 в 09.17.28.png" width = 100%>
