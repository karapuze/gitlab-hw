# Домашнее задание к занятию «Уязвимости и атаки на информационные системы» - `Натетков Александр`



### Задание 1. 

Ответьте на следующие вопросы:

1. Какие сетевые службы в ней разрешены?
2. Какие уязвимости были вами обнаружены? (список со ссылками: достаточно трёх уязвимостей)

Приведите ответ в свободной форме.


1.
![Network_Service1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-15%20в%2015.41.54.png)

![Network_Service1](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-15%20в%2015.42.10.png)


2.1 https://www.exploit-db.com/exploits/49757

2.2 https://www.exploit-db.com/exploits/19203

2.3 https://www.exploit-db.com/exploits/13853


### Задание 2.

Проведите сканирование Metasploitable в режимах SYN, FIN, Xmas, UDP.

Запишите сеансы сканирования в Wireshark.
Ответьте на следующие вопросы:

1. Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?
2. Как отвечает сервер?

Приведите ответ в свободной форме.

SYN - TCP соединение не устанавливается до конца, отправляется SYN пакет если в ответ приходит SYN/ACK, то порт считается открытым, если RST, то закрытым.

FIN - Похоже на SYN, но с установленным TCP FIN битом, задумка в том чтобы обойти устаревшие МЭ (пакетные фильтры и non-stateful), но например с Cisco или Windows бесполезно, т.к. ответ будет везде RST, соответсвтенно все порты будут считаться закрытыми.

Xmas - По сути тоже самое что и FIN, просто устанавливается больше флагов ( FIN, PSH, URG )

UDP - Отправляется пустой UDP пакет, если в ответ приходит ICMP ошибка определенного типа, то порт считается закрытым, если ошибки других типов, то порт считается октрытым.


