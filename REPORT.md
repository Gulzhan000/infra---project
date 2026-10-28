# Отчёт по мидтерму DevOps

Проект: midterm devops

Репозиторий: https://github.com/Gulzhan000/infra---project

Для переноса проекта из infra-project заново выполнила шаги задания в новом пустом репозитории. Команды Git и Docker выполнялись в Linux-контейнере Alpine. В отчёте оставлены команды, основные результаты и короткие объяснения.

## 1.1 Настройка и первый коммит

Настроила имя, почту и ветку main. Склонировала пустой репозиторий по SSH.

```text
$ git clone git@github.com:Gulzhan000/infra---project.git infra---project
Cloning into '/work/infra---project'...
warning: You appear to have cloned an empty repository.
```

```text
$ uname -s
Linux
```

```text
$ git --version
git version 2.54.0
```

```text
$ git config --global user.name Gulzhan000

```

```text
$ git config --global user.email ashuubayeva_g@iuca.kg

```

```text
$ git config --global init.defaultBranch main

```

```text
$ git add .gitignore

```

```text
$ git commit -m "added gitignore"
[main (root-commit) 531616d] added gitignore
 1 file changed, 3 insertions(+)
 create mode 100644 .gitignore
```

В первом коммите только .gitignore. В него внесены .env, *.log и *.retry.

## 1.2 Файлы проекта

```text
$ git status --short
?? .env.example
?? README.md
?? REPORT.md
?? ansible/
?? docker/
```

```text
$ git check-ignore .env app.log
.env
app.log
```

```text
$ git add README.md .env.example

```

```text
$ git commit -m "added project description"
[main c0c6708] added project description
 2 files changed, 7 insertions(+)
 create mode 100755 .env.example
 create mode 100755 README.md
```

```text
$ git add docker/

```

```text
$ git commit -m "added docker files"
[main ad7c68e] added docker files
 2 files changed, 7 insertions(+)
 create mode 100755 docker/Dockerfile
 create mode 100755 docker/index.html
```

```text
$ git add ansible/

```

```text
$ git commit -m "added ansible files"
[main 4950d43] added ansible files
 4 files changed, 29 insertions(+)
 create mode 100755 ansible/ansible.cfg
 create mode 100755 ansible/group_vars/all.yml
 create mode 100755 ansible/inventory.ini
 create mode 100755 ansible/playbook.yml
```

Файлы добавлены тремя отдельными коммитами. .env и app.log игнорируются и не попадают на GitHub.

```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra---project.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

## 1.3 Просмотр изменений

```text
$ git diff
diff --git a/README.md b/README.md
index 27ca4e3..27eb212 100755
--- a/README.md
+++ b/README.md
@@ -3,3 +3,5 @@
 Учебный инфраструктурный репозиторий курса 
 
 <!-- Допишите сюда -->
+
+Проект midterm devops: учебная практика по Git, Docker и Ansible.
```

```text
$ git add README.md

```

```text
$ git diff

```

```text
$ git diff --staged
diff --git a/README.md b/README.md
index 27ca4e3..27eb212 100755
--- a/README.md
+++ b/README.md
@@ -3,3 +3,5 @@
 Учебный инфраструктурный репозиторий курса 
 
 <!-- Допишите сюда -->
+
+Проект midterm devops: учебная практика по Git, Docker и Ansible.
```

```text
$ git commit -m "updated project description"
[main 89db7a1] updated project description
 1 file changed, 2 insertions(+)
```

До git add было видно добавленное описание. После git add обычный git diff пустой: рабочий файл совпадает с индексом. Подготовленное изменение видно через git diff --staged.

## 2.1 История коммитов

```text
$ git log --oneline
89db7a1 updated project description
4950d43 added ansible files
ad7c68e added docker files
c0c6708 added project description
531616d added gitignore
```

```text
$ git log --oneline --graph --all
* 89db7a1 updated project description
* 4950d43 added ansible files
* ad7c68e added docker files
* c0c6708 added project description
* 531616d added gitignore
```

```text
$ git show HEAD~1
commit 4950d4338ba7bf2429b3a8a0d32c2aae4ed94e07
Author: Gulzhan000 <ashuubayeva_g@iuca.kg>
Date:   Wed Oct 28 16:53:02 2026 +0000

    added ansible files

diff --git a/ansible/ansible.cfg b/ansible/ansible.cfg
new file mode 100755
index 0000000..4dd424d
--- /dev/null
+++ b/ansible/ansible.cfg
@@ -0,0 +1,3 @@
+[defaults]
+inventory = inventory.ini
+host_key_checking = False
diff --git a/ansible/group_vars/all.yml b/ansible/group_vars/all.yml
new file mode 100755
index 0000000..d780f15
--- /dev/null
+++ b/ansible/group_vars/all.yml
@@ -0,0 +1,3 @@
+---
+app_port: 8080
+app_user: deploy
diff --git a/ansible/inventory.ini b/ansible/inventory.ini
new file mode 100755
index 0000000..c78d1be
--- /dev/null
+++ b/ansible/inventory.ini
@@ -0,0 +1,2 @@
+[web]
+web1 ansible_host=192.168.56.10 ansible_user=vagrant
diff --git a/ansible/playbook.yml b/ansible/playbook.yml
new file mode 100755
index 0000000..9814f7b
--- /dev/null
+++ b/ansible/playbook.yml
@@ -0,0 +1,21 @@
+---
+- name: Configure web servers
+  hosts: web
+  become: true
+
+  tasks:
+    - name: Install nginx
+      ansible.builtin.package:
+        name: nginx
+        state: present
+
+    - name: Create application user
+      ansible.builtin.user:
+        name: "{{ app_user }}"
+        shell: /bin/bash
+
+    - name: Ensure nginx is started and enabled
+      ansible.builtin.service:
+        name: nginx
+        state: started
+        enabled: true
```

На момент проверки HEAD~1 — 4950d43 added ansible files. Это родитель текущего коммита.

## 2.2 Ветка и слияние

```text
$ git switch -c feature/healthcheck
Switched to a new branch 'feature/healthcheck'
```

```text
$ git add docker/Dockerfile

```

```text
$ git commit -m "added health check"
[feature/healthcheck 00c0a95] added health check
 1 file changed, 1 insertion(+)
```

```text
$ git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)
```

```text
$ git merge feature/healthcheck
Updating 89db7a1..00c0a95
Fast-forward
 docker/Dockerfile | 1 +
 1 file changed, 1 insertion(+)
```

```text
$ git branch -d feature/healthcheck
Deleted branch feature/healthcheck (was 00c0a95).
```

HEALTHCHECK теперь в main. Слияние прошло Fast-forward, временная ветка удалена.

## 2.3 Конфликт слияния

```text
$ git switch -c fix/app-port
Switched to a new branch 'fix/app-port'
```

```text
$ git add ansible/group_vars/all.yml

```

```text
$ git commit -m "changed app port to 9090"
[fix/app-port 4aa7227] changed app port to 9090
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
$ git switch main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 2 commits.
  (use "git push" to publish your local commits)
```

```text
$ git add ansible/group_vars/all.yml

```

```text
$ git commit -m "changed app port to 8000"
[main 25c6139] changed app port to 8000
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```text
$ git merge fix/app-port
Auto-merging ansible/group_vars/all.yml
CONFLICT (content): Merge conflict in ansible/group_vars/all.yml
Automatic merge failed; fix conflicts and then commit the result.
```

```text
$ cat ansible/group_vars/all.yml
---
<<<<<<< HEAD
app_port: 8000
=======
app_port: 9090
>>>>>>> fix/app-port
app_user: deploy
```

```text
$ git add ansible/group_vars/all.yml

```

```text
$ git commit -m "resolved app port conflict"
[main f5de312] resolved app port conflict
```

```text
$ git log --oneline --graph --all
*   f5de312 resolved app port conflict
|\  
| * 4aa7227 changed app port to 9090
* | 25c6139 changed app port to 8000
|/  
* 00c0a95 added health check
* 89db7a1 updated project description
* 4950d43 added ansible files
* ad7c68e added docker files
* c0c6708 added project description
* 531616d added gitignore
```

Конфликт возник из-за изменения одной строки в двух ветках. Оставила порт 8000 и убрала маркеры. В истории появился merge-коммит.

```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra---project.git
   4950d43..f5de312  main -> main
branch 'main' set up to track 'origin/main'.
```

## 3.1 Получение изменений с сервера

В редакторе GitHub добавила в README строку об авторе и сохранила коммит updated readme on github. Затем получила изменение локально.

```text
$ git pull
From github.com:Gulzhan000/infra---project
   f5de312..5f4f56f  main       -> origin/main
Updating f5de312..5f4f56f
Fast-forward
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

```text
$ git log --oneline
5f4f56f updated readme on github
f5de312 resolved app port conflict
25c6139 changed app port to 8000
4aa7227 changed app port to 9090
00c0a95 added health check
89db7a1 updated project description
4950d43 added ansible files
ad7c68e added docker files
c0c6708 added project description
531616d added gitignore
```

Коммит из веб-интерфейса появился в локальной истории, новая строка — в README.

## 3.2 Pull Request

```text
$ git switch -c feature/ansible-curl
Switched to a new branch 'feature/ansible-curl'
```

```text
$ git add ansible/playbook.yml

```

```text
$ git commit -m "feat(ansible): added curl installation"
[feature/ansible-curl 413e1d0] feat(ansible): added curl installation
 1 file changed, 26 insertions(+), 21 deletions(-)
```

Добавила задачу установки curl через ansible.builtin.package со state: present. Она нужна для проверки ответа веб-сервера из терминала.

```text
$ git push -u origin feature/ansible-curl
remote: 
remote: Create a pull request for 'feature/ansible-curl' on GitHub by visiting:        
remote:      https://github.com/Gulzhan000/infra---project/pull/new/feature/ansible-curl        
remote: 
To https://github.com/Gulzhan000/infra---project.git
 * [new branch]      feature/ansible-curl -> feature/ansible-curl
branch 'feature/ansible-curl' set up to track 'origin/feature/ansible-curl'.
```

Перед слиянием сохранила в playbook.yml прежний формат переносов строк, чтобы в изменении осталась только задача curl. Исправлен только коммит рабочей ветки; main не переписывалась.

```text
$ git push --force-with-lease origin feature/ansible-curl
To https://github.com/Gulzhan000/infra---project.git
 + 413e1d0...ea57cb2 feature/ansible-curl -> feature/ansible-curl (forced update)
```
```text
$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

```text
$ git pull
From github.com:Gulzhan000/infra---project
   5f4f56f..fe16357  main       -> origin/main
Updating 5f4f56f..fe16357
Fast-forward
 ansible/playbook.yml | 5 +++++
 1 file changed, 5 insertions(+)
```

Pull Request №1 создан и слит через GitHub: https://github.com/Gulzhan000/infra---project/pull/1 . Описание объясняет, что изменено и зачем. Задача установки curl есть в main.

## 3.3 Уборка веток

```text
$ git branch -d feature/ansible-curl
Deleted branch feature/ansible-curl (was ea57cb2).
```

```text
$ git push origin --delete feature/ansible-curl
To https://github.com/Gulzhan000/infra---project.git
 - [deleted]         feature/ansible-curl
```

```text
$ git fetch --prune

```

```text
$ git branch -a
  fix/app-port
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

Ветка feature/ansible-curl удалена локально и на сервере.

## 4.1 Docker

```text
$ git switch main
Already on 'main'
Your branch is up to date with 'origin/main'.
```

```text
$ git pull
Already up to date.
```

```text
$ git switch -c feature/docker-page
Switched to a new branch 'feature/docker-page'
```

В index.html перенесла простую страницу midterm devops с автором Gulzhan000. В Dockerfile после COPY добавлена строка EXPOSE 80.

```text
$ docker --version
Docker version 29.9.2, build a82e528901260c8df6073311379eac65f92db204
```

```text
$ docker build -t infra-web:1.0 docker/
Step 1/4 : FROM nginx:1.27-alpine
 ---> 65645c7bb6a0
Step 2/4 : COPY index.html /usr/share/nginx/html/
 ---> 7588f6be0292
Step 3/4 : EXPOSE 80
 ---> Running in 8dbf4cf668b4
 ---> Removed intermediate container 8dbf4cf668b4
 ---> 02e48c8d698f
Step 4/4 : HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
 ---> Running in fc04018fc0eb
 ---> Removed intermediate container fc04018fc0eb
 ---> a4f17fb051fc
Successfully built a4f17fb051fc
Successfully tagged infra-web:1.0
```

```text
$ docker run -d --name infra-web -p 8080:80 infra-web:1.0
74a1521a3640889d1b90418414643521c500acd3f986e365524fd7ffb2bb09d2
```

```text
$ curl --fail --silent --show-error --retry 10 --retry-connrefused --retry-delay 1 http://localhost:8080
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>midterm devops</title>
</head>
<body>
  <h1>midterm devops</h1>
  <p>Автор: Gulzhan000</p>
  <p>Учебный проект по Git, Docker и Ansible.</p>
</body>
</html>
```

```text
$ docker exec infra-web wget -qO- http://localhost/
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>midterm devops</title>
</head>
<body>
  <h1>midterm devops</h1>
  <p>Автор: Gulzhan000</p>
  <p>Учебный проект по Git, Docker и Ansible.</p>
</body>
</html>
```

```text
$ docker rm -f infra-web
infra-web
```

Образ собрался, curl вернул мою страницу. Команда из HEALTHCHECK внутри контейнера тоже сработала. После проверки контейнер удалила.

```text
$ git add docker/Dockerfile docker/index.html README.md

```

```text
$ git commit -m "feat(docker): added project page"
[feature/docker-page c584d26] feat(docker): added project page
 3 files changed, 29 insertions(+), 2 deletions(-)
```

```text
$ git push -u origin feature/docker-page
remote: 
remote: Create a pull request for 'feature/docker-page' on GitHub by visiting:        
remote:      https://github.com/Gulzhan000/infra---project/pull/new/feature/docker-page        
remote: 
To https://github.com/Gulzhan000/infra---project.git
 * [new branch]      feature/docker-page -> feature/docker-page
branch 'feature/docker-page' set up to track 'origin/feature/docker-page'.
```

```text
$ git switch main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.
```

```text
$ git pull
From github.com:Gulzhan000/infra---project
   fe16357..e1e31e7  main       -> origin/main
Updating fe16357..e1e31e7
Fast-forward
 README.md         | 18 ++++++++++++++++++
 docker/Dockerfile |  1 +
 docker/index.html | 12 ++++++++++--
 3 files changed, 29 insertions(+), 2 deletions(-)
```

Pull Request №2 создан и слит через GitHub: https://github.com/Gulzhan000/infra---project/pull/2 . Страница, EXPOSE 80 и раздел запуска Docker теперь в main.

## 5.1 Отмена изменений и исправление коммита

```text
$ git diff -- docker/Dockerfile
diff --git a/docker/Dockerfile b/docker/Dockerfile
index 686201f..da5e344 100755
--- a/docker/Dockerfile
+++ b/docker/Dockerfile
@@ -2,3 +2,4 @@ FROM nginx:1.27-alpine
 COPY index.html /usr/share/nginx/html/
 EXPOSE 80
 HEALTHCHECK CMD wget -qO- http://localhost/ || exit 1
+temporary line
```

```text
$ git restore docker/Dockerfile

```

```text
$ git diff -- docker/Dockerfile

```

```text
$ git add README.md

```

```text
$ git commit -m "docs: updted readme"
[main 60bd1ca] docs: updted readme
 1 file changed, 1 insertion(+), 7 deletions(-)
```

```text
$ git commit --amend -m "docs: updated readme"
[main e70b127] docs: updated readme
 Date: Wed Oct 28 17:01:01 2026 +0000
 1 file changed, 1 insertion(+), 7 deletions(-)
```

git restore убрал лишнюю строку из Dockerfile. Опечатка в сообщении коммита исправлена до отправки на сервер.

```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra---project.git
   e1e31e7..e70b127  main -> main
branch 'main' set up to track 'origin/main'.
```

## 5.2 Отмена опубликованного коммита

```text
$ git add .gitignore

```

```text
$ git commit -m "removed log ignore rule"
[main 7f5f71f] removed log ignore rule
 1 file changed, 1 deletion(-)
```

```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra---project.git
   e70b127..7f5f71f  main -> main
branch 'main' set up to track 'origin/main'.
```

```text
$ git revert --no-edit 7f5f71fa24be7d1fa3d23746d62a5e8e8ddd7be8
[main 74feee5] Revert "removed log ignore rule"
 Date: Wed Oct 28 17:01:14 2026 +0000
 1 file changed, 1 insertion(+)
```

```text
$ git commit --amend -m "revert: restored log ignore rule" -m "this reverts commit 7f5f71fa24be7d1fa3d23746d62a5e8e8ddd7be8."
[main 6bc0731] revert: restored log ignore rule
 Date: Wed Oct 28 17:01:14 2026 +0000
 1 file changed, 1 insertion(+)
```

```text
$ git log -2 --oneline
6bc0731 revert: restored log ignore rule
7f5f71f removed log ignore rule
```

```text
$ git check-ignore app.log
app.log
```

Строка *.log вернулась в .gitignore. Для уже опубликованного изменения использовала revert: он сохраняет общую историю и не требует force-push. Сообщение нового revert-коммита перед push привела к нижнему регистру.

```text
$ git push -u origin main
To https://github.com/Gulzhan000/infra---project.git
   7f5f71f..6bc0731  main -> main
branch 'main' set up to track 'origin/main'.
```

```text
$ git push origin --delete feature/docker-page
To https://github.com/Gulzhan000/infra---project.git
 - [deleted]         feature/docker-page
```

```text
$ git branch -d feature/docker-page
Deleted branch feature/docker-page (was c584d26).
```

```text
$ git branch -d fix/app-port
Deleted branch fix/app-port (was 4aa7227).
```

```text
$ git fetch --prune

```

```text
$ git branch -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
```

## Проверка перед сдачей

```text
$ git log --oneline --graph --all
* 6bc0731 revert: restored log ignore rule
* 7f5f71f removed log ignore rule
* e70b127 docs: updated readme
*   e1e31e7 merged docker page
|\  
| * c584d26 feat(docker): added project page
|/  
*   fe16357 merged curl installation
|\  
| * ea57cb2 feat(ansible): added curl installation
|/  
* 5f4f56f updated readme on github
*   f5de312 resolved app port conflict
|\  
| * 4aa7227 changed app port to 9090
* | 25c6139 changed app port to 8000
|/  
* 00c0a95 added health check
* 89db7a1 updated project description
* 4950d43 added ansible files
* ad7c68e added docker files
* c0c6708 added project description
* 531616d added gitignore
```

```text
$ git check-ignore .env app.log
.env
app.log
```

```text
$ git show --stat 531616da9767d78f06684873d0272d319523f205
commit 531616da9767d78f06684873d0272d319523f205
Author: Gulzhan000 <ashuubayeva_g@iuca.kg>
Date:   Wed Oct 28 16:52:59 2026 +0000

    added gitignore

 .gitignore | 3 +++
 1 file changed, 3 insertions(+)
```

Первый коммит содержит только .gitignore. Файлы проекта добавлены тремя отдельными коммитами. В main есть задача установки curl, EXPOSE 80 и HEALTHCHECK. Маркеров конфликта в all.yml нет. Оба Pull Request слиты, временные ветки удалены.

## 5.3 Контрольные вопросы

1. **Что такое коммит и зачем нужен индекс?** Коммит сохраняет выбранные изменения в истории. Индекс нужен, чтобы через git add выбрать, что войдёт в следующий коммит.

2. **Что такое ветка?** Это указатель на коммит. В отдельной ветке можно работать над задачей, а потом объединить результат с main.

3. **Что такое HEAD?** HEAD показывает текущее положение в репозитории. Обычно он указывает на активную ветку.

4. **Чем fetch отличается от pull?** fetch получает данные с сервера, не объединяя их с текущей веткой. pull получает изменения и включает их в текущую ветку слиянием или rebase, в зависимости от настроек.

5. **Почему нельзя добавлять .env?** В нём могут быть пароли и токены. После отправки они останутся в истории даже при удалении файла. Поэтому .env игнорируется, а в .env.example оставляют только пример без секретов.

