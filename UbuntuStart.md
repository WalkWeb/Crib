<h1>Настройка рабочей среды на ubuntu 16.04</h1>

<h3>[1] Добавления русского языка в ввод текста</h3>

<p>Параметры системы -> Ввод текста -> Добавляем русский язык</p>

<h3>[2] Установка Google Chrome</h3>

<p>wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -</p>
<p>sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'</p>
<p>sudo apt-get update</p>
<p>sudo apt-get install google-chrome-stable</p>

<h3>[3] Nginx</h3>

<p>sudo apt-get update</p>
<p>sudo apt-get install nginx</p>
<br />
<p>Проверяем успешность установки: открываем страницу http://localhost/ в браузере - должны увидеть приветствие "Welcome to nginx!"</p>
<br />
<p>Для простоты локальной разработки даем права на изменение папки конфигов (НИКОГДА не делайте так на боевом сервере):</p>
<p>sudo chmod -R 777 /etc/nginx/sites-available</p>
<p>sudo chmod -R 777 /etc/nginx/sites-enabled</p>

<h3>[4] MySQL</h3>

<p>sudo apt-get install mysql-server</p>
<p>Во избежание возможного геморроя НИКОГДА не делаем пустой пароль</p>

<h3>[5] Создаем первый локальный домен test.loc</h3>

<p>Для простоты локальной разработки даем права 777 на /var/www (НИКОГДА не делайте так на боевом сервере)</p>
<p>sudo chmod -R 777 /var/www</p>
<p>Создаем папку test.loc в /var/www</p>
<p>Добавляем запись "127.0.0.1 test.loc" (без кавычек) в sudo gedit /etc/hosts</p>
<br />
<p>Делаем ссылку: sudo ln -s /etc/nginx/sites-available/test.loc /etc/nginx/sites-enabled/</p>
<br />
<p>Перезапускаем nginx: sudo service nginx restart</p>
<br />
<p>На всякий случай можно проверить конфигурацию: sudo nginx -t</p>
<br />
<p>Создаем в /var/www/test.loc файл index.html с содержимым <h1>Test.loc</h1> и проверяем домен в браузере http://test.loc/ - должно работать</p>

<h3>[6] Установка PHP</h3>

<p>sudo apt-get install php-fpm php-mysql</p>
<br />
<p>Включаем отображение всех ошибок:</p>
<p>sudo gedit /etc/php/7.0/fpm/pool.d/www.conf</p>
<p>добавляем строку: php_flag[display_errors] = on</p>
<p>sudo service php7.0-fpm stop</p>
<p>sudo service php7.0-fpm start</p>

<h3>[7] phpMyAdmin</h3>

<p>sudo apt-get install phpmyadmin</p>
<p>Вход в phpmyadmin: http://localhost/phpmyadmin/</p>
<p>Для входа должен быть настроен конфиг default в nginx (он появляется автоматически при установке nginx)</p>

<h3>[8] Git</h3>

<p>sudo apt-get install git</p>
<p>git config --global user.email "Ваша_почта"</p>
<p>git config --global user.name "Ваше_имя"</p>

<h3>[9] Установка phpStorm</h3>

<p>sudo apt remove openjdk*</p>
<p>sudo add-apt-repository ppa:webupd8team/java</p>
<p>sudo apt-get update</p>
<p>sudo apt-get install java-common oracle-java8-installer (!!! Принимаем лицензионное соглашение в процессе установки !!!)</p>
<p>sudo apt-get install oracle-java8-set-default</p>
<p>source /etc/profile</p>
<p>wget https://download-cf.jetbrains.com/webide/PhpStorm-2017.3.6.tar.gz</p>
<br />
<p>После завершения загрузки откройте терминал и смените рабочую директорию на каталог, в котором лежит загруженный файл.
<br />
<p>tar xvf PhpStorm-2017.3.6.tar.gz</p>
<p>sudo mv PhpStorm-173.4674.46/ /opt/phpstorm/</p>
<p>sudo ln -s /opt/phpstorm/bin/phpstorm.sh /usr/local/bin/phpstorm</p>
<p>phpstorm</p>


<h3>Донастраиваем phpStorm</h3>

<p><b>Изменение стиля на темный</b>: Settings -> Editor -> Color Scheme -> выбираем Dracula</p>
<p><b>Отключаем браузеры в правом верхнем углу</b>: Settings -> Tools -> Web Browser -> Снимаем все галочки</p>
<br />
<p><b>Чиним горячие клавиши в русской расладке:</b></p>
<p>1. создаем папку fix в домашней дирректории юзера</p>
<p>2. git clone https://github.com/zheludkovm/LinuxJavaFixes.git fix</p>
<p>3. sudo gedit /opt/phpstorm/bin/phpstorm64.vmoptions</p>
<p>4. добавляем строку: -javaagent:/home/walk/fix/build/LinuxJavaFixes-1.0.0-SNAPSHOT.jar  (!!! слово walk заменяем на свое имя пользователя !!!)</p>
<p>5. перезапускаем phpstorm</p>
<br />
<p><b>Отключаем подстветку html кода:</b></p>
<p>Settings -> Editor -> Color Scheme -> General -> Code -> Injected language fragment -> снимаем галочку с цвета фона #364135</p>

<p><b>Подключаем SQL диалект:</b></p>
<p>File > Settings > Languages & Frameworks > SQL Dialects</p>
<p>Подключаем MySQL диалект</p>

<p><b>Еще может понадобиться такая настройка:</b></p>
<p>https://confluence.jetbrains.com/display/IDEADEV/Inotify+Watches+Limit</p>


<h3>[10] Composer</h3>

<p>sudo apt install curl</p>
<p>curl -sS https://getcomposer.org/installer -o composer-setup.php</p>
<p>sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer</p>
<p>для проверки работы выполняем: composer - должно отобразиться информационное приветствие</p>


<h3>[11] Node</h3>
<p>sudo apt-get install nodejs</p>



<h3>[12] npm</h3>
<p>sudo apt-get install npm</p>



<h3>[13] GIMP (обычно он установлен)</h3>

<p>sudo apt-get install gimp</p>
<p>gimp</p>
<p>После установи запускаем Gimp, в меню "Окна" -> "Однооконный режим"</p>


<h3>[14] GitKraken</h3>

<p>sudo apt install gdebi</p>
<p>https://www.gitkraken.com/download</p>
<p>качаем .deb-пакет</p>
<p>sudo gdebi gitkraken-amd64.deb</p>
<p>для подтверждения установки необходимо нажать именно русскую "д"</p>

<h3>[15] Skype</h3>

<p>Заходим в менеджер приложений -> в поиске Skype -> устанавливаем</p>

<p>Если не получается через менеджер приложений:</p>
<p>sudo snap install skype --classic</p>

<h3>[16] Pidgin</h3>

<p>sudo apt-get install pidgin</p>

<h3>[17] MongoDB</h3>

<p>sudo apt-get install mongodb</p>
<p>sudo apt-get update</p>
<p>sudo service mongodb start</p>

<p>Проверить работу:</p>
<p>входим через консоль: mongo</p>
<p>показать текущие базы: show dbs;</p>
<p>создаем и переключаемся в новую бд: use mydb;</p>
<p>показать текущую дб: db;</p>
<p>добавляем запись: db.mycol.insert({"name" : "WalkWeb"});</p>
<p>показать коллекции: show collections;</p>
<p>показать содержимое текущей бд: db.mycol.find();</p>

<p>Подключаем драйвер для PHP:</p>
<p>sudo apt-get install php7.0-dev</p>
<p>sudo pecl install mongodb</p>
<p>sudo gedit /etc/php/7.0/fpm/php.ini</p>
<p>добавляем строку: extension=mongodb.so</p>
<p>sudo service php7.0-fpm restart</p>
<p>Проверяем: выполняем phpinfo() - должен появиться блок mongodb</p>

<h3>[18] xDebug</h3>

<p>Ставим xDebug:</p>
<p>sudo apt-get install php-xdebug</p>
<p>sudo phpenmod xdebug</p>
<p>sudo service php7.0-fpm restart</p>
<p>sudo gedit /etc/php/7.0/mods-available/xdebug.ini</p>
<br />
<p>Конфигурация для xdebug.ini:</p>
<p>zend_extension=xdebug.so</p>
<p>xdebug.remote_autostart=on</p>
<p>xdebug.remote_enable=on</p>
<p>xdebug.remote_port=9002</p>
<p>xdebug.idekey="PHPSTORM"</p>
<p>xdebug.extended_info=1</p>
<br />
<p>Добавляем интерпретатор php в phpStorm:</p>
<p>Настройки -> Языки и фрейморвки -> PHP -> Добавляем новый: PHP и путь: /usr/bin/php</p>
<br />
<p>Добавляем сервер для дебажинга</p>
<br />
<p>После настройки необходимо ОБЯЗАТЕЛЬНО перезапустить phpStorm или можно перезагрузить весь комп сразу</p>



<h3>[19] Docker</h3>

<p>coming soon</p>



<h3>[20] Telegram</h3>

<p>sudo add-apt-repository ppa:atareao/telegram</p>
<p>sudo apt update</p>
<p>sudo apt install telegram</p>



<h3>[21] FileZilla</h3>

<p>sudo apt-get install filezilla</p>




<h3>[22] Добавляем версих PHP 7.1</h3>

<p>sudo add-apt-repository ppa:ondrej/php</p>
<p>sudo apt-get update</p>
<p>sudo apt-get install php7.1-fpm</p>
<br />
<p>Включаем отображение всех ошибок:</p>
<p>sudo gedit /etc/php/7.0/fpm/pool.d/www.conf</p>
<p>добавляем строку: php_flag[display_errors] = on</p>
<p>sudo service php7.0-fpm stop</p>
<p>sudo service php7.0-fpm start</p>
<br />
<p>Устанавливаем mysqli драйвер:</p>
<p>sudo apt-get install php7.0-mysqli</p>
