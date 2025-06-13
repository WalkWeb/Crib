# Базовые команды по Docker

## Запустить контейнеры в фоне

`docker compose up -d`

## Пересобрать контейнеры, запустить, удалить неиспользуемые:

`docker compose up -d --build --remove-orphans`

## Остановить контейнеры

`docker compose down`

## Зайти в контейнер

`docker exec -it container_name bash`

У некоторых контейнеров не установлен bash, тогда:

`docker exec -it container_name sh`

# Удаление

## Остановить все контейнеры

`docker stop $(docker ps -a -q)`

## Удалить все контейнеры

`docker rm -vf $(docker ps -aq)`

## Удалить все images

`docker rmi -f $(docker images -aq)`

## Удалить все сети

`docker network prune`

# Статистика

## Посмотреть, сколько места на диске занимают images и контейнеры:

`docker system df`

## Детализация по размеру /var/lib/docker/overlay2/

`sudo du -h -d 1 /var/lib/docker/overlay2/`

## Посмотреть информацию по потребляемым ресурсам (процессору, оперативной памяти) контейнерами

`docker stats`

# hub.docker.com и публикация своих image

## Авторизация 

Требуется регистрация/авторизация на hub.docker.com

`docker login`

Создаем новый репозиторий, запоминаем его название вида `user/name`

Добавляем своему image тег:

`docker tag image_id user/name:tag`

Например:

`docker tag 90cd74c5b5ab walkweb/php-8.4-mysql`

Пушим:

`docker push walkweb/php-8.4-mysql`

## Заставить контейнер всегда работать

Добавить команду при старте:

`tail -f /dev/null`
