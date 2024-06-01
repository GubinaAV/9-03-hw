# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Gubina Angela Vital'evna`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/9-02-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

##### Процесс выполнения
 1. Выполняя ДЗ сверяйтесь с процессом отражённым в записи лекции.
 2. Установите PostgreSQL. Для установки достаточна та версия что есть в системном репозитороии Debian 11
 3. Пользуясь конфигуратором комманд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache
 4. Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server

##### Требования к результаты
 1. Прикрепите в файл README.md скриншот авторизации в админке
 2. Приложите в файл README.md текст использованных команд в GitHub

### Решение 1
 ##### 1. Скрин авторизации
 ![alt text](https://github.com/GubinaAV/9-02-hw/blob/main/img/zabbix01.png)

 ###### 2. Листинг команд 
  
  apt update
  apt install postgresql
  systemctl start postgresql
  wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
  dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
  sudo -u postgres createuser --pwprompt zabbix
  sudo -u postgres createdb -O zabbix zabbix
  apt update
  apt install zabbix-server-pgsql zabbix-frontend-php php8.1-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent
  zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
  nano  /etc/zabbix/zabbix_server.conf
  systemctl restart zabbix-server zabbix-agent apache2
  systemctl enable zabbix-server zabbix-agent apache2

---

### Задание 2

Установите Zabbix Agent на два хоста.

##### Процесс выполнения
 1. Выполняя ДЗ сверяйтесь с процессом отражённым в записи лекции.
 2. Установите Zabbix Agent на 2 виртмашины, одной из них может быть ваш Zabbix Server
 3. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов
 4. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera
 5. Проверьте что в разделе Latest Data начали появляться данные с добавленных агентов

##### Требования к результаты
 1. Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
 2. Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
 3. Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
 4. Приложите в файл README.md текст использованных команд в GitHub
 
 ### Решение 2
 ##### 1. Скриншот агентов
 ![alt text](https://github.com/GubinaAV/9-02-hw/blob/main/img/zabbix02.png)
 ##### 2. Скриншот лога агента
 ![alt text](https://github.com/GubinaAV/9-02-hw/blob/main/img/zabbix03.png)
 ##### 3.скриншот раздела latest data для мониторируемой машины и сервера, подраздел CPU
 ![alt text](https://github.com/GubinaAV/9-02-hw/blob/main/img/zabbix04.png)
 ##### 4. листинг команд на мониторируемом компьютере
 wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-4+ubuntu22.04_all.deb
 dpkg -i zabbix-release_6.0-4+ubuntu22.04_all.deb
 apt update
 apt install zabbix-agent
 systemctl restart zabbix-agent
 systemctl enable zabbix-agent
 nano /etc/zabbix/zabbix_agentd.conf
 systemctl restart zabbix-agent
 systemctl status zabbix-agent
 tail -f /var/log/zabbix/zabbix_agentd.log

---
### Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

##### Требования к результаты
Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:
### Решение 3