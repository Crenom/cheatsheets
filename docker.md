[-к оглавлению-](./README.md)

# Docker cheat sheet

### Установка на Windows
1. https://docs.docker.com/docker-for-windows/install/
2. https://docs.docker.com/toolbox/ - *для более старых версий windows*

### Установка на Linux
- https://docs.docker.com/install/linux/docker-ce/ubuntu/ - *community edition*
- https://docs.docker.com/compose/install/ - *docker compose*

## Работа с реестром докер (докер хабом)
**docker search *[anything]*** - поиск образа на докер хабе  

    Примеры:
    docker search nginx
    docker search nginx -- filter stars=3 --no-trunc busybox


**docker pull *[image_id]*** - выкачать образ из докер хаба  

    Примеры:
    docker pull nginx
    docker pull eon01/nginx localhost:5000/myadmin/nginx

**docker push *[image_id]*** - загрузка образа на докер хаб  

    Примеры:
    docker push eon01/nginx
    docker push eon01/nginx localhost:5000/myadmin/nginx


## Работа с образами
**docker images** - отобразит все образы  
**docker rmi *[image_id]*** - удалить один или несколько образов  

    docker rmi $(docker images -q) - удалит все образы

**docker create *[image_id]*** - создаёт контейнер из образа, но не стартует  
**docker run *[image_id]*** - создаёт и стартует контейнер из образа  

    Некоторые параметры:
    --name - назвать контейнер который запустится из данного имейджа
    -d, --detach - запуск в бэкграунде
    -i, --interactive - оставляет соединение
    -t, --tty - подключает терминал.
      Пример: docker run -it [image_id] bash
    -p, --publish - проброс портов.
      Пример: -p наш_порт:порт_контейнера
    -P, --publish-all  - **уточнить**
    -v, --volume - подключение внешней папки

**docker build .** - создать образ из докерфайла  
**docker tag** - создать тег для репозитория  
**docker history *[image_id]*** - показать каждый слой образа  
**docker import *[tgz]*** - создать образ из tgz  

**docker export *[container_id]*** - создать tar из контейнера  

    --output, -o - писать в файл.
     Пример: docker export --output="latest.tar" red_panda


## Работа с контейнерами
**docker start *[container_id]*** - перезапуск контейнера который был остановлен и виден в docker ps -a  
**docker stop *[container_id]*** - остановка запущенного контейнера  

**docker ps** - показать запущенные контейнеры  

    -a - показывает все контейнеры, даже остановленные
    docker ps -aq -f status=exited - вывести id всех остановленных контейнеров

**docker rm *[container_id]*** - удалить один или несколько контейнеров

    docker rm $(docker ps -a -q) - удалит все контейнеры
    docker rm -v $(docker ps -aq -f status=exited) - удалит все остановленные контейнеры и их volume
    docker rm -v [container_id] - удаление контейнера и его volume

**docker container prune** - удаление всех остановленных контейнеров

    docker container prune --filter "until=24h" - удалит тех которые остановлены более суток назад

**docker commit**  
**docker rename *[old_name]* *[new_name]*** - переименование контейнера

###### Инспектирование контейнера
**docker inspect []** - инфа о контейнере  
**docker inspect [] | grep IPAddress** - фильтровать  


docker diff [] - список файолов измененных в работающем контейнере  

docker logs [] - список событий выполненных в контейнере  

## Работа с сетью
Создание сети

docker network create -d overlay MyOverlayNetwork

docker network create -d bridge MyBridgeNetwork

docker network create -d overlay \
  --subnet=192.168.0.0/16 \
  --subnet=192.170.0.0/16 \
  --gateway=192.168.0.100 \
  --gateway=192.170.0.100 \
  --ip-range=192.168.1.0/24 \
  --aux-address="my-router=192.168.1.5" --aux-address="my-switch=192.168.1.6" \
  --aux-address="my-printer=192.170.1.5" --aux-address="my-nas=192.170.1.6" \
  MyOverlayNetwork

Удаление сети

docker network rm MyOverlayNetwork

Список сетей

docker network ls

Получение информации о сети

docker network inspect MyOverlayNetwork

Подключение работающего контейнера к сети

docker network connect MyOverlayNetwork nginx

Подключение контейнера к сети при его запуске

docker run -it -d --network=MyOverlayNetwork nginx

Отключение контейнера от сети

docker network disconnect MyOverlayNetwork nginx


## Полезная информация
Добавить docker в группу sudo:  

    sudo ussermod -aG docker $(whoami)
    sudo service docker restart

Получить информацию по контейнеру  

    docker stats [container_id]

Мануал докер  

    man docker-build


## Доработать

    docker build -t [tag_name] (-t - задаёт назщвание тега)


    https://habr.com/ru/company/flant/blog/336654/
