# 5.3-Docker_Aleksandr_Molokov

Задание 1
https://hub.docker.com/r/amolokov/netologi_1_fork
docker pull amolokov/netologi_1_fork
Проверял через Curl скрины в приложении

Задание 2
Сценарий:

Высоконагруженное монолитное java веб-приложение
- Физическая машина, чтобы не расходовать ресурсы на виртуализацию и из-за монолитности не будет проблем с разворачиванием на разных машинах

Nodejs веб-приложение
- Docker, для более простого воспроизведения зависимостей в рабочих средах

Мобильное приложение c версиями для Android и iOS
- Виртуальные машины, подходит для разных ОС, проще для тестирования

Шина данных на базе Apache Kafka
- Docker, есть готовые образы для apache kafka, легко откатить на стабильные версии в случае обнаружения багов

Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana
- Docker, Elasticsearch доступен для установки как образ docker, проще удалять логи, удобнее при кластеризации - меньше времени на запуск контейнеров.

Мониторинг-стек на базе Prometheus и Grafana
- Docker, есть готовые образы, приложения не хранят данные, что удобно при контейниризации, удобно масштабировать и быстро разворачивать.
MongoDB, как основное хранилище данных для java-приложения
- Физическая машина как наиболее надежное, отказоустойчивое решение.

Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry
- Docker лучшая изолированность, но скорее всего возможны все варианты.

Задание 3
Запускаем первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера
Создаю папку data 
vagrant@vagrant:~$ sudo mkdir data

Запуск контернера centos  и подключение папки /data
vagrant@vagrant:~$ sudo docker run -v /data:/data --name centos-container -d -t centos

Запуск контейнера debian и подключение папки /data
vagrant@vagrant:~$ sudo docker run -v /data:/data --name debian-container -d -t debian

Проверка запущенные контейнеров
vagrant@vagrant:~$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
03d3f5a4440d   debian    "bash"        15 seconds ago   Up 12 seconds             debian-container
4d1ff3d27ff8   centos    "/bin/bash"   4 minutes ago    Up 4 minutes              centos-container

Подключение к контернеру centos (sudo docker exec) и добавление файла в папку /data
vagrant@vagrant:/$ docker exec centos-container /bin/bash -c "echo test_line>/data/centos_file.txt"

Созадние файла в папке /data на хостовой машине
vagrant@vagrant:/$ cd /data
vagrant@vagrant:/data$ sudo nano host_file.txt

Переключение в контейнер debian и проверка файлов в папке /data в контейнере
vagrant@vagrant:/$ sudo docker exec -it debian-container /bin/bash
root@03d3f5a4440d:/# cd /data
root@03d3f5a4440d:/data# ls -l
total 8
-rw-r--r-- 1 root root 10 Nov 12 10:33 centos_file.txt
-rw-r--r-- 1 root root 18 Nov 12 10:35 host_file.txt



