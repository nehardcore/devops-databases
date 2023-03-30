# Задача 1

    mysql> status;
    --------------
    mysql  Ver 8.0.32 for Linux on aarch64 (MySQL Community Server - GPL)

    Connection id:		22
    Current database:	test_db
    Current user:		root@localhost
    SSL:			Not in use
    Current pager:		stdout
    Using outfile:		''
    Using delimiter:	;
    Server version:		8.0.32 MySQL Community Server - GPL
    Protocol version:	10
    Connection:		Localhost via UNIX socket
    Server characterset:	utf8mb4
    Db     characterset:	utf8mb4
    Client characterset:	latin1
    Conn.  characterset:	latin1
    UNIX socket:		/var/run/mysqld/mysqld.sock
    Binary data as:		Hexadecimal
    Uptime:			1 hour 8 min 6 sec

    Threads: 2  Questions: 149  Slow queries: 0  Opens: 212  Flush tables: 3  Open tables: 130  Queries per second avg: 0.036
    --------------

    mysql> show tables;
    +-------------------+
    | Tables_in_test_db |
    +-------------------+
    | orders            |
    +-------------------+
    1 row in set (0.01 sec)

    mysql> select count(*) from orders where price>300;
    +----------+
    | count(*) |
    +----------+
    |        1 |
    +----------+
    1 row in set (0.00 sec)
    
# Задача 2

    mysql> select * from user_attributes
        -> ;
    +------------------+-----------+---------------------------------------------------------------------+
    | USER             | HOST      | ATTRIBUTE                                                           |
    +------------------+-----------+---------------------------------------------------------------------+
    | root             | %         | NULL                                                                |
    | mysql.infoschema | localhost | NULL                                                                |
    | mysql.session    | localhost | NULL                                                                |
    | mysql.sys        | localhost | NULL                                                                |
    | root             | localhost | NULL                                                                |
    | test             | localhost | {"comment": "{\"LastName\": \"Pretty\", \"FirstName\": \"James\"}"} |
    +------------------+-----------+---------------------------------------------------------------------+
    6 rows in set (0.00 sec)

# Задача 3
<img width="1308" alt="Screenshot 2023-03-30 at 00 38 53" src="https://user-images.githubusercontent.com/97674120/228764356-7c538f57-c335-46d0-ac80-e66240a2c13b.png">
<img width="501" alt="Screenshot 2023-03-30 at 00 46 42" src="https://user-images.githubusercontent.com/97674120/228766497-558609ee-5160-4ec7-a274-f2c993000e53.png">
<img width="524" alt="Screenshot 2023-03-30 at 00 47 52" src="https://user-images.githubusercontent.com/97674120/228766942-5ad9e522-e819-49ed-9712-679efa199322.png">

# Задача 4


