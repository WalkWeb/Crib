
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
