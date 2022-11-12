
# Полезные команды по языку Go

## Установка

`sudo rm -rf /usr/lib/go-1.6/ /usr/lib/go-1.6/src/ /usr/lib/go-1.6/src/runtime/ /usr/lib/go-1.6/src/runtime/race`

`curl -O https://storage.googleapis.com/golang/go1.19.3.linux-amd64.tar.gz`

`sudo tar -C /usr/local -xzf go1.19.3.linux-amd64.tar.gz`

`mkdir -p ~/go; echo "export GOPATH=$HOME/go" >> ~/.bashrc`

`echo "export PATH=$PATH:$HOME/go/bin:/usr/local/go/bin" >> ~/.bashrc`
`source ~/.bashrc`

Чтобы убедиться, что никаких проблем с путями нет, установите какой-нибудь пакет:

`go get github.com/gorilla/websocket`

И если никаких ошибок не произошло (в случае успеха ничего не выводится) - значит все нормально.

## Удаление

`sudo apt-get purge golang*`

`sudo rm -rf /usr/local/go`

## Обновление версии

Функционала обновления версии в Go нет. Для установки более нужной версии нужно вначале удалить существующую:

`sudo apt-get purge golang*`

А затем скачать и установить нужную (с.м. выше).

## Проблемы с GOPATH/PATH/context

Если установить Go через стандартный установщик `apt install golang` то потом вы скорее всего столкнетесь с ошибками
GOPATH/PATH/context. Проще всего исправить их следующим образом: вначале удалите существующую версию go, а затем 
установите по инструкции выше.
