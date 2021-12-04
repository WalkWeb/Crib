
Выполняем и подтверждаем (`y`):

`composer require "codeception/codeception" --dev`

Создаем `.env.test` и `.env.test.local`, где помимо параметров аналогично `.env` нужны еще параметры для codeception:

```yaml
# Codeception tests
DB_HOST=localhost
DB_PORT=5432
DB_USER=db_user
DB_PASSWORD=password
DB_NAME=db_name
```

В `codeception.yml` указываем ссылку на `.env.test.local`

Заполняем `tests/unit.suite.yml` необходимыми данными по модулям Symfony, Doctrine2, Cli и Db. В финале должно
получиться примерно так:

```yaml
actor: UnitTester
modules:
    enabled:
        - Asserts
        - \App\Tests\Helper\Unit
        -   Symfony:
                app_path: 'src'
                environment: 'test'
        -   Doctrine2:
              depends: Symfony
        -   Cli:
              class: Codeception\Module\Cli
        -   Db:
              dsn: 'pgsql:host=%DB_HOST%;port=%DB_PORT%;dbname=%DB_NAME%'
              user: "%DB_USER%"
              password: '%DB_PASSWORD%'
              populate: true
              cleanup: false
              # Example command for create dump:
              # pg_dump -h localhost -U postgres -F t -f /var/www/symfony-blog.loc/tests/_data/symfony_blog.sql.tar symfony_blog --no-owner
              dump: 'tests/_data/symfony_blog.sql.tar'
              populator: 'pg_restore --no-owner --clean --if-exists --dbname $dbname --username $user --host $host --port=$port $dump'
error_level: "E_ERROR"
```

Создаем первый тест:

`php vendor/bin/codecept generate:test unit Example`

Устанавливаем необходимые пакеты для codeception:
 
```bash
composer require codeception/module-asserts --dev
composer require codeception/module-symfony --dev
composer require codeception/module-doctrine2 --dev
composer require codeception/module-cli --dev
composer require codeception/module-db --dev
composer require --dev doctrine/doctrine-fixtures-bundle
```

Выполняем сгенерированный в предыдущем шаге тест:

`php vendor/bin/codecept run unit`

Если ошибок нет - значит все успешно. Теперь можно писать свои unit и приемочные тесты.
