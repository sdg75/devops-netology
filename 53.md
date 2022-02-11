# Домашнее задание к занятию "5.3. Введение. Экосистема. Архитектура. Жизненный цикл Docker контейнера"


## Обязательная задача 1
Сценарий выполения задачи:

создайте свой репозиторий на https://hub.docker.com;
выберете любой образ, который содержит веб-сервер Nginx;
создайте свой fork образа;
реализуйте функциональность: запуск веб-сервера в фоне с индекс-страницей, содержащей HTML-код ниже:
<html>
<head>
Hey, Netology
</head>
<body>
<h1>I’m DevOps Engineer!</h1>
</body>
</html>
Опубликуйте созданный форк в своем репозитории и предоставьте ответ в виде ссылки на https://hub.docker.com/username_repo.

### Ответ

Подготовим Dockerfile вида:
```
vagrant@vagrant:~$ cat ./Dockerfile
FROM nginx:alpine

RUN rm -rf /usr/share/nginx/html/*

COPY index.html /usr/share/nginx/html/

EXPOSE 80

ENTRYPOINT ["nginx", "-g", "daemon off;"]
```
Соберем Docker образ
```
vagrant@vagrant:~$ docker build -t d7k5/nginx:alpine .
```
Залогинимся в Docker и запушим полученный образ:
```
vagrant@vagrant:~$ docker login -u d7k5
vagrant@vagrant:~$ docker push d7k5/nginx:alpine
```
Результат доступен по ссылке:
https://hub.docker.com/repository/docker/d7k5/nginx

## Обязательная задача 2
Посмотрите на сценарий ниже и ответьте на вопрос: "Подходит ли в этом сценарии использование Docker контейнеров или лучше подойдет виртуальная машина, физическая машина? Может быть возможны разные варианты?"

Детально опишите и обоснуйте свой выбор.

--

Сценарий:
- Высоконагруженное монолитное java веб-приложение;
- Nodejs веб-приложение;
- Мобильное приложение c версиями для Android и iOS;
- Шина данных на базе Apache Kafka;
- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana;
- Мониторинг-стек на базе Prometheus и Grafana;
- MongoDB, как основное хранилище данных для java-приложения;
- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

### Ответ
- Высоконагруженное монолитное java веб-приложение.

Монолитное - следовательно в микросерверах не реализуемо без изменения кода,
высоконагруженное - необходим физический доступ к ресурсами, без использования гипервизора.
В данном сценарии лучше подойдет физическая машина.

- Nodejs веб-приложение.

Веб-приложения хорошо реализуются в микросерверах.
В данном сценарии использование Docker вполне оправданно.

- Мобильное приложение c версиями для Android и iOS.

Приложения для Android и iOS - разные среды исполнения.
В данном сценарии целесообразно использовать виртуальную машину.

- Шина данных на базе Apache Kafka.

Kafka неплохо реализуется на микросерверах. Есть готовые Docker образы.
В данном случае решением будет Docker.

- Elasticsearch кластер для реализации логирования продуктивного веб-приложения - три ноды elasticsearch, два logstash и две ноды kibana.

Вполне реализуемо на Docker, но с точки зрения отказоустойчивости кластер лучше реализовать на виртуальных машинах.

- Мониторинг-стек на базе Prometheus и Grafana.
- 
Prometheus и Grafana хорошо реализуются на Docker.

- MongoDB, как основное хранилище данных для java-приложения.
- 
Как для основного хранилища данных в данном случае Docker не подходит.
Решением будет виртуальная машина или физический сервер в зависимости от нагрузки.

- Gitlab сервер для реализации CI/CD процессов и приватный (закрытый) Docker Registry.

Для хранения образов Docker и системы версионирования случае лучше подойдет виртуальная машина. 


## Обязательная задача 3
- Запустите первый контейнер из образа centos c любым тэгом в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
- Запустите второй контейнер из образа debian в фоновом режиме, подключив папку /data из текущей рабочей директории на хостовой машине в /data контейнера;
- Подключитесь к первому контейнеру с помощью docker exec и создайте текстовый файл любого содержания в /data;
- Добавьте еще один файл в папку /data на хостовой машине;
- Подключитесь во второй контейнер и отобразите листинг и содержание файлов в /data контейнера.

### Ответ
Загрузим docker-образ Centos:
```
vagrant@vagrant:~$ docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```

Загрузим docker-образ Debian:
```
vagrant@vagrant:~$ docker pull debian
Using default tag: latest
latest: Pulling from library/debian
0c6b8ff8c37e: Pull complete
Digest: sha256:fb45fd4e25abe55a656ca69a7bef70e62099b8bb42a279a5e0ea4ae1ab410e0d
Status: Downloaded newer image for debian:latest
docker.io/library/debian:latest
```
Запустим контейнер CentOS:
```
vagrant@vagrant:~$ docker run -itdv /home/vagrant/data:/data:rw centos /bin/bash
9cde2e79ffe87b4b276fec4308993de65c09052e9e0775849045bdc6135b5b62
```
Запустим контейнер Debian:
```
vagrant@vagrant:~$ docker run -itdv /home/vagrant/data:/data:rw debian /bin/bash
ae00e61085965966fd7738a50783f6f617ac63d2d80ff2da6caac2ae65e926ea
```
Проверим статус:
```
vagrant@vagrant:~$ docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS              PORTS     NAMES
ae00e6108596   debian    "/bin/bash"   About a minute ago   Up About a minute             hardcore_curie
9cde2e79ffe8   centos    "/bin/bash"   About a minute ago   Up About a minute             hungry_heyrovsky
```
Подключимся к контейнеру Centos и создадим файл:
```
vagrant@vagrant:~$ docker exec -it 9cde2e79ffe8 /bin/bash
[root@9cde2e79ffe8 /]# touch /data/first.file
[root@9cde2e79ffe8 /]# echo "first string" >> /data/first.file
[root@9cde2e79ffe8 /]# cat /data/first.file
first string
```
Добавим еще один файл в папку /data на хостовой машине:
```
vagrant@vagrant:~$ touch ./data/second.file
vagrant@vagrant:~$ echo "another string" >> ./data/second.file
vagrant@vagrant:~$ cat  ./data/second.file
another string
```
Подключимся во второй контейнер и отобразим листинг и содержание файлов в /data контейнера:
```
vagrant@vagrant:~$ docker exec -it ae00e6108596 /bin/bash
root@ae00e6108596:/# ls /data
first.file  second.file
root@ae00e6108596:/# cat /data/first.file
first string
root@ae00e6108596:/# cat /data/second.file
another string
```