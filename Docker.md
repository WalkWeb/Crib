# Полезные команды по Docker

## Посмотреть, сколько места на диске занимают images и контейнеры:

`sudo docker system df`

## Детализация по размеру /var/lib/docker/overlay2/

`sudo du -h -d 1 /var/lib/docker/overlay2/`
