# Домашнее задание к занятию "6.4. PostgreSQL"

## Обязательная задача 1
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

- вывода списка БД
- подключения к БД
- вывода списка таблиц
- вывода описания содержимого таблиц
- выхода из psql

### Ответ
Подготовим контейнер
```
vagrant@vagrant:~$ docker pull postgres:13
vagrant@vagrant:~$ docker volume create sql_data
vagrant@vagrant:~$ docker run --rm --name pg-docker -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v sql_data:/var/lib/p
ostgresql/data postgres:13
vagrant@vagrant:~$ docker exec -it pg-docker bash
root@512eac3edf70:/# psql -U postgres -W
```
- вывод списка БД
```
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 |
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)
```
- подключениe к БД
```
postgres=# \c postgres
```
- вывод списка таблиц
```
postgres=# \dt[S+]
```
- вывод описания содержимого таблиц
```
postgres=# \d[S+]
```
- выход из psql
```
postgres=# \q
```
## Обязательная задача 2
Используя psql создайте БД test_database.

Изучите бэкап БД.

Восстановите бэкап БД в test_database.

Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.

### Ответ
Создадим БД:
```
postgres=# CREATE DATABASE test_database;
CREATE DATABASE
```
Восстановим backup:
```
root@4b26a36c218f:/# psql -U postgres -f /var/lib/postgresql/backup/test_dump.sql test_database
```
Подключаемся к БД и смотрим список таблиц:
```
postgres=# \c test_database
Password:
You are now connected to database "test_database" as user "postgres".
test_database=# \dt
         List of relations
 Schema |  Name  | Type  |  Owner
--------+--------+-------+----------
 public | orders | table | postgres
(1 row)
```
Проведем операцию ANALYZE для сбора статистики по таблице:
```
test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
```
Найдем столбец таблицы orders с наибольшим средним значением размера элементов в байтах
```
test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width
-----------
         4
        16
         4
(3 rows)
```
## Обязательная задача 3

Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

### Ответ
Пересоздадим таблицу orders как секционированную:
```
test_database=# alter table orders rename to orders_simple;
ALTER TABLE
test_database=# create table orders (id integer, title varchar(80), price integer) partition by range(price);
CREATE TABLE
test_database=# create table orders_2 partition of orders for values from (0) to (499);
CREATE TABLE
test_database=# create table orders_1 partition of orders for values from (499) to (999999999);
CREATE TABLE
test_database=# insert into orders (id, title, price) select * from orders_simple;
INSERT 0 8
```
"Ручное" разбиение можно было исключить при проектировании таблицы orders, изначально создав ее секционированной.
## Обязательная задача 4
Используя утилиту pg_dump создайте бекап БД test_database.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?
### Ответ
Создадим backup:
```
root@4b26a36c218f:/# pg_dump -U postgres -d test_database > test_database_dump.sql
```
Для уникальности можно добавить индекс с проверкой уникальности значений.
```
CREATE UNIQUE INDEX title_idx ON orders (title);
```