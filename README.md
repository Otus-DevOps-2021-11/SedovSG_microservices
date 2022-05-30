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
```

## Устновка Gitlab Omnibus

Установка Gitlab Omnibus может быть использована в ознакомительных целях, использовать в окружении производсва - не рекомендуется.

### Запуск контейнера Gitlab

```bash
$: cd gitlab-ci && docker-compose up --build -d
```

## Установка и регистрация бегунов для выполнения задач CI/CD

```bash
$: docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest
$: docker exec -it gitlab-runner gitlab-runner register --url http://130.193.53.217 --non-interactive --locked=false --name DockerRunner --executor docker --docker-image alpine:latest --registration-token GR13489416hUfrpJPwiPrApXBuUxi --tag-list "linux,xenial,ubuntu,docker" --run-untagged
```

## Мониторинг сервисов:

```bash
$: yc compute instance create \
--name docker-host \
--hostname docker-host \
--memory=4 --zone=ru-central1-a \
--create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15GB \ 
--network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --ssh-key ~/.ssh/id_rsa.yc.pub
```

```bash
$: docker-machine create \
--driver generic \
--generic-ip-address=178.154.247.119 \
--generic-ssh-user yc-user \
--generic-ssh-key ~/.ssh/id_rsa.yc \
docker-host

$: eval $(docker-machine env docker-host)
```

### Образы в репозитории на Docker Hub:

- [sedovsg/reddit-ui](https://hub.docker.com/repository/docker/sedovsg/reddit-ui)
- [sedovsg/reddit-post](https://hub.docker.com/repository/docker/sedovsg/reddit-post)
- [sedovsg/reddit-comment](https://hub.docker.com/repository/docker/sedovsg/reddit-comment)
- [sedovsg/reddit-prometheus](https://hub.docker.com/repository/docker/sedovsg/reddit-prometheus)


## Логирование сервисов:

```bash
$: git clone --branch=logging git@github.com:express42/reddit.git src
```

```bash
$: yc compute instance create --name docker-host --hostname docker-host --memory=4 --zone=ru-central1-a --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15GB --network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4 --ssh-key ~/.ssh/id_rsa.yc.pub
```

```bash
$: docker-machine create --driver generic --generic-ip-address=51.250.85.141 --generic-ssh-user yc-user --generic-ssh-key ~/.ssh/id_rsa.yc logging
```

```bash
$: $: eval $(docker-machine env docker-host)
```