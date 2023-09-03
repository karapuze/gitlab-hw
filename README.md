# Домашнее задание к занятию "Disaster recovery и Keepalived" - `Натетков Александр`


### Задание 1

На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

1. Cкриншот вывода show standby на маршрутизаторах
<img src = "img/Снимок экрана 2023-09-03 в 14.57.38.png" width = 100%>


2. [PKT файл](https://github.com/karapuze/gitlab-hw/blob/main/forfiles/hsrp_done.pkt)


---

### Задание 2

Установите Zabbix Agent на два хоста.

1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
<img src = "img/Снимок экрана 2023-08-11 в 16.14.30.png" width = 100%>

2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером

   
В данном задании столкнулся с проблемой, как видно агент подключен и отдает данные серверу, однако в логах агента не видно информации о подключении
<img src = "img/Снимок экрана 2023-08-11 в 16.46.21.png" width = 100%>

4. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
<img src = "img/Снимок экрана 2023-08-11 в 16.46.41.png" width = 100%>

5. Приложите в файл README.md текст использованных команд в GitHub
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

