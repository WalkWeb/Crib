
# MySQL

## Работа через консоль

Вход:

`mysql -h 127.0.0.1 -u root -p`

где:
* `127.0.0.1` хост базы данных, если подключаемся к локальной базе (127.0.0.1), этот параметр можно опустить
* `root` имя пользователя

Выбрать базу:

`use DBName`

Импорт:

`source путь_к_файлу/имя_файла.sql`

Добавить юзера через консоль:

`CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';`

Дать права на базы:

`GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';`

Где первая `*` - это база, вторая `*` - тип прав (`*` - значит все права)

Выход из консоли mysql: 

`exit`

## Запросы

Посмотреть все FOREIGN KEY таблицы:

`SHOW CREATE TABLE inventory;`

Изменение имя существующей таблицы:

`RENAME TABLE старое_имя_таблицы TO новое_имя_таблицы;`

Изменение существующей колонки:

`ALTER TABLE table CHANGE COLUMN old_column_name new_column_name INT NOT NULL DEFAULT 0;`

Альтернативный вариант:

`ALTER TABLE table MODIFY COLUMN column_name VARCHAR(50) NOT NULL;`

Колонка даты с автозаполнением:

с автоматическим обновлением:

`column_name TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`

без обновления:

`column_name TIMESTAMP DEFAULT CURRENT_TIMESTAMP`

Изменить колонку без блокировки таблицы (есть нюансы - читайте документацию):

`ALTER TABLE table CHANGE old_column new_column varchar(50), ALGORITHM=INPLACE, LOCK=NONE;`

## Логирование

Открываем конфиг MySQL:

`sudo gedit /etc/mysql/mysql.conf.d/mysqld.cnf`

Чтобы включить логирование всех запросов, раскомментируйте строки:

````
general_log_file        = /var/log/mysql/mysql.log
general_log             = 1
````

и перезапустите сервис:

`sudo service mysql restart`

Посмотреть логи запросов:

`sudo gedit /var/log/mysql/mysql.log`


## Пробросить порты

`ssh -fNL LOCAL_PORT:localhost:3306 REMOTE_USER@REMOTE_HOST`


## Удаление MySQL

`sudo yum list installed | grep mysql`

`sudo yum remove mysql-client mysql-server mysql-common mysql-devel`

Еще раз проверяем, не осталось ли чего-либо:

`sudo yum list installed | grep mysql`

Теперь, нам осталось лишь удалить саму папку MySQL

`rm –rf /var/lib/mysql`

`rm –rf /etc/my.conf`



