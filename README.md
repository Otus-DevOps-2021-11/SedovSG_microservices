# SedovSG_microservices
SedovSG microservices repository

## Инициализация Docker окружения в Яндекс облаке

```bash
docker-machine create \
--driver generic \
--generic-ip-address=51.250.9.140 \
--generic-ssh-user yc-user \
--generic-ssh-key ~/.ssh/id_rsa.yc \
docker-host
```

## Сборка Docker образа

```bash
$: docker build -t reddit:latest .
```

## Загрузка образа на Docker Hub

```bash
$: docker tag reddit:latest sedovsg/otus-reddit:1.0 && docker push sedovsg/otus-reddit:1.0
```

## Запуск контейнера из полученного образа

```bash
$: docker run --name reddit -d --network=host reddit:latest
```

## Создание сети

```bash
$: docker network create {name}
```

## Запуск контейнеров:

```bash
$: docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
$: docker run -d --network=reddit --network-alias=post sedovsg/post:1.0
$: docker run -d --network=reddit --network-alias=comment sedovsg/comment:1.0
$: docker run -d --network=reddit -p 9292:9292 sedovsg/ui:1.0
```

## Изменение имени сервиса

Для данной опреации используются переменные окрцжения, предоставленные Docker Compose:

- REPOSITORY_NAME - позволяет переопределить значение используемого по умолчнию директории docker-compose
- COMPOSE_PROJECT_NAME - название проекта

## Разделение сред сборок приложения

По умолчанию в Compose используются два файла `docker-compose.yml` и `docker-compose.override.yml`. Если первый файл, по соглашению, содержит всю базовую конфигурацию сервиса, то второй - может переопределения базовой конфигурации. `docker-compose.override.yml` чаще всего используют для `dev` среды, т.к. при сборке приложения нет необходимости указывать флаг `-f`.

Возможны следующие среды - `stage` и `prod`. Тогда для запуска соответсвующих файлов неоходимо исаользовать следующие команды:

```bash
$: docker-compose -f docker-compose.yml -f docker-compose.stage.yml run --build -d
$: docker-compose -f docker-compose.yml -f docker-compose.prod.yml run --build -d
