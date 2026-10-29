# Мидтерм: Инфраструктурный репозиторий

Репозиторий: https://github.com/ggwpadis/infra-project

## Блок 1. Основы

### Задание 1.1. Настройка и первый коммит

Команды:

```bash
ssh -T git@github.com
git clone git@github.com:ggwpadis/infra-project.git
printf '.env\n*.log\n*.retry\n' > .gitignore
git add .gitignore
git commit -m "chore: add .gitignore"
git push -u origin main
```

Вывод:

```
Hi ggwpadis! You've successfully authenticated, but GitHub does not provide shell access.
warning: You appear to have cloned an empty repository.
[main (root-commit) 72fa740] chore: add .gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
 * [new branch]      main -> main
```

Результат: репозиторий создан и склонирован по SSH. Первый коммит `72fa740` содержит только файл `.gitignore` со строками `.env`, `*.log`, `*.retry`.

### Задание 1.2. Файлы проекта

Команды:

```bash
echo "DB_PASSWORD=secret123" > .env
echo "test log" > app.log
git status
git add README.md .env.example
git commit -m "docs: add README and .env.example"
git add docker/
git commit -m "feat(docker): add Dockerfile and index page"
git add ansible/
git commit -m "feat(ansible): add playbook and inventory"
git push
git log --oneline
git check-ignore .env app.log
```

Вывод:

```
Untracked files:
	.env.example
	README.md
	REPORT.md
	ansible/
	docker/

[main ae41ef8] docs: add README and .env.example
[main 5c1ec65] feat(docker): add Dockerfile and index page
[main d8ae235] feat(ansible): add playbook and inventory
   72fa740..d8ae235  main -> main

d8ae235 (HEAD -> main, origin/main) feat(ansible): add playbook and inventory
5c1ec65 feat(docker): add Dockerfile and index page
ae41ef8 docs: add README and .env.example
72fa740 chore: add .gitignore

.env
app.log
```

Результат: после `.gitignore` в истории три отдельных коммита (README и `.env.example`, `docker/`, `ansible/`). `git status` не показывает `.env` и `app.log`, а `git check-ignore` подтверждает, что они игнорируются, поэтому на сервер они не попали.

### Задание 1.3. Просмотр изменений

Команды:

```bash
echo "Учебный инфраструктурный репозиторий: Docker и Ansible." >> README.md
git diff
git add README.md
git diff
git diff --staged
git commit -m "docs: add project description to README"
```

Вывод `git diff` до `git add`:

```
diff --git a/README.md b/README.md
index 27ca4e3..037219e 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,4 @@
 Учебный инфраструктурный репозиторий курса 
 
 <!-- Допишите сюда -->
+Учебный инфраструктурный репозиторий: Docker и Ansible.
```

Вывод `git diff` после `git add`: пусто.

Вывод `git diff --staged` после `git add`:

```
diff --git a/README.md b/README.md
index 27ca4e3..037219e 100644
--- a/README.md
+++ b/README.md
@@ -3,3 +3,4 @@
 Учебный инфраструктурный репозиторий курса 
 
 <!-- Допишите сюда -->
+Учебный инфраструктурный репозиторий: Docker и Ansible.
```

Результат: `git diff` без параметров сравнивает рабочую директорию с индексом. После `git add` изменение уже находится в индексе, поэтому между рабочей директорией и индексом разницы нет, и второй `git diff` пустой. Изменение при этом видно в `git diff --staged`, который сравнивает индекс с последним коммитом.

## Блок 2. История и ветки

### Задание 2.1. История коммитов

Команды:

```bash
git log --oneline
git log --oneline --graph --all
git show HEAD~1
```

Вывод:

```
0a731c0 (HEAD -> main, origin/main) docs: add project description to README
d8ae235 feat(ansible): add playbook and inventory
5c1ec65 feat(docker): add Dockerfile and index page
ae41ef8 docs: add README and .env.example
72fa740 chore: add .gitignore

* 0a731c0 (HEAD -> main, origin/main) docs: add project description to README
* d8ae235 feat(ansible): add playbook and inventory
* 5c1ec65 feat(docker): add Dockerfile and index page
* ae41ef8 docs: add README and .env.example
* 72fa740 chore: add .gitignore

commit d8ae235a4248535169144b16b3260f6ddd300a76
Author: Adis Zarylbekov <zarylbekov_a@iuca.kg>

    feat(ansible): add playbook and inventory
```

Результат: `HEAD~1` — это коммит `d8ae235` с сообщением `feat(ansible): add playbook and inventory`, то есть родитель текущего коммита `HEAD` (`0a731c0`).

### Задание 2.2. Ветка и слияние

Команды:

```bash
git switch -c feature/healthcheck
echo 'HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1' >> docker/Dockerfile
git add docker/Dockerfile
git commit -m "feat(docker): add healthcheck"
git switch main
git merge feature/healthcheck
git branch -d feature/healthcheck
```

Вывод:

```
[feature/healthcheck 29a766c] feat(docker): add healthcheck
Updating 0a731c0..29a766c
Fast-forward
 docker/Dockerfile | 1 +
 1 file changed, 1 insertion(+)
Deleted branch feature/healthcheck (was 29a766c).
```

Результат: слияние прошло методом Fast-forward, потому что в `main` не было новых коммитов, расходящихся с веткой. Строка `HEALTHCHECK` есть в `docker/Dockerfile` в `main`, ветка удалена.

### Задание 2.3. Конфликт слияния

Команды:

```bash
git switch -c fix/app-port
sed -i 's/^app_port:.*/app_port: 9090/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 9090"
git switch main
sed -i 's/^app_port:.*/app_port: 8000/' ansible/group_vars/all.yml
git commit -am "fix(ansible): set app_port to 8000"
git merge fix/app-port
cat ansible/group_vars/all.yml
```

Вывод слияния и файл с маркерами конфликта:

```
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```

Разрешение конфликта (оставлен порт 8000, маркеры удалены):

```bash
cat > ansible/group_vars/all.yml <<'EOF'
---
app_port: 8000
app_user: deploy
EOF
git add ansible/group_vars/all.yml
git commit -m "merge: resolve app port conflict"
git log --oneline --graph --all
git push
```

```
[main 6c3462a] merge: resolve app port conflict

*   6c3462a (HEAD -> main) merge: resolve app port conflict
|\  
| * 973e12e (fix/app-port) fix(ansible): set app_port to 9090
* | caef050 fix(ansible): set app_port to 8000
|/  
* 29a766c feat(docker): add healthcheck
```

Результат: Git не смог слить изменения автоматически, потому что одна и та же строка была изменена в двух ветках по-разному (8000 и 9090). Я вручную оставил значение 8000 и удалил маркеры. В итоговом файле маркеров нет, в истории есть merge-коммит `6c3462a`.

## Блок 3. Удалённый репозиторий и Merge Request

### Задание 3.1. Получение изменений с сервера

README.md отредактирован в веб-интерфейсе GitHub (кнопка Edit), изменения сохранены коммитами в `main`.

Команды:

```bash
git pull
tail -5 README.md
git log --oneline
```

Вывод:

```
From github.com:ggwpadis/infra-project
   8b6b75d..99de5f8  main       -> origin/main
Updating 8b6b75d..99de5f8
Fast-forward
 REPORT.md | 1 +

From github.com:ggwpadis/infra-project
   99de5f8..f27970c  main       -> origin/main
Fast-forward
 README.md | 1 +
 REPORT.md | 2 +-

README updated through GitHub web interface.

f27970c Update README to reflect GitHub web interface changes
89ae3c2 Remove outdated README update note
99de5f8 Update README via GitHub web interface
```

Результат: коммиты, сделанные через веб-интерфейс, появились локально после `git pull`, строка из веб-интерфейса видна в `README.md`.

### Задание 3.2. Merge Request (Pull Request)

Команды:

```bash
git switch -c feature/ansible-curl
nano ansible/playbook.yml
git add ansible/playbook.yml
git commit -m "feat(ansible): install curl"
git push -u origin feature/ansible-curl
```

Вывод:

```
[feature/ansible-curl b9ab9f4] feat(ansible): install curl
 1 file changed, 5 insertions(+)
remote: Create a pull request for 'feature/ansible-curl' on GitHub by visiting:
remote:      https://github.com/ggwpadis/infra-project/pull/new/feature/ansible-curl
 * [new branch]      feature/ansible-curl -> feature/ansible-curl
```

Добавленная задача в `ansible/playbook.yml`:

```yaml
    - name: Install curl
      ansible.builtin.package:
        name: curl
        state: present
```

Описание Pull Request #1:

> Добавлена задача установки curl в ansible/playbook.yml (модуль ansible.builtin.package).
>
> Зачем: curl нужен на управляемых серверах, чтобы проверять доступность веб-сервиса и выполнять HTTP-запросы прямо на хосте.

После слияния PR через веб-интерфейс:

```bash
git switch main
git pull
```

```
   f27970c..bcc2a7e  main       -> origin/main
Fast-forward
 ansible/playbook.yml | 5 +++++
```

Результат: Pull Request #1 из `feature/ansible-curl` слит в `main`, коммит оформлен по Conventional Commits, задача установки curl есть в `playbook.yml` в `main`.

### Задание 3.3. Уборка веток

Команды:

```bash
git branch -d feature/ansible-curl
git fetch --prune
git branch -a
git push origin --delete feature/ansible-curl
git fetch --prune
git branch -a
```

Вывод:

```
Deleted branch feature/ansible-curl (was b9ab9f4).

(git branch -a — до удаления на сервере)
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/feature/ansible-curl
  remotes/origin/main

 - [deleted]         feature/ansible-curl

(git branch -a — после удаления и git fetch --prune)
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

Результат: GitHub не удалил ветку сам, поэтому я удалил её командой `git push origin --delete`. После `git fetch --prune` ветки `feature/ansible-curl` нет ни локально, ни на сервере.

## Блок 4. Docker

### Задание 4.1. Собрать и запустить образ

Команды:

```bash
git switch main
git pull
git switch -c feature/docker-page
nano docker/index.html
nano docker/Dockerfile
docker build -t adis-infra-page ./docker
docker run -d --name adis-infra-page -p 8080:80 adis-infra-page
curl http://localhost:8080
```

Изменение в `docker/Dockerfile` (`git diff`):

```
 FROM nginx:1.27-alpine
+
 COPY index.html /usr/share/nginx/html/
+
+EXPOSE 80
+
 HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
```

Последние строки `docker build`:

```
Step 3/4 : EXPOSE 80
 ---> 1df678c69c39
Step 4/4 : HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
 ---> bf80d1313023
Successfully built bf80d1313023
Successfully tagged adis-infra-page:latest
```

Вывод `curl http://localhost:8080`:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Adis — Infrastructure Project</title>
</head>
<body>
    <h1>Infrastructure Project</h1>
    <h2>Adis Zarylbekov</h2>
    <p>My personal Docker web page.</p>
    <p>Server is running successfully!</p>
</body>
</html>
```

Статус контейнера:

```
3dddcfb96da6   adis-infra-page   "/docker-entrypoint.…"   Up 45 seconds (healthy)   0.0.0.0:8080->80/tcp
```

Остановка и удаление контейнера:

```bash
docker rm -f adis-infra-page
```

```
adis-infra-page
```

Коммиты и Merge Request:

```bash
git add docker/Dockerfile docker/index.html
git commit -m "feat(docker): personalize web page"
git add README.md
git commit -m "docs: add Docker page instructions"
git push -u origin feature/docker-page
git switch main
git pull
```

```
[feature/docker-page 5c8aa90] feat(docker): personalize web page
[feature/docker-page 49ffa3c] docs: add Docker page instructions
   bcc2a7e..d8cbdd4  main       -> origin/main
Fast-forward
 README.md         | 15 +++++++++++++++
 docker/Dockerfile |  4 ++++
 docker/index.html | 13 +++++++++++--
```

Результат: образ собран, контейнер запущен, `curl` вернул мою страницу, контейнер получил статус healthy. В `docker/Dockerfile` есть `EXPOSE 80` после `COPY`, в `README.md` добавлен раздел «Запуск в Docker» с командами сборки и запуска. Pull Request #2 из `feature/docker-page` слит в `main`, коммиты оформлены по Conventional Commits.

## Блок 5. Исправление ошибок и контрольные вопросы

### Задание 5.1. Отмена изменений и правка коммита

`git restore` на `docker/Dockerfile`:

```bash
echo "# temporary test line" >> docker/Dockerfile
git diff -- docker/Dockerfile
git restore docker/Dockerfile
git diff -- docker/Dockerfile
```

```
@@ -5,3 +5,4 @@ COPY index.html /usr/share/nginx/html/
 EXPOSE 80
 
 HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
+# temporary test line
```

Повторный `git diff` ничего не вывел: файл вернулся к состоянию последнего коммита.

`git commit --amend` (выполнен до `git push`):

```bash
echo "Amend demo" > amend-demo.txt
git add amend-demo.txt
git commit -m "test: add amend demo file"
git commit --amend -m "test: demonstrate commit amend"
git log -3 --oneline
```

```
[main afc5681] test: add amend demo file
[main fac43e3] test: demonstrate commit amend
fac43e3 (HEAD -> main) test: demonstrate commit amend
e61a41d test: add restore demo file
d8cbdd4 (origin/main, origin/HEAD) Merge pull request #2 from ggwpadis/feature/docker-page
```

Результат: коммит `afc5681` заменён коммитом `fac43e3` с исправленным сообщением. Старый коммит в историю на сервере не попал.

### Задание 5.2. Отмена опубликованного коммита

Команды:

```bash
sed -i '/^\*\.log$/d' .gitignore
git add .gitignore
git commit -m "chore: remove log files from gitignore"
git push origin main
git revert 78b0cb3
git push origin main
cat .gitignore
git log --oneline -5
```

Вывод:

```
[main 78b0cb3] chore: remove log files from gitignore
   0ebc527..78b0cb3  main -> main
[main 8013f4a] Revert "chore: remove log files from gitignore"
   78b0cb3..8013f4a  main -> main
.env
*.log
*.retry
8013f4a (HEAD -> main, origin/main, origin/HEAD) Revert "chore: remove log files from gitignore"
78b0cb3 chore: remove log files from gitignore
0ebc527 docs: correct Git control questions
```

Результат: строка `*.log` снова есть в `.gitignore`, в истории остались оба коммита. Для уже отправленного на сервер коммита используют `git revert`, потому что он отменяет изменения новым коммитом и не переписывает общую историю. Если удалить коммит из истории, у остальных участников история разойдётся с сервером, и потребуется force-push.

### Задание 5.3. Контрольные вопросы

1. **Что такое коммит и зачем нужна область индекса (staging area)?**

   Коммит — это сохранённая версия изменений в истории Git. Staging area позволяет выбрать, какие именно изменения попадут в следующий коммит.

2. **Что такое ветка в Git?**

   Ветка — это отдельная линия разработки. Она позволяет работать над изменениями отдельно от основной ветки main.

3. **Что такое HEAD?**

   HEAD указывает на текущий коммит, с которым я работаю. Обычно он указывает на последний коммит текущей ветки.

4. **Чем git fetch отличается от git pull?**

   git fetch загружает информацию об изменениях с удалённого репозитория, но не объединяет её с текущей веткой. git pull загружает изменения и объединяет их с текущей веткой.

5. **Почему файл .env нельзя добавлять в репозиторий?**

   В файле .env могут храниться пароли, токены и другие секреты. Если отправить такой файл в удалённый репозиторий, секреты могут стать доступны другим людям.

