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

## Обновление коллекций на php (создание миграций для MongoDB)

Добавляем библиотеку `mongodb/mongodb`:

`composer require mongodb/mongodb`

Код:

```php
$user = "admin";
$pwd = 'password';
$host = '127.0.0.1';
$port = '27017';

$client = new MongoDB\Client("mongodb://$user:$pwd@$host:$port");

$collection = $client->getCollection('db_name', 'collection_name');

$filter = [];

// Добавление нового поля
$collection->updateMany($filter, [
    '$set' => ['new_field' => 'value'],
]);

// Удаление поля
$collection->updateMany($filter, [
    '$unset' => ['new_field' => ''],
]);

// Обновление (сразу всей сущности)
// _id старой и новой записи должны совпадать
foreach ($collection->find() as $item) {
    $new = [
        '_id' => $item->_id,
        'updatedField' => $item->oldField . ' updated',
        'createdAt' => $item->createdAt,
    ];

    $collection->replaceOne(['_id' => $item->_id], $new);
}
```

При работе с MongoDB нужно помнить, что коллекция может содержать что угодно. Соответственно обращаясь к любому полю
нужно проверять, что оно есть, и что оно содержит нужный тип данных.
