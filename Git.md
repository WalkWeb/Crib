
# Полезные команды по Git

Не отслеживать изменение права файлов в git:

`git config core.filemode false`

Применить изменения конкретной версии из стеша:

```
$ git stash list
stash@{0}: ...
stash@{1}: ...
$ git stash apply stash@{0}
```

Если какие-то файлы были добавлено в git, а затем добавлены в .gitignore, то git продолжит показывать изменения в них.
Чтобы полностью убрать их из видимости гита:

Для файлов:

`git rm --cached file_name` 

Для директорий:

`git rm --cached -r dir_name` 
