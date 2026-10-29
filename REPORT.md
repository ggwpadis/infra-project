# Мидтерм: Инфраструктурный репозиторий

## Задание 1.1. Настройка и первый коммит

Команды:
- `git clone git@github.com:ggwpadis/infra-project.git`
- `git add .gitignore`
- `git commit -m "chore: add .gitignore"`
- `git push -u origin main`

Результат:
Репозиторий был склонирован по SSH. Первый коммит содержит только файл .gitignore со строками .env, *.log и *.retry.

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
Файлы проекта были добавлены тремя отдельными коммитами и отправлены на GitHub. Файлы .env и app.log игнорируются Git.

## Задание 1.3. Просмотр изменений

Команды:
- `git diff`
- `git add README.md`
- `git diff`
- `git diff --staged`
- `git commit -m "docs: add project description to README"`

Результат:
До добавления в индекс git diff показывал изменение README.md. После git add команда git diff стала пустой, а git diff --staged показывала подготовленное изменение.

## Задание 2.1. История коммитов

Команды:
- `git log --oneline`
- `git log --oneline --graph --all`
- `git show HEAD~1`

Результат:
Команды показали историю коммитов. HEAD~1 указывал на предыдущий коммит d8ae235, который добавил файлы Ansible.

## Задание 2.2. Ветка и Fast-forward

Команды:
- `git switch -c feature/healthcheck`
- `git add docker/Dockerfile`
- `git commit -m "feat(docker): add healthcheck"`
- `git switch main`
- `git merge feature/healthcheck`
- `git branch -d feature/healthcheck`

Результат:
В Dockerfile добавлена инструкция HEALTHCHECK. Слияние прошло методом Fast-forward, потому что main не содержала новых коммитов относительно feature/healthcheck. После слияния ветка была удалена.

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
Git обнаружил конфликт, поскольку в двух ветках значение app_port было изменено на 8000 и 9090. Конфликт разрешён вручную: в итоговом файле оставлен порт 8000. Слияние завершено коммитом 6c3462a и отправлено на GitHub.

## Блоки 3–5

Эти задания ещё предстоит выполнить. После выполнения добавлю сюда реальные команды, выводы и объяснения.

## Контрольные вопросы

Ответы будут добавлены после выполнения оставшихся заданий.
README updated through GitHub web interface.
