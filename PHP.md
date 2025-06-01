
## phpstan

Добавить в проект

`composer require --dev phpstan/phpstan`

Запомнить все существующие ошибки, и не ругаться на них:

`php vendor/bin/phpstan analyse src --generate-baseline`

Чтобы phpstan учитывал файл `phpstan-baseline.neon` необходимо добавить ссылку на него в `phpstan.dist.neon`:

```
includes:
	- phpstan-baseline.neon
```
