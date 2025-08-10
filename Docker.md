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

1. Требуется регистрация/авторизация на hub.docker.com

`docker login`

2. Создаем новый репозиторий, запоминаем его название вида `user/name`

### Простой вариант (из Dockerfile)

3. Переходим в директорию с Dockerfile и выполняем (не забыв заменить на свой `user` / `name` / `tag`):

`docker build -t user/name:1.0 .`

4. Загружаем созданный image:

`docker push user/name:1.0`

### Более сложный вариант (из готового image)

Этот вариант более сложен тем, что вам нужно правильно определить нужный image, а их, при большом количестве проектов
может быть много.

Добавляем нужному image тег:

`docker tag image_id user/name:tag`

Например:

`docker tag 90cd74c5b5ab walkweb/php-8.4-mysql`

Пушим:

`docker push walkweb/php-8.4-mysql`

## Заставить контейнер всегда работать

Добавить команду при старте:

`tail -f /dev/null`
