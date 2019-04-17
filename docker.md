[-к оглавлению-](./README.md)

# Docker cheat sheet

### Установка на Windows
1. https://docs.docker.com/docker-for-windows/install/
2. https://docs.docker.com/toolbox/ - *для более старых версий windows*

### Установка на Linux
- https://docs.docker.com/install/linux/docker-ce/ubuntu/ - *community edition*
- https://docs.docker.com/compose/install/ - *docker compose*

## Работа с образами
docker run *<image_id>*  
 - --name - назвать контейнер который запустится из данного имейджа
 - -d, --detach - запуск в бэкграунде
 - -i, --interactive - оставляет соединение
 - -t, --tty - подключает терминал. Пример docker run -it *<image_id>* bash
 - -p, --publish - проброс портов. Пример -p наш\_порт:порт\_контейнера
 - -P, --publish-all  - **уточнить**
 - -v, --volume - подключение внешней папки



## Работа с контейнерами
docker start *<container_id>* - перезапуск контейнера который был остановлен и виден в docker ps -a
docker stop *<container_id>* - остановка запущенного контейнера


docker run -h имя_хоста_сами_задаём -it ubuntu bash ???? что такое -h
docker inspect <> - инфа о контейнере
docker inspect <> | grep IPAddress - фильтровать

docker run --name Имя_сами_задаём -it ubuntu bash

docker diff <> - список файолов измененных в работающем контейнере

docker logs <> - список событий выполненных в контейнере

docker rm <> - удалить контейнер

docker ps -aq -f status=exited - вывести id всех остановленных контейнеров
docker rm -v $(docker ps -aq -f status=exited)

docker run -d <> - "-d" = в фоновом режиме

docker run -d -p 1234:8080 <> - "-p наш:контейнер" порт

docker images - список образов


## Полезная информация
Добавить docker в группу sudo:  

    sudo ussermod -aG docker $(whoami)
    sudo service docker restart

Получить информацию по контейнеру

    docker stats <container_id>


Посмотреть каждый уровень контейнерами

    docker history <container_id>

Мануал докер

    man docker-build


## Доработать

    docker build -t <tag_name> (-t - задаёт назщвание тега)
