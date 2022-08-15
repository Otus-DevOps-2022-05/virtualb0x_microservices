# virtualb0x_microservices
virtualb0x microservices repository
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
