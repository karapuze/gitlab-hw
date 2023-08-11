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

Установите Zabbix Agent на два хоста.

1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
<img src = "img/Снимок экрана 2023-08-11 в 16.14.30.png" width = 100%>

2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
В данном задании столкнулся с проблемой, как видно агент подключен и отдает данные серверу, однако в логах агента не видно информации о подключении
<img src = "img/Снимок экрана 2023-08-11 в 16.46.21.png" width = 100%>

3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
<img src = "img/Снимок экрана 2023-08-11 в 16.46.41.png" width = 100%>

4. Приложите в файл README.md текст использованных команд в GitHub
   Опять не понял какие команды нужны
```bash
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb
sudo apt update
sudo apt install zabbix-agent
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent
sudo vim /etc/zabbix/zabbix_agentd.conf
sudo systemctl restart zabbix-agent
sudo cat /var/log/zabbix/zabbix_agentd.log 
```   

