<h1>Настройка рабочей среды на Ubuntu 16.04</h1>

<h3>[1] Добавления русского языка в ввод текста</h3>

<p>Параметры системы -> Ввод текста -> Добавляем русский язык</p>

<h3>[2] Установка Google Chrome</h3>

<p>wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -<br />
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'<br />
sudo apt-get update<br />
sudo apt-get install google-chrome-stable</p>

<h3>[3] Nginx</h3>

<p>sudo apt-get update<br />
sudo apt-get install nginx</p>

<p>Проверяем успешность установки: открываем страницу http://localhost/ в браузере - должны увидеть приветствие "Welcome to nginx!"</p>

<p>Для простоты локальной разработки даем права на изменение папки конфигов (НИКОГДА не делайте так на боевом сервере):<br />
sudo chmod -R 777 /etc/nginx/sites-available<br />
sudo chmod -R 777 /etc/nginx/sites-enabled</p>

<h3>[4] MySQL</h3>

<p>sudo apt-get install mysql-server<br />
Во избежание возможного геморроя НИКОГДА не делаем пустой пароль</p>

<h3>[5] Создаем первый локальный домен test.loc</h3>

<p>Для простоты локальной разработки даем права 777 на /var/www (НИКОГДА не делайте так на боевом сервере)<br />
sudo chmod -R 777 /var/www<br />
Создаем папку test.loc в /var/www</p>

<p>Добавляем запись "127.0.0.1 test.loc" (без кавычек) в sudo gedit /etc/hosts</p>

<p>Делаем ссылку: sudo ln -s /etc/nginx/sites-available/test.loc /etc/nginx/sites-enabled/</p>

<p>Перезапускаем nginx: sudo service nginx restart</p>

<p>На всякий случай можно проверить конфигурацию: sudo nginx -t</p>

<p>Создаем в /var/www/test.loc файл index.html с любым текстом и проверяем домен в браузере http://test.loc/ - должно работать</p>

<h3>[6] Установка PHP</h3>

<p>sudo apt-get install php-fpm php-mysql</p>

<p>Включаем отображение всех ошибок:<br />
sudo gedit /etc/php/7.0/fpm/pool.d/www.conf<br />
добавляем строку: php_flag[display_errors] = on<br />
sudo service php7.0-fpm stop<br />
sudo service php7.0-fpm start</p>

<h3>[7] phpMyAdmin</h3>

<p>sudo apt-get install phpmyadmin<br />
Вход в phpmyadmin: http://localhost/phpmyadmin/<br />
Для входа должен быть настроен конфиг default в nginx (он появляется автоматически при установке nginx)</p>

<h3>[8] Git</h3>

<p>sudo apt-get install git<br />
git config --global user.email "Ваша_почта"<br />
git config --global user.name "Ваше_имя"</p>

<h3>[9] Установка phpStorm</h3>

<p>sudo apt remove openjdk*<br />
sudo add-apt-repository ppa:webupd8team/java<br />
sudo apt-get update<br />
sudo apt-get install java-common oracle-java8-installer (!!! Принимаем лицензионное соглашение в процессе установки !!!)<br />
sudo apt-get install oracle-java8-set-default<br />
source /etc/profile<br />
wget https://download-cf.jetbrains.com/webide/PhpStorm-2017.3.6.tar.gz</p>

<p>После завершения загрузки откройте терминал и смените рабочую директорию на каталог, в котором лежит загруженный файл.</p>

<p>tar xvf PhpStorm-2017.3.6.tar.gz<br />
sudo mv PhpStorm-173.4674.46/ /opt/phpstorm/<br />
sudo ln -s /opt/phpstorm/bin/phpstorm.sh /usr/local/bin/phpstorm<br />
phpstorm</p>


<h3>Донастраиваем phpStorm</h3>

<p><b>Изменение стиля на темный</b>: Settings -> Editor -> Color Scheme -> выбираем Dracula<br />
<b>Отключаем браузеры в правом верхнем углу</b>: Settings -> Tools -> Web Browser -> Снимаем все галочки</p>

<p><b>Чиним горячие клавиши в русской расладке:</b><br />
1. создаем папку fix в домашней дирректории юзера<br />
2. git clone https://github.com/zheludkovm/LinuxJavaFixes.git fix<br />
3. sudo gedit /opt/phpstorm/bin/phpstorm64.vmoptions<br />
4. добавляем строку: -javaagent:/home/walk/fix/build/LinuxJavaFixes-1.0.0-SNAPSHOT.jar  (!!! слово walk заменяем на свое имя пользователя !!!)<br />
5. перезапускаем phpstorm</p>

<p><b>Отключаем подстветку html кода:</b><br />
Settings -> Editor -> Color Scheme -> General -> Code -> Injected language fragment -> снимаем галочку с цвета фона #364135</p>

<p><b>Подключаем SQL диалект:</b><br />
File > Settings > Languages & Frameworks > SQL Dialects<br />
Подключаем MySQL диалект</p>

<p><b>Еще может понадобиться такая настройка:</b><br />
https://confluence.jetbrains.com/display/IDEADEV/Inotify+Watches+Limit</p>


<h3>[10] Composer</h3>

<p>sudo apt install curl<br />
curl -sS https://getcomposer.org/installer -o composer-setup.php<br />
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer<br />
для проверки работы выполняем: composer - должно отобразиться информационное приветствие</p>


<h3>[11] Node</h3>
<p>sudo apt-get install nodejs</p>



<h3>[12] npm</h3>
<p>sudo apt-get install npm</p>



<h3>[13] GIMP (обычно он установлен)</h3>

<p>sudo apt-get install gimp<br />
gimp<br />
После установи запускаем Gimp, в меню "Окна" -> "Однооконный режим"</p>


<h3>[14] GitKraken</h3>

<p>sudo apt install gdebi<br />
https://www.gitkraken.com/download<br />
качаем .deb-пакет<br />
sudo gdebi gitkraken-amd64.deb<br />
для подтверждения установки необходимо нажать именно русскую "д"</p>

<h3>[15] Skype</h3>

<p>Заходим в менеджер приложений -> в поиске Skype -> устанавливаем</p>

<p>Если не получается через менеджер приложений:<br />
sudo snap install skype --classic</p>

<h3>[16] Pidgin</h3>

<p>sudo apt-get install pidgin</p>

<h3>[17] MongoDB</h3>

<p>sudo apt-get install mongodb<br />
sudo apt-get update<br />
sudo service mongodb start</p>

<p>Проверить работу:<br />
входим через консоль: mongo<br />
показать текущие базы: show dbs;<br />
создаем и переключаемся в новую бд: use mydb;<br />
показать текущую дб: db;<br />
добавляем запись: db.mycol.insert({"name" : "WalkWeb"});<br />
показать коллекции: show collections;<br />
показать содержимое текущей бд: db.mycol.find();</p>

<p>Подключаем драйвер для PHP:<br />
sudo apt-get install php7.0-dev<br />
sudo pecl install mongodb<br />
sudo gedit /etc/php/7.0/fpm/php.ini<br />
добавляем строку: extension=mongodb.so<br />
sudo service php7.0-fpm restart<br />
Проверяем: выполняем phpinfo() - должен появиться блок mongodb</p>

<h3>[18] xDebug</h3>

<p>Ставим xDebug:<br />
sudo apt-get install php-xdebug<br />
sudo phpenmod xdebug<br />
sudo service php7.0-fpm restart<br />
sudo gedit /etc/php/7.0/mods-available/xdebug.ini</p>

<p>Конфигурация для xdebug.ini:<br />
zend_extension=xdebug.so<br />
xdebug.remote_autostart=on<br />
xdebug.remote_enable=on<br />
xdebug.remote_port=9002<br />
xdebug.idekey="PHPSTORM"<br />
xdebug.extended_info=1</p>

<p>Добавляем интерпретатор php в phpStorm:<br />
Настройки -> Языки и фрейморвки -> PHP -> Добавляем новый: PHP и путь: /usr/bin/php</p>

<p>Добавляем сервер для дебажинга</p>

<p>После настройки необходимо ОБЯЗАТЕЛЬНО перезапустить phpStorm или можно перезагрузить весь комп сразу</p>



<h3>[19] Docker</h3>

<p>coming soon</p>



<h3>[20] Telegram</h3>

<p>sudo add-apt-repository ppa:atareao/telegram<br />
sudo apt update<br />
sudo apt install telegram</p>



<h3>[21] FileZilla</h3>

<p>sudo apt-get install filezilla</p>




<h3>[22] Добавляем версих PHP 7.1</h3>

<p>sudo add-apt-repository ppa:ondrej/php<br />
sudo apt-get update<br />
sudo apt-get install php7.1-fpm</p>

<p>Включаем отображение всех ошибок:<br />
sudo gedit /etc/php/7.0/fpm/pool.d/www.conf<br />
добавляем строку: php_flag[display_errors] = on<br />
sudo service php7.0-fpm stop<br />
sudo service php7.0-fpm start</p>

<p>Устанавливаем mysqli драйвер:<br />
sudo apt-get install php7.0-mysqli</p>


<h3>[23] RabbitMQ</h3>

<p>echo "deb http://www.rabbitmq.com/debian/ testing main"  | sudo tee  /etc/apt/sources.list.d/rabbitmq.list > /dev/null<br />
wget https://www.rabbitmq.com/rabbitmq-signing-key-public.asc<br />
sudo apt-key add rabbitmq-signing-key-public.asc<br />
sudo apt-get update<br />
sudo apt-get install rabbitmq-server -y<br />
sudo service rabbitmq-server start<br />
sudo rabbitmq-plugins enable rabbitmq_management<br />
sudo service rabbitmq-server restart</p>

<h3>TeamViewer</h3>

<p><a href="https://www.teamviewer.com/ru/%D1%81%D0%BA%D0%B0%D1%87%D0%B0%D1%82%D1%8C/linux/">Скачиваем deb-пакет</a><br />
cd ~/Загрузки/<br />
sudo dpkg -i teamviewer_14.7.1965_amd64.deb<br />
Если появилась ошибка "При обработке следующих пакетов произошли ошибки: teamviewer"<br />
Выполняем:<br />
sudo apt-get -f install</p>
