[-к оглавлению-](./README.md)

# Docker cheat sheet

## Что это такое
Image(образ)–собранная подсистема, необходимая для работы процесса, сохраненная в образе  
Container–процесс, инициализированный на базе образа  

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

**docker commit *[container_id]* *[docker_hub_name]/[image_name]*** - позволяет контейнер (работающий или остановленныйы) превратить в образ  

    Пример:
      docker commit mycontainer my_dockerhub_name/my_new_image
    Чтобы запустить:
      docker run my_dockerhub_name/my_new_image

**docker push *[image_id]***  
**docker push *[docker_hub_name]/[image_name]*** - запушит контейнер в докерхаб  

    Пример:
      docker commit mycontainer my_dockerhub_name/my_new_image
    Чтобы добавить тег (обычно это latest):
      my_dockerhub_name/my_new_image:new_tag
    Еще примеры:
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
    -h - задать имя хоста
      Пример: если через терминал зайти на контейнер, будет root@newhostname:/#
    -d, --detach - запуск в фоновом режиме
    -i, --interactive - оставить STDIN открытым, даже если контейнер запущен в неприкрепленном режиме
    -t, --tty - запустить псевдотерминал
      Пример: docker run -it [image_id] bash
    -p, --publish - проброс портов.
      Пример: -p наш_порт:порт_контейнера
    -P, --publish-all  - **уточнить**
    -v, --volume - подключение внешней папки
    --link - соединение между контейнерами
      --link mysqlserver:db (значит что дб будет в новом контейнере соединено с контейнером mysqlserver)

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

**docker exec -it *[container_id]** bash** - подключиться через bash к запущенному контейнеру
	
	Если внутри контейнера не удастся найти оболочку bash, то можно запустить простую оболочку sh
	$ docker exec -it [container_id] sh

**docker ps** - показать запущенные контейнеры  

    -a - показывает все контейнеры, даже остановленные
    docker ps -aq -f status=exited - вывести id всех остановленных контейнеров

**docker rm *[container_id]*** - удалить один или несколько контейнеров

    docker rm $(docker ps -a -q) - удалит все контейнеры
    docker rm -v $(docker ps -aq -f status=exited) - удалит все остановленные контейнеры и их volume
    docker rm -v [container_id] - удаление контейнера и его volume

**docker <system|container|image|volume|network> prune** - удалить лишние контейнеры, образы, сети и томы

    docker container prune - удалит остановленные контейнеры
    docker container prune --filter "until=24h" - удалит тех которые остановлены более суток назад

**docker rename *[old_name]* *[new_name]*** - переименование контейнера  

###### Инспектирование контейнера
**docker inspect *[container_id]*** - получение гигантского списка с инфой о контейнере    

    docker inspect [container_id] | grep IPAddress - отфильтрует информацию о контейнере оставит только IPAddress

**docker diff *[container_id]*** - список файолов которые были измененны в работающем контейнере  

**docker logs *[container_id]*** - список событий произошедших в контейнере (при выходе из контейнера этот список не уничтожается)  


## Dockerfile

ENTRYPOINT - команда для запуска приложения в контейнере (по умолчанию /bin/sh-c)  
Имеет 2 режима выполнения:
- exec: ENTRYPOINT ["executable", "param1", "param2"] (исполняемая форма, предпочтительно)  
- shell: ENTRYPOINT command param1 param2 (форма оболочки)  
    
    
    Отличия ENTRYPOINT от CMD:
    https://habr.com/ru/company/southbridge/blog/329138/

Примеры:  

    FROM <имя-образа>:<tag(не обязательно)> —какой образ использовать в качестве базы (должна быть первой строкой в любом Dockerfile).
    RUN <команда> — запустить указанную команду внутри контейнера.
    •CMD <команда> —выполнить команду при запуске контейнера (обычно идет последней).
    •EXPOSE <порт> —список портов, которые будет слушать контейнер (используется механизмом линковки).
    •ADD<путь> <путь> —скопировать файл/каталог внутрь контейнера/образа (первый аргумент может быть URL).
    •ENTRYPOINT <команда> —команда для запуска приложения в контейнере (по умолчанию /bin/sh-c).
    •WORKDIR <путь> —сменить каталог внутри контейнера

     ==
    # Use an official Python runtime as a parent image
    FROM python:2.7-slim

    # Set the working directory to /app
    WORKDIR /app
    # Copy the current directory contents into the container at /app
    ADD . /app
    # Install any needed packages specified in requirements.txt
    RUN pip install --trusted-host pypi.python.org -r req.txt
    # Make port 80 available to the world outside this container
    EXPOSE 80# Run app.py when the container launches
    CMD ["python", "app.py"]

    1) Создаем необходимые файлы (в примере это req.txt и app.py)
    2) Запускаем build: dockerbuild --tag hello .
    3) Запускаем контейнер из образа:dockerrun -p 4000:80 hello
    
    ---
    FROM ubuntu:18.04
    ENV http_proxy http://...
    ENV https_proxy https://...
    ADD sonarqube-7.2.1 /sonarqube-7.2.1
    RUN useradd -ms /bin/bash sonarqube && \
        apt-get update -qq && apt-get upgrade -qq && apt-get -y install default-jdk && \
        chmod 777 -R sonarqube-7.2.1/
    EXPOSE 6060
    WORKDIR /sonarqube-7.2.1/bin/linux-x86-64/

    # CMD ["sonar.sh", "start"]
    # CMD ./sonar.sh start
    CMD ./start.sh
    ---

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

    sudo usermod -aG docker $(whoami)
    sudo service docker restart

Получить информацию по контейнеру  

    docker stats [container_id]

Мануал докер  

    man docker-build


    Если надо чтобы скрипт не вылетал:
    echo "start.sh started"
    sh ./sonar.sh start
    while true
    do
      sleep 1000
    done
    echo "start.sh somehow stopped"
    
Docker behind proxy
 
    https://docs.docker.com/config/daemon/systemd/
    
    Конфигурация прокси для докера:
    https://www.thegeekdiary.com/how-to-configure-docker-to-use-proxy/ (Method 2)
    /etc/systemd/system/docker.service.d -> http-proxy.conf; https-proxy.conf


-----
Передача образа
1) Push в Docker Hub–реестр публичных и приватных репозиториев
2) Передача образа файлом
dockersave -o image_name.tar image_name
dockerload -iimage_name.tar

Передача образа. Docker Hub
Push в Docker Hub
•https://hub.docker.com
•export DOCKER_ID_USER=“username”
•dockerlogin
•dockertag my_image$DOCKER_ID_USER/my_image
•dockerpush $DOCKER_ID_USER/my_image

## Доработать

    docker build -t [tag_name] (-t - задаёт назщвание тега)


    https://habr.com/ru/company/flant/blog/336654/
