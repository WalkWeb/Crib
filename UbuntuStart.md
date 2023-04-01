# Настройка рабочей среды на Ubuntu 16.04

## [1] Ubuntu 20+ Темная тема

Настройки -> Внешний вид -> Темная тема


## [2] Добавления русского языка в раскладку

Для Ubuntu 16:

Параметры системы -> Ввод текста -> Добавляем русский язык

Для Ubuntu 18+:

`sudo apt install gnome-tweaks`

Лаунчер -> поиск приложения "Доп. настройки"

Клавиатура и мышь -> Дополнительные параметры раскладки -> Переключение на другую раскладку "Alt+Shift"


## [3] Установка Google Chrome

`wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`

`sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'`

`sudo apt-get update`

`sudo apt-get install google-chrome-stable`


## [4] Nginx

`sudo apt-get update`

`sudo apt-get install nginx`

Проверяем успешность установки: открываем страницу http://localhost/ в браузере - должны увидеть приветствие "Welcome to 
nginx!"

Для простоты локальной разработки даем права на изменение папки конфигов (НИКОГДА не делайте так на боевом сервере):

`sudo chmod -R 777 /etc/nginx/sites-available`

`sudo chmod -R 777 /etc/nginx/sites-enabled`


## [5] MySQL

`sudo apt-get install mysql-server`

Во избежание возможного геморроя НИКОГДА не делаем пустой пароль

Устанавливаем mysqli драйвер:

`sudo apt-get install php7.0-mysqli`


## [6] Создаем первый локальный домен test.loc

Для простоты локальной разработки даем права 777 на /var/www (НИКОГДА не делайте так на боевом сервере):

`sudo chmod -R 777 /var/www`

Создаем папку `test.loc` в `/var/www`

Добавляем запись "127.0.0.1 test.loc" (без кавычек) в файл hosts, открыть его:

`sudo gedit /etc/hosts`

Делаем ссылку: sudo ln -s /etc/nginx/sites-available/test.loc /etc/nginx/sites-enabled/

Перезапускаем nginx: sudo service nginx restart

На всякий случай можно проверить конфигурацию: sudo nginx -t

Создаем в /var/www/test.loc файл index.html с любым текстом и проверяем домен в браузере http://test.loc/ - должно 
работать


## [7] Установка PHP

На примере версии 7.4, если у вас, например, 8.0 - подставьте нужные цифры.

`sudo apt-get install php-fpm php-mysql`

Включаем отображение всех ошибок:

`sudo gedit /etc/php/7.4/fpm/pool.d/www.conf`

Добавляем строку: php_flag[display_errors] = on

Перезапускаем php-fpm:

`sudo service php7.4-fpm stop`

`sudo service php7.4-fpm start`


## [8] Добавляем версии PHP

`sudo add-apt-repository ppa:ondrej/php`

`sudo apt-get update`

`sudo apt-get install php7.2-fpm`


## [9] phpMyAdmin

`sudo apt-get install phpmyadmin`

Для входа в phpmyadmin пишем в адресе: http://localhost/phpmyadmin/

Для входа должен быть корректно настроен конфиг default в nginx (смотрите примеры конфигов в директории nginx)


## [10] Git

`sudo apt-get install git`

`git config --global user.email "Ваша_почта"`

`git config --global user.name "Ваше_имя"`


## [11] Установка phpStorm

Сейчас проще всего установить phpStorm через JetBrains Toolbox.

Скачиваем архив со страницы `https://www.jetbrains.com/ru-ru/lp/toolbox/`

Разорхивируем, запускаем полученный файл. Через 5-10 секунд JetBrains Toolbox запустится. Через него уже устанавливаем
phpStorm.


## Настраиваем phpStorm

1. Подключаем плагины `Symfony Support` и `Php Inspections`:

Settings > Plugins (набираем в поиске названия плагинов, устанавливаем, перезапускаем phpStorm)

2. Улучшаем шаблоны класса/интерфейса/трейта - дефолтные некорректны: отсутствует пустая строка в конце (а она должна быть
по PSR), а также нет `declare(strict_types=1)` что является стандартом для хорошего кода.

Settings > Editor > File and Code Templates

PHP Class
```php
<?php

declare(strict_types=1);

#if (${NAMESPACE})
namespace ${NAMESPACE};
#end

class ${NAME} {

}

```

PHP Interface
```php
<?php

#if (${NAMESPACE})
namespace ${NAMESPACE};
#end

interface ${NAME} {

}

```

PHP Trait
```php
<?php
#parse("PHP File Header.php")

declare(strict_types=1);

#if (${NAMESPACE})
namespace ${NAMESPACE};
#end

trait ${NAME} {

}

```

3. Включаем отображение измененных файлов в панели Git (внизу):

Settings > Version Control > Commit > снимаем галочку с пункта `Use non-modal commit interface`

4. Отключаем браузеры в правом верхнем углу: 

Settings > Tools > Web Browser > Снимаем все галочки

5. Отключаем подстветку html кода: 

Settings > Editor > Color Scheme > General > Code > Injected language fragment > снимаем галочку с цвета фона #364135

6. Подключаем SQL диалект:

File > Settings > Languages & Frameworks > SQL Dialects

Подключаем MySQL диалект

7. Включаем выравнивание элементов в массивах:

File > Settings > Editor > Code Style > PHP > Wrapping and Braces > Array initializer > Align key-value pairs

8. Еще может понадобиться такая настройка:

https://confluence.jetbrains.com/display/IDEADEV/Inotify+Watches+Limit


## [12] Composer

`sudo apt install curl`

`curl -sS https://getcomposer.org/installer -o composer-setup.php`

`sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer`

Для проверки работы выполняем: composer - должно отобразиться информационное приветствие


## [13] Node

`sudo apt-get install nodejs`

Имейте в виду, что по умолчанию Ubuntu, даже 20.04 ставит довольно старую версию ноды. 

Чтобы установить нужную версию, и в целом управлять версиями используйте [nvm](https://github.com/nvm-sh/nvm)


## [14] npm

`sudo apt-get install npm`


## [15] GIMP (обычно он установлен)

`sudo apt-get install gimp`

`gimp`

После установи запускаем Gimp, в меню "Окна" -> "Однооконный режим"


## [16] GitKraken

`sudo apt install gdebi`

https://www.gitkraken.com/download

Качаем .deb-пакет

`sudo gdebi gitkraken-amd64.deb`

Для подтверждения установки необходимо нажать именно русскую "д".


## [17] Skype

Заходим в менеджер приложений -> в поиске Skype -> устанавливаем

Если не получается/не находит через менеджер приложений устанавливаем через snap:

`sudo snap install skype --classic`


## [18] Pidgin

`sudo apt-get install pidgin`


## [19] MongoDB

`sudo apt-get install mongodb`

`sudo apt-get update`

`sudo service mongodb start`

Проверить работу:

Входим через консоль: 

`mongo`

Показать текущие базы: 

`show dbs;`

Создаем и переключаемся в новую бд: 

`use mydb;`

Показать текущую дб: 

`db_name;`

Добавляем запись: 

`db.mycol.insert({"name" : "WalkWeb"});`

Показать коллекции: 

`show collections;`

Показать содержимое текущей бд: 

`db.mycol.find();`

Подключаем драйвер для PHP:

`sudo apt-get install php7.0-dev`

`sudo pecl install mongodb`

`sudo gedit /etc/php/7.0/fpm/php.ini`

Добавляем строку: extension=mongodb.so

Перезапускаем php-fpm:

`sudo service php7.0-fpm restart`

Проверяем: выполняем phpinfo() - должен появиться блок mongodb


## [20] xDebug

Ставим xDebug:

`sudo apt-get install php-xdebug`

`sudo phpenmod xdebug`

`sudo service php7.0-fpm restart`

`sudo gedit /etc/php/7.0/mods-available/xdebug.ini`


Конфигурация для xdebug.ini:

```
zend_extension=xdebug.so
xdebug.remote_autostart=on
xdebug.remote_enable=on
xdebug.remote_port=9002
xdebug.idekey="PHPSTORM"
xdebug.extended_info=1
```

Добавляем интерпретатор php в phpStorm:

Настройки -> Языки и фрейморвки -> PHP -> Добавляем новый: PHP и путь: /usr/bin/php

Добавляем сервер для дебагинга

После настройки необходимо ОБЯЗАТЕЛЬНО перезапустить phpStorm (сразу работать не будет)


## [21] Docker

`sudo apt-get install  curl apt-transport-https ca-certificates software-properties-common`

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

`sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

`sudo apt install docker-ce`

Проверка установки:

`sudo docker run hello-world`


## [22] Docker Compose Install

`sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`sudo chmod +x /usr/local/bin/docker-compose`

Проверка установки:

`docker-compose --version`


## [23] Telegram

`sudo add-apt-repository ppa:atareao/telegram`

`sudo apt update`

`sudo apt install telegram`


## [24] FileZilla

`sudo apt-get install filezilla`


## [25] RabbitMQ

`echo "deb http://www.rabbitmq.com/debian/ testing main"  | sudo tee  /etc/apt/sources.list.d/rabbitmq.list > /dev/null`

`wget https://www.rabbitmq.com/rabbitmq-signing-key-public.asc`

`sudo apt-key add rabbitmq-signing-key-public.asc`

`sudo apt-get update`

`sudo apt-get install rabbitmq-server -y`

`sudo service rabbitmq-server start`

`sudo rabbitmq-plugins enable rabbitmq_management`

`sudo service rabbitmq-server restart`


## [26] TeamViewer

Скачиваем deb-пакет:

`https://www.teamviewer.com/ru/%D1%81%D0%BA%D0%B0%D1%87%D0%B0%D1%82%D1%8C/linux/`

`cd ~/Загрузки/`

`sudo dpkg -i teamviewer_14.7.1965_amd64.deb`

Если появилась ошибка "При обработке следующих пакетов произошли ошибки: teamviewer" Выполняем:

`sudo apt-get -f install`


# [27] Protobuf

`PB_REL="https://github.com/protocolbuffers/protobuf/releases"`

`curl -LO $PB_REL/download/v3.19.3/protoc-3.19.3-linux-x86_64.zip`

`unzip protoc-3.19.3-linux-x86_64.zip -d $HOME/.local`

`export PATH="$PATH:$HOME/.local/bin"`

`protoc --version` 

Должны увидеть: libprotoc 3.19.3


# [28] PostMan

sudo snap install postman


# [29] Генерация SSH-ключей

`ssh-keygen -t rsa`

Нажимаем несколько раз Enter, подтвержая все параметры по умолчанию.

Распечатать полученный ключ:

`cat ~/.ssh/id_rsa.pub`

