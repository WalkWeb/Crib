
# Полезные заметки по Symfony

## Добавление codeception-тестов в Symfony проект

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
error_level: "E_NOTICE"
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

## Хранение кэша на Redis

`docker-compose.yml`

```yaml
  redis:
    container_name: redis
    image: redis:7.0
    ports:
      - ${REDIS_PORT}:6379
    volumes:
      - ${SERVER_HOME}/redis/data:/data/redis
    networks:
      - you_network
```

`cache.yaml`

```yaml
framework:
    cache:
        app: cache.adapter.redis
        default_redis_provider: redis://localhost
        prefix_seed: oos/api-notifier
```

Код-пример для контроллера:

```php
    /**
     * @Route("/test/{id}", name="test")
     * @param string $id
     * @param EntityManagerInterface $entityManager
     * @param CacheInterface $cache
     * @return Response
     * @throws InvalidArgumentException
     */
    public function test(string $id, EntityManagerInterface $entityManager, CacheInterface $cache): Response
    {
        /** @var User $stock */
        $stock = $cache->get($id, function (ItemInterface $item) use ($id, $entityManager) {

            echo 'No Cache<br />';

            echo 'Create key: ' . $item->getKey() . '<br />';

            return $entityManager->getRepository(User::class)->findOneBy([
                'id' => $id,
            ]);
        });

        return new Response("{$stock->getId()}, {$stock->getEmail()}");
    }
```

Запускаем сервер:

`symfony server:start`

Открываем страницу вида `localdomain.loc/test/1`

```
No Cache
Create key: 270378f0-b29c-4bf6-b7d1-7a64d6e212ff
270378f0-b29c-4bf6-b7d1-7a64d6e212ff, admin@mail.com
```

При обновлении страницы:

```
270378f0-b29c-4bf6-b7d1-7a64d6e212ff, admin@notificator.com
```

Проверить, что кэш сохранился именно в Redis:

1. Заходим в контейнер

`sudo docker exec -it redis redis-cli`

2. Смотрим сохраненные ключи:

```
127.0.0.1:6379> keys *
1) "gTPBdlcdoq:270378f0-b29c-4bf6-b7d1-7a64d6e212ff"
127.0.0.1:6379> get gTPBdlcdoq:270378f0-b29c-4bf6-b7d1-7a64d6e212ff
"O:15:\"App\\Entity\\User\":6:{...}"
```

## Исправляем ошибку с flex.symfony.com

В 2022 году появилась ошибка, когда в проекте на симфони перестает работать composer - ни добавить ни удалить какой-либо 
пакет, ругается на доступ к flex.symfony.com. Исправляется следующей командой:

`composer update symfony/flex --no-plugins --no-scripts`

## Доработка EntityManager в Doctrine

Родной EntityManager в Doctrine не держит стабильное соединение с базой - в случае падения базы он не пытается к ней
переподключиться (хотя падение могло быть на пару секунд), если была SQL-ошибка, то он просто закрывает EntityManager.
Если в обычных http-запросах это допустимо, то когда на php пишут воркеры, которые постоянно обрабатывают данные в 
каком-нибудь брокере сообщений - уже нет.

Соответственно нужно дорабатывать, добавляя свою прослойку над EntityManager. 

Обертка над EntityManager:

```php
class EntityManagerDecorator implements EntityManagerInterface
{
    private ObjectManager $entityManager;
    private ManagerRegistry $doctrine;

    public function __construct(ObjectManager $entityManager, ManagerRegistry $doctrine)
    {
        $this->entityManager = $entityManager;
        $this->doctrine = $doctrine;
    }

    /**
     * @param $object
     * @return void
     * @throws Exception
     */
    public function persist($object): void
    {
        $this->correctionConnect();
        $this->entityManager->persist($object);
    }

    /**
     * @return void
     * @throws Exception
     */
    public function flush(): void
    {
        $this->correctionConnect();
        $this->entityManager->flush();
    }


    private function correctionConnect(): void
    {
        // For errors:
        // SQLSTATE[57P01]: Admin shutdown: 7 FATAL:  terminating connection due to administrator command
        // SQLSTATE[08006] [7] could not translate host name "postgres" to address: Temporary failure in name resolution
        if ($this->entityManager->getConnection()->ping() === false) {
            $this->entityManager->getConnection()->close();
            $this->entityManager->getConnection()->connect();
        }

        // For error "The EntityManager is closed"
        if (!$this->entityManager->isOpen()) {
            $this->doctrine->resetManager();
            $this->entityManager = $this->doctrine->getManager();
        }
    }
    
    // Все другие методы для EntityManagerInterface перебрасываются напрямую на базовый EntityManager
}
```

Настройка `config/services.yaml`, чтобы декоратор использовался по умолчанию при зависимости на EntityManagerInterface:

```yaml
App\Proxy\EntityManagerDecorator:
    decorates: 'doctrine.orm.entity_manager'
```

Также необходимо установить пакет `symfony/proxy-manager-bridge`:

`composer require symfony/proxy-manager-bridge`
