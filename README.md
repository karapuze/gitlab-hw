# Домашнее задание к занятию "Disaster recovery и Keepalived" - `Натетков Александр`


### Задание 1

На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

1. Cкриншот вывода show standby на маршрутизаторах
<img src = "img/Снимок экрана 2023-09-03 в 14.57.38.png" width = 100%>


2. [PKT файл](https://github.com/karapuze/gitlab-hw/blob/main/forfiles/hsrp_done.pkt)


---

### Задание 2

На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html

1. bash скрипт
```bash
#!/bin/bash
if [ -e /var/www/html/index.nginx-debian.html ] && nc -w 1 10.211.55.8 80
then
echo "done"
exit 0
else
echo "nedone"	
exit 1
fi
```

2. конфигурационный файл keepalived
```
vrrp_script check_port {
	script "/home/parallels/script"
	interval 3
}
vrrp_instance VI_1 {
        state MASTER
        interface enp0s5
        virtual_router_id 15
        priority 255
        advert_int 1

        virtual_ipaddress {
              10.211.55.100/24
        }
	track_script {
		check_port
	}
}
```
3. Скриншот переезда
<img src = "img/Снимок экрана 2023-09-03 в 19.55.09.png" width = 100%>
