# Мидтерм: Инфраструктурный репозиторий

Репозиторий: https://github.com/ggwpadis/infra-project

## Задание 1.1. Настройка и первый коммит

Команды:
- `git clone git@github.com:ggwpadis/infra-project.git`
- `git add .gitignore`
- `git commit -m "chore: add .gitignore"`
- `git push -u origin main`

Результат:
Репозиторий был склонирован по SSH. Первый коммит содержит только
файл .gitignore со строками .env, *.log и *.retry.

## Задание 1.2. Файлы проекта

Команды:
- `git add README.md .env.example`
- `git commit -m "docs: add README and .env.example"`
- `git add docker/`
- `git commit -m "feat(docker): add Dockerfile and index page"`
- `git add ansible/`
- `git commit -m "feat(ansible): add playbook and inventory"`
- `git push`

Результат:
Файлы проекта были добавлены тремя отдельными коммитами.
Файлы .env и app.log игнорируются Git.

## Задание 1.3. Просмотр изменений

Команды:
- `git diff`
- `git add README.md`
- `git diff`
- `git diff --staged`
- `git commit -m "docs: add project description to README"`

Результат:
До добавления в индекс git diff показывал незастейдженные изменения.
После git add команда git diff не показывала эти изменения,
а git diff --staged показывала подготовленное изменение.

## Задание 2.1. История коммитов

Команды:
- `git log --oneline`
- `git log --oneline --graph --all`
- `git show HEAD~1`

Результат:
Команды использовались для просмотра истории коммитов,
графа веток и изменений конкретного коммита.

## Задание 2.2. Ветка и Fast-forward

Команды:
- `git switch -c feature/healthcheck`
- `git add docker/Dockerfile`
- `git commit -m "feat(docker): add healthcheck"`
- `git switch main`
- `git merge feature/healthcheck`
- `git branch -d feature/healthcheck`

Результат:
В Dockerfile добавлена инструкция HEALTHCHECK.
Слияние прошло методом Fast-forward, поскольку main не содержала
новых коммитов, расходящихся с веткой feature/healthcheck.
После слияния ветка была удалена.

## Задание 2.3. Разрешение конфликта слияния

Команды:
- `git switch -c fix/app-port`
- `git commit -am "fix(ansible): set app_port to 9090"`
- `git switch main`
- `git commit -am "fix(ansible): set app_port to 8000"`
- `git merge fix/app-port`
- `git add ansible/group_vars/all.yml`
- `git commit -m "merge: resolve app port conflict"`
- `git push`

Результат:
Git обнаружил конфликт, поскольку в двух ветках значение app_port
было изменено на 8000 и 9090. Конфликт разрешён вручную.
В итоговом файле оставлено значение app_port: 8000.
Слияние завершено коммитом разрешения конфликта.

## Задание 3.1. Изменение README через GitHub

Команды:
- `git switch main`
- `git pull`

Результат:
README.md был отредактирован через веб-интерфейс GitHub.
Затем git pull загрузил удалённые изменения в локальный репозиторий.

## Задание 3.2. Установка curl через Ansible и Pull Request

Команды:
- `git switch -c feature/ansible-curl`
- `nano ansible/playbook.yml`
- `git add ansible/playbook.yml`
- `git commit -m "feat(ansible): install curl"`
- `git push -u origin feature/ansible-curl`

Результат:
В playbook.yml добавлена задача установки curl через
ansible.builtin.package. Pull Request #1 успешно объединён в main.
После слияния локальная и удалённая feature-ветки удалены.

## Задание 4. Docker-страница

Команды:
- `git switch -c feature/docker-page`
- `nano docker/index.html`
- `nano docker/Dockerfile`
- `git diff`
- `docker build -t adis-infra-page ./docker`
- `docker run -d --name adis-infra-page -p 8080:80 adis-infra-page`
- `curl http://localhost:8080`
- `git add docker/Dockerfile docker/index.html`
- `git commit -m "feat(docker): personalize web page"`
- `git add README.md`
- `git commit -m "docs: add Docker page instructions"`
- `git push -u origin feature/docker-page`

Результат:
Создана персонализированная HTML-страница.
В Dockerfile добавлены EXPOSE 80 и HEALTHCHECK.
Docker-образ успешно собран, контейнер запущен на порту 8080.
Команда curl вернула HTML-страницу с именем Adis Zarylbekov.
Контейнер получил статус healthy.
Инструкции по запуску добавлены в README.md.
Pull Request #2 успешно объединён в main, ветка удалена.

## Задание 5.1. git restore

Команды:
- `echo "Original text" > restore-demo.txt`
- `git add restore-demo.txt`
- `git commit -m "test: add restore demo file"`
- `echo "Modified text" > restore-demo.txt`
- `git diff`
- `git restore restore-demo.txt`
- `cat restore-demo.txt`

Результат:
После изменения файла git restore вернул содержимое
к версии последнего коммита: Original text.

## Задание 5.2. git commit --amend

Команды:
- `echo "Amend demo" > amend-demo.txt`
- `git add amend-demo.txt`
- `git commit -m "test: add amend demo file"`
- `git commit --amend -m "test: demonstrate commit amend"`
- `git log -3 --oneline`

Результат:
Последний коммит был заменён новым коммитом с исправленным сообщением.
Команда amend использовалась до отправки этих коммитов на GitHub.

## Задание 5.3. git revert

Команды:
- `echo "This change will be reverted" > revert-demo.txt`
- `git add revert-demo.txt`
- `git commit -m "test: add revert demo file"`
- `git revert --no-edit 4d09a8c`
- `git log -4 --oneline`
- `ls revert-demo.txt`

Результат:
Команда git revert создала коммит abc3ab2, отменивший изменения
коммита 4d09a8c. Файл revert-demo.txt был удалён.
При этом исходный коммит остался в истории.

## Контрольные вопросы

1. Для чего нужен файл .gitignore?
   Он указывает Git, какие неотслеживаемые файлы не нужно добавлять
   в репозиторий, например .env и файлы логов.

2. Чем отличаются git diff и git diff --staged?
   git diff показывает незастейдженные изменения.
   git diff --staged показывает изменения, добавленные в индекс
   и подготовленные к коммиту.

3. Что такое ветка и Pull Request?
   Ветка позволяет разрабатывать изменения отдельно от основной ветки.
   Pull Request позволяет проверить изменения и объединить их в main.

4. Чем отличаются git restore и git revert?
   git restore отменяет незакоммиченные изменения файла.
   git revert создаёт новый коммит, отменяющий изменения предыдущего коммита.

5. Для чего используется git commit --amend?
   Команда позволяет исправить последний коммит, например изменить
   его сообщение или включить дополнительные изменения.
