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