# Домашнее задание к занятию "Резервное копирование" - `Натетков Александр`


### Задание 1

	Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию /tmp/backup
	Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
	Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
	На проверку направить скриншот с командой и результатом ее выполнения

1. Cкриншот с командой и результатом выполнения
<img src = "img/Снимок экрана 2023-09-19 в 08.30.42.png" width = 100%>


---

### Задание 2

	Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
	Резервная копия должна быть полностью зеркальной
	Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
	Резервная копия размещается локально, в директории /tmp/backup
	На проверку направить файл crontab и скриншот с результатом работы утилиты.

1. Файл crontab
```
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command

0 3 * * * /home/parallels/rsync.sh

```

2. Скриншот с результатом работы утилиты и записью логов

<img src = "img/Снимок экрана 2023-09-19 в 08.55.59.png" width = 100%>

3. Скрипт
``` bash
#!/bin/bash

bkp_date=$(date +%Y-%m-%d@%H:%M:%S)

rsync -acv --delete /home/parallels/ /tmp/backup/
if [ $? -eq 0 ]
then 
	echo "$bkp_date Backup successful" >> /var/log/rsync.log
	exit 0
else
	echo "$bkp_date Backup failed" >> /var/log/rsync.log
	exit 1
fi 

```   
```

