# Полезные команды по Linux

Сведения о системе:

`inxi -v5 -z`

Посмотреть процессы:

`htop`

При прерывании установки пакета:

`sudo dpkg --configure -a`

Мониторинг всех http запросов (результат по умолчанию сохраняется в отдельных файлах в вашей домашней директории):

`sudo tcpflow port 80`

Посчитать количество строк в .php файлах в указанной директории:

`find /var/www/site.loc/ -name "*.php" -type f -exec wc -l {} \; | awk 'BEGIN{sum=0}{sum+=$1;}END{print sum;}'`

Запись видео с экрана:

`ffmpeg -y -f alsa -i pulse -f x11grab -s 1920x1080 -r 25 -i :0.0 -vcodec mpeg4 -qscale 0 -f avi -acodec pcm_s16le /var/www/video.avi`

## Linux screen

Список существующих скринов:

`screen -list`

Создать скрин:

`screen`

Создать скрин с определенным именем:

`screen -S ScreenName`

Зайти в определенный скрин:

`screen -r ScreenName`


Выйти из скрина оставив его работать:

`Ctrl + A после чего нажать  D`

Закрыть скрин:

`exit`

# Прочее

Запуск программы в фоне:

`program_name &`

Убить фоновую программу по id:

`kill %123`
