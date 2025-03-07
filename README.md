# Домашнее задание #1: Docker

## Формулировка

В рамках первого домашнего задания требуется пометить в docker и запустить backend и frontend части приложения TODO
list.

* Приложение реализует простейший TODO list:
    * добавить заметку;
    * удалить заметку;
    * вывести все заметки.
* Backend написан на Kotlin + Spring, frontend написан на Typescript + React/Redux.
* Backend запускается на порту 8080 и профиле docker.
* Backend для хранения данных использует PostgreSQL 13.
* Frontend наружу маппируется на порт 3000. общается с backend'ом по протоколу HTTP на порт 8080.

## Требования

* Для успешного выполнения задания нужно установить на host машину:
    * git;
    * opendjk 11;
    * docker;
* Код backend'а и frontend'а хранится в отдельных репозиториях, их нужно затянуть
  командой `git submodule update --init --recursive`.
* Нужно реализовать двухэтапную сборку приложений, сборку контейнеров описать в
  файлах [backend.Dockerfile](backend.Dockerfile) и [frontend.Dockerfile](frontend.Dockerfile).
* Для персистентного хранения нужно создать Volume для PostgreSQL.
* Внешний маппинг портов:
    * backend 8080:8080;
    * frontend 3000:80.
* Нужно создать две _разных_ сети (driver: bridge):
    * для взаимодействия между backend и PostgreSQL;
    * для взаимодействия backend и frontend.
* Docker compose использовать нельзя, все ресурсы описываются через docker.
* В [test.sh](test.sh) прописать `STUDENT_LABEL` свою фамилию.
* Каждый создаваемый ресурс (network, volume, container) требуется пометить label `$BASE_LABEL-$STUDENT_LABEL`. Это
  нужно для корректной очистки ресурсов.
* В результате реализации всех описанных выше шагов, должна быть возможность работать TODO list с localhost, т.е. можно
  открыть страницу в браузере и проверить работоспобность .
* Для автоматизированной проверки работоспособности выполняется запрос из контейнера frontend в контейнер backend по
  имени сервиса.

## Пояснения

* Для сборки на GitHub используется GitHub Actions, манифест сборки прописан в [main.yml](.github/workflows/main.yml).
* Backend нужно запустить с профилем `docker`. Для этого требуется внутрь контейнера пробросить переменную
  среды `SPRING_PROFILES_ACTIVE=docker`.
* Для очистки ресурсов можно использовать [cleanup.sh](cleanup.sh). Этот скрипт создает
  контейнер [ryuk](https://github.com/testcontainers/moby-ryuk) , которые ищет ресурсы, помеченные label=<name>, и
  удаляет их через 10 секунд.

## Сдача домашней работы

1. Создаете fork этого репозитория.
2. Выполняете задание (в [test.sh](test.sh) места, где нужно дописать код, помечены TODO).
3. После успешного прохождения тестов в _вашем_ репозитории (вкладка Actions), создаете Pull Request в основной
   репозиторий (Pull requests -> New pull request).