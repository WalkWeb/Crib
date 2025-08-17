# MongoDB

Статус:

`/etc/init.d/mongodb status`

Вход через консоль (на старых версиях достаточно `mongo`): 

`mongosh -u user -p password`

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

Показать содержимое текущей коллекции: 

`db.mycol.find();`

Удалить все записи в коллекции:

`db.mycol.remove({})`

Создание бекапа:

sudo mongodump --db database --out /var/backups/\`date +"%m-%d-%y"\`

где database - имя базы, которую бекапим

Импорт базы из бекапа:

`mongorestore -d database /var/backups/mondodump`
