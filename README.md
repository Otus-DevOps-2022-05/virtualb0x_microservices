# virtualb0x_microservices
virtualb0x microservices repository
## Выполнено ДЗ 15
Все задания без звездочек

Познакомились с тем, как развернуть GITLAB
Посмотрели на то, как в методичке не хватает скринов
Кстати, теперь не master, main в исключениях на динамический репозиторий
Все заработало



## Выполнено ДЗ 14
Docker при инициализации контейнера может подключить к нему  одну сеть.
Дополнительные сети подключаются командой:

> docker network connect <network> <container>

При запуске создаваемые docker-compose контейнеры получают имя в формате <имя проекта>_<имя контейнера>_<инкремент>
По-умолчанию в качестве <имя проекта> берётся имя директории.

```

Изменить его можно:
задав переменную окружения `COMPOSE_PROJECT_NAME` в файле `.env` либо экспортировав её

```
$ export COMPOSE_PROJECT_NAME=reddit
$ docker-compose up -d
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED          STATUS          PORTS                                       NAMES
3e021f50ca1c   username/post:1.0      "python3 post_app.py"    16 seconds ago   Up 14 seconds                                               reddit-post-1
f76ca39abfc7   username/ui:1.0        "puma --debug -w 2"      16 seconds ago   Up 12 seconds   0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   reddit-ui-1
3bd27fca2c1d   username/comment:1.0   "puma --debug -w 2"      16 seconds ago   Up 12 seconds                                               reddit-comment-1
75889dbc4d73   mongo:3.2              "docker-entrypoint.s…"   31 hours ago     Up 14 seconds   27017/tcp                                   reddit-post_db-1
```

указав его в строке запуска через ключ `-p`

```
$ docker-compose -p reddit_otus up -d
$ docker ps
CONTAINER ID   IMAGE                  COMMAND                   CREATED          STATUS          PORTS                    NAMES
9597baea0b53   mongo:3.2              "docker-entrypoint.s…"   21  minutes ago   Up 21 minutes   27017/tcp                reddit_otus_post_db_1
95c2eb7cd78f   saaverdo/post:1.0      "python3 post_app.py"    21  minutes ago   Up 21 minutes                            reddit_post_1
ded8cc33b8d0   saaverdo/comment:1.0   "puma"                   21  minutes ago   Up 21 minutes                            reddit_otus_comment_1
d3f8e8e6c178   saaverdo/ui:1.0        "puma"                   21  minutes ago   Up 21 minutes   0.0.0.0:9292->9292/tcp   reddit_otus_ui_1
```

Кроме того, имя контейнера можно задать директивой `container_name:` - оно в результате будет без префиксов и суффиксов.




## Выполнено ДЗ 13

1. Скопипастили исходники Dockerfile в целевые файлы
2. Обнаружили что не хватает MarkupSafe, добавили его в файл requerments.txt, взлетело
3. Создали сетку, запустили контейнеры, проверили что можно сделать пост.

___________
Переопределила сетевые алиасы, запустила контейнеры с передачей переменных окружения
```
docker run -d --network=reddit --network-alias=loa_post_db --network-alias=loa_comment_db mongo:latest
docker run -d --network=reddit --network-alias=loa_post --env POST_DATABASE_HOST=loa_post_db rasdark/post:1.0
docker run -d --network=reddit --network-alias=loa_comment --env COMMENT_DATABASE_HOST=loa_comment_db  virtualb0xloame/comment:1.0
docker run -d --network=reddit -p 9292:9292 --env POST_SERVICE_HOST=loa_post --env COMMENT_SERVICE_HOST=my_comment virtualb0xloame/ui:1.0
```
Сделать-то сделала, а зачем - не поняла, и без этого нормально работает

Оптимизировала образы сервиса ui с использованием в качестве базового образа - ubuntu:16 и alpine
virtualb0xloame/ui        3.0            e813e6ca961c   6 minutes ago    310MB
virtualb0xloame/ui        2.0            602f2f0906a1   46 minutes ago   464MB
virtualb0xloame/ui        1.0            0da86741b033   46 hours ago     772MB
Разница, конечно есть, но версионирование чутка хромает

comment:
virtualb0xloame/comment   2.0            a2d60738b076   About a minute ago   293MB
virtualb0xloame/comment   1.0            32395ac1eccf   46 hours ago         770MB

Создала том, подключила том к контейнеру с монгой, проверка работы приложения в целом (с остановкой контейнера с базой)

## Выполнено ДЗ 12

 Я забыла написать описание, теперь мне лень
