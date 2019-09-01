<p>Установка:<br />
sudo apt-get install postgresql postgresql-contrib</p>


<p>Вход через консоль:<br />
sudo -u postgres psql</p>


<p>Создание базы:<br />
CREATE DATABASE symfony_test_db;</p>


<p>Создание юзера:<br />
CREATE USER symfony_test_user WITH password '12345';</p>


<p>Дать все права:<br />
GRANT ALL privileges ON database symfony_test_db TO symfony_test_user;</p>


<p>Показать базы:<br />
\l</p>


<p>Выход:<br />
\q</p>