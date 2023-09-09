# Домашнее задание к занятию "Кластеризация и балансировка нагрузки" - `Натетков Александр`


### Задание 1

На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.

1. Cкриншот где видно обращения на разные сервера
<img src = "img/Снимок экрана 2023-09-09 в 19.34.31.png" width = 100%>

2. конфиг haproxy
```
global
	log /dev/log	local0
	log /dev/log	local1 notice
	chroot /var/lib/haproxy
	stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
	stats timeout 30s
	user haproxy
	group haproxy
	daemon

	# Default SSL material locations
	ca-base /etc/ssl/certs
	crt-base /etc/ssl/private

	# See: https://ssl-config.mozilla.org/#server=haproxy&server-version=2.0.3&config=intermediate
        ssl-default-bind-ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
        ssl-default-bind-ciphersuites TLS_AES_128_GCM_SHA256:TLS_AES_256_GCM_SHA384:TLS_CHACHA20_POLY1305_SHA256
        ssl-default-bind-options ssl-min-ver TLSv1.2 no-tls-tickets

defaults
	log	global
	mode	http
	option	httplog
	option	dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
	errorfile 400 /etc/haproxy/errors/400.http
	errorfile 403 /etc/haproxy/errors/403.http
	errorfile 408 /etc/haproxy/errors/408.http
	errorfile 500 /etc/haproxy/errors/500.http
	errorfile 502 /etc/haproxy/errors/502.http
	errorfile 503 /etc/haproxy/errors/503.http
	errorfile 504 /etc/haproxy/errors/504.http

listen stats  # веб-страница со статистикой
        bind                    :888
        mode                    http
        stats                   enable
        stats uri               /stats
        stats refresh           5s
        stats realm             Haproxy\ Statistics

frontend example  # секция фронтенд
        mode tcp
	bind :8089
        default_backend web_servers

backend web_servers    # секция бэкенд
        mode tcp
        balance roundrobin
        server s1 127.0.0.1:8888 
        server s2 127.0.0.1:9999 

```



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
