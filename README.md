# Домашнее задание к занятию «Репликация и масштабирование. Часть 1» - `Натетков Александр`



### Задание 1. 

На лекции рассматривались режимы репликации master-slave, master-master, опишите их различия.

Ответить в свободной форме.




– Master-Slave Replication: Этот режим используется чаще всего и предполагает наличие одного главного сервера (master) и одного или нескольких подчиненных серверов (slaves).
Данные на главном сервере постоянно обновляются, а подчиненные сервера синхронизируются с главным, копируя все изменения.
Этот режим подходит для ситуаций, когда необходимо обеспечить высокую производительность чтения с главного сервера и высокую производительность записи только на главном сервере.
-Master-Master Replication: В этом режиме оба сервера являются главными и могут обрабатывать запросы на запись. Это полезно в ситуациях, когда важно обеспечить высокую доступность и непрерывность работы, например, в кластерах. Однако, из-за сложностей в координации изменений между двумя серверами, этот режим может привести к потере данных или несогласованности в некоторых ситуациях.


### Задание 2.

Выполните конфигурацию master-slave репликации, примером можно пользоваться из лекции.

Приложите скриншоты конфигурации, выполнения работы: состояния и режимы работы серверов.

1. Конфигурация

![Конфигурация](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-01%20в%2012.57.10.png)

![Конфигурация](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-01%20в%2013.01.43.png)

2. Статус master

   
![master](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-01%20в%2013.00.48.png)

3. Статус slave


![slave](https://github.com/karapuze/gitlab-hw/blob/main/img/Снимок%20экрана%202023-12-01%20в%2013.00.30.png)
