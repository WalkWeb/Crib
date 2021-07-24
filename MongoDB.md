# MongoDB

Статус:

`/etc/init.d/mongodb status`

Вход через консоль: 

`mongo`

Показать текущие базы: 

`show dbs;`

Создать и переключиться в новую бд: 

`use mydb;`

Показать текущую дб: 

`db;`

Добавить запись: 

`db.mycol.insert({"name" : "WalkWeb"});`

Показать коллекции: 

`show collections;`

Показать содержимое текущей бд: 

`db.mycol.find();`

Создание бекапа:

sudo mongodump --db database --out /var/backups/\`date +"%m-%d-%y"\`

где database - имя базы, которую бекапим

Импорт базы из бекапа:

`mongorestore -d database /var/backups/mondodump`

