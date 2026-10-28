# Infra project

Автор: Gulzhan000. Строка добавлена через веб-интерфейс GitHub.

Учебный инфраструктурный репозиторий курса 

<!-- Допишите сюда -->

Проект midterm devops: учебная практика по Git, Docker и Ansible.

## Запуск в Docker

Из корня проекта:

```bash
docker build -t infra-web:1.0 docker/
docker run -d --name infra-web -p 8080:80 infra-web:1.0
curl http://localhost:8080
```

Страница «midterm devops» доступна по адресу http://localhost:8080.

Остановить и удалить контейнер:

```bash
docker rm -f infra-web
```
