
# PostgreSQL

## Установка и настройка

Установка Postgres:

`sudo apt-get install postgresql postgresql-contrib`

Установка драйвера к php:

`sudo apt install php7.3-pgsql`

Избавляемся от проблем с правами доступа при локальной разработке - узнаем, где лежат настройки:

`ps aux | grep postgres | grep -- -D`

Смотрим путь к postgresql.conf, в той же директории лежит нужный нам pg_hba.conf - открываем его и комментируем все строки с доступом и добавляем:

`host    all             all             localhost               trust`

Перезагружаем:

`systemctl restart postgresql-9.5`


## Работа через консоль

Вход:

`psql -U user -h 127.0.0.1 -p 5432 -W password`

Создать бд:

`CREATE DATABASE "symfony_blog";`

Создать пользователя:

`CREATE USER "symfony_blog_user" password '12345';`

Дать все привилегии на базу:

`GRANT ALL PRIVILEGES ON DATABASE "symfony_blog" TO "symfony_blog_user";`

Прочее:

```
\q - выход
\l - показать базы
\c dbname - выбрать базу
\dt — список всех таблиц
\d users - показать информацию по таблице users
```

## Смена пароля у пользователя

Частая проблема, когда теряется пароль для главного пользователя `postgres`. Вначале переключаемся на супер 
пользователя:

`sudo -i`

Входим в консоль Postgres (под `root` пользователем пароль спрашиваться не будет):

`psql -U postgres -h localhost`

Меняем пароль:

`ALTER USER user_name WITH PASSWORD 'new_password';`


## Исправление схемы public по умолчанию (если она сбилась)

Заходим в базу:

`psql -U postgres -h localhost`

Выбираем нужную базу:

`\c db_name`

Выполняем:

`grant usage on schema public to public;`

`grant create on schema public to public;`

## Ошибка с подключением на CentOS:
   
```
service httpd stop
service postgresql stop
setsebool -P httpd_can_network_connect 1
service httpd start
service postgresql start
```

## Месторасположение

CentOS:

`/var/lib/pgsql/11/data/`

Ubuntu:

`/etc/postgresql/11/main/`

## Работа с координатами:
   
Устанавливаем PostGIS

`sudo apt install postgis`

Заходим в консоль под суперпользователем и выполняем:

`CREATE EXTENSION postgis;`

Там же в консоли выбираем нужную бд

`\c dbname`

И еще раз выполняем:

`CREATE EXTENSION postgis;`

После чего мы можем создать таблицу с типом координат

`CREATE TABLE cities (id int4 primary key, name varchar(50), the_geom geometry(POINT,4326));`

И заполнить её данными:

```
INSERT INTO cities (id, the_geom, name) VALUES (1,ST_GeomFromText('POINT(-0.1257 51.508)',4326),'London, England');
INSERT INTO cities (id, the_geom, name) VALUES (2,ST_GeomFromText('POINT(-81.233 42.983)',4326),'London, Ontario');
INSERT INTO cities (id, the_geom, name) VALUES (3,ST_GeomFromText('POINT(27.91162491 -33.01529)',4326),'East London,SA');
```

## Происходит ли сейчас чистка мусора (активен ли vacuum)

`SELECT check_vacuum('my_table');`

```
CREATE FUNCTION check_vacuum(name text) RETURNS boolean
    LANGUAGE sql SECURITY DEFINER
AS $_$
SELECT count(*)::int > 0
FROM pg_stat_progress_vacuum
WHERE relid::regclass = $1::regclass OR
        relid::regclass::text in (
        SELECT reltoastrelid::regclass::text FROM pg_class WHERE relname = $1
    );
$_$;
```

## Просмотр активных соединений

На микросервисной архитектуре с одним инстансом Postgres бывает что какой-то сервис забивает лимит подключений, и 
необходимо выяснить виновного. Для этого заходим в консоль и выполняем:

`SELECT * from pg_stat_activity;`

## Логирование всех запросов

Открываем файл конфига (указав свою версию):

`sudo gedit /etc/postgresql/9.5/main/postgresql.conf`

В конец файла добавляем (в конец, чтобы потом, когда нужно будет отключить или еще раз включить логирование - не 
приходилось править строки по всему файлу):

`log_statement = 'all'`

`log_directory = 'pg_log'`

`log_filename = 'postgresql-%Y-%m-%d_%H%M%S.log'`

`logging_collector = on`

Затем перезагружаем postgres:

`sudo service postgresql restart`

