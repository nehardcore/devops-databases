# Задача 1

Найдите и приведите управляющие команды для:

вывода списка БД:

    \l[+]   [PATTERN] - list databases
подключения к БД:

    \c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo} - connect to new database (currently "postgres")
вывода списка таблиц:

     \dt[S+] [PATTERN] - list tables
вывода описания содержимого таблиц:

      \d[S+]  NAME - describe table, view, sequence, or index    
выхода из psql:

    \q - quit psql
    
# Задача 2
<img width="828" alt="Screenshot 2023-03-31 at 23 01 31" src="https://user-images.githubusercontent.com/97674120/229268716-cc706c7a-f0dd-4c2f-8891-888fec48b0c1.png">

# Задача 3
>Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам как успешному выпускнику курсов DevOps в Нетологии предложили провести разбиение таблицы на 2: шардировать на orders_1 - price>499 и orders_2 - price<=499.

>Предложите SQL-транзакцию для проведения этой операции.

    BEGIN;
    CREATE TABLE orders_temp (
        id integer NOT NULL,
        title character varying(80) NOT NULL,
        price integer DEFAULT 0
    ) PARTITION BY RANGE (price);

    CREATE TABLE orders_1 PARTITION OF orders_temp FOR VALUES FROM (500) TO (MAXVALUE);
    CREATE TABLE orders_2 PARTITION OF orders_temp FOR VALUES FROM (MINVALUE) TO (500);
    INSERT INTO orders_temp SELECT * FROM orders;
    DROP TABLE orders;
    ALTER TABLE orders_temp RENAME TO orders;
    COMMIT;
    
>Можно ли было изначально исключить ручное разбиение при проектировании таблицы orders?

Не совсем понял про "ручное разбиение". Даже если при проктировании мы сразу разделим таблицу на две по какому-то признаку - это тоже, по сути, является ручным разбиением.

# Задача 4
>Используя утилиту pg_dump, создайте бекап БД test_database

    cch@MBP-Costas 06-db-04-postgresql % docker exec -it MyPostgres sh -c "pg_dump -U postgres test_database > /var/lib/postgresql/data/db_backup_o1_o2.sql"
>Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

Добавить ключевое слово UNIQUE:

<img width="444" alt="Screenshot 2023-04-01 at 21 44 43" src="https://user-images.githubusercontent.com/97674120/229331875-a1cd35ff-6c0f-4264-bb19-2c916ca4669e.png">



