<h1>Полезные LINUX команды</h1>

<p>Мониторинг всех http запросов (результат по умолчанию сохраняется в отдельных файлах в вашей домашней дирректории):<br />
sudo tcpflow port 80</p>

<p>Посчитать количество строк в .php файлах в указанной дирректории:<br />
find /var/www/site.loc/ -name "*.php" -type f -exec wc -l {} \; | awk 'BEGIN{sum=0}{sum+=$1;}END{print sum;}'</p>

<p>Запись видео с экрана:<br />
ffmpeg -y -f alsa -i pulse -f x11grab -s 1920x1080 -r 25 -i :0.0 -vcodec mpeg4 -qscale 0 -f avi -acodec pcm_s16le /var/www/video.avi</p>