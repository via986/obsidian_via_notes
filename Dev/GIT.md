Локально:
`git init`
`git add .`
`git commit -m "Initial commit"`

связываем с удаленным репозиторием:

`git remote add origin https://github.com/пользователь/имя-репозитория.git`
проверяем что мы в main
`git branch -M main`
пушим локальные файлы
`git push -u origin main`

Команда `git push -u origin main` связывает вашу локальную ветку `main` с веткой на GitHub. В следующий раз для отправки изменений 
вам достаточно будет написать просто `git push`

Проверка
`git remote -v`