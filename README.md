
«Система мониторинга Zabbix» - «Бычков Денис Вячеславович»      

---
### Задание 1
Установите Zabbix Server с веб-интерфейсом.

Процесс выполнения
1 Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2 Установите PostgreSQL. Для установки достаточна та версия, что есть в системном репозитороии Debian 11.
3 Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
4 Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.
Требования к результаты
Прикрепите в файл README.md скриншот авторизации в админке.
Приложите в файл README.md текст использованных команд в GitHub.

<img src = "img/1-1.JPG" width = 50%>

Установка репозитория Zabbix
wget https://repo.zabbix.com/zabbix/6.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.4-1+ubuntu22.04_all.deb
dpkg -i zabbix-release_6.4-1+ubuntu22.04_all.deb

Установка Zabbix сервера, веб-интерфейса и агента
apt install zabbix-server-mysql zabbix-frontend-php zabbix-nginx-conf zabbix-sql-scripts zabbix-agent

Создание базы данных
mysql -uroot -p
password
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'password';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;

zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix

mysql -uroot -p
password
set global log_bin_trust_function_creators = 0;
quit;

Настройка базы данных для Zabbix сервера
DBPassword=password

Настройка PHP для веб-интерфейса
listen 8080;
server_name example.com;

Запуск процессов Zabbix сервера и агента
systemctl restart zabbix-server zabbix-agent nginx php8.1-fpm
systemctl enable zabbix-server zabbix-agent nginx php8.1-fpm


### Задание 2
Установите Zabbix Agent на два хоста.

Процесс выполнения
1 Выполняя ДЗ, сверяйтесь с процессом отражённым в записи лекции.
2 Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
3 Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
4 Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.
Требования к результаты
1 Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу
2 Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером
3 Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.
4 Приложите в файл README.md текст использованных команд в GitHub

<img src = "img/4-1.JPG" width = 50%>

<img src = "img/4-2.JPG" width = 50%>

<img src = "img/4-3.JPG" width = 50%>


### Задание 3 со звёздочкой*
Установите Zabbix Agent на Windows (компьютер) и подключите его к серверу Zabbix.

Требования к результаты
1 Приложите в файл README.md скриншот раздела Latest Data, где видно свободное место на диске C:

<img src = "img/3-1.JPG" width = 50%>