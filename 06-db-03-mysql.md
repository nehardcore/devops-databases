# Задача 1
    mysql> status
    --------------
    mysql  Ver 8.0.28 for Linux on x86_64 (MySQL Community Server - GPL)
    
    Connection id:		20
    Current database:	test_db
    Current user:		root@localhost
    SSL:			Not in use
    Current pager:		stdout
    Using outfile:		''
    Using delimiter:	;
    Server version:		8.0.28 MySQL Community Server - GPL
    Protocol version:	10
    Connection:		Localhost via UNIX socket
    Server characterset:	utf8mb4
    Db     characterset:	utf8mb4
    Client characterset:	latin1
    Conn.  characterset:	latin1
    UNIX socket:		/var/run/mysqld/mysqld.sock
    Binary data as:		Hexadecimal
    Uptime:			12 min 56 sec
    
    Threads: 2  Questions: 96  Slow queries: 0  Opens: 123  Flush tables: 3  Open tables: 98  Queries per second avg: 0.0009
    --------------

    mysql> show tables;
    +-------------------+
    | Tables_in_test_db |
    +-------------------+
    | orders            |
    +-------------------+
    1 row in set (0.00 sec)
    
    mysql> select count(*) from orders where price>300;
    +----------+
    | count(*) |
    +----------+
    |        1 |
    +----------+
    1 row in set (0.00 sec)
