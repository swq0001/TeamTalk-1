# Build in Ubuntu

## proepare

```bash
$ chmod 777 *.sh
$ sudo ./make_hiredis.sh
$ sudo ./make_protobuf.sh
$ sudo apt install liblog4cxx-dev     # log4cxx
$ sudo apt install libmysqlclient-dev # mysql.h
```

## Base

```bash
# github/TeamTalk/server/src
$ cd base
$ cmake .
$ make
$ cp libbase.a ../lib
$ cd ..
```

## Slog

```bash
# github/TeamTalk/server/src
$ cd slog
$ cmake . && make
$ cp libslog.so ../lib
$ cd ..
```

## login_server

```bash
# github/TeamTalk/server/src
$ mkdir -p bin/login_server
$ cmake ../../login_server/
$ make
```

## msg_server

```bash
$ mkdir -p bin/msg_server
$ cmake ../../msg_server/
$ make
```

## route_server

```bash
$ mkdir -p bin/route_server
$ cmake ../../route_server/
$ make
```

## db_proxy_server

```bash
$ vim  db_proxy_server/CMakeList.txt
# SET(MYSQL_LIB_NAME mysqlclient_r)
SET(MYSQL_LIB_NAME mysqlclient) 

$ mkdir -p bin/db_proxy_server
$ cmake ../../db_proxy_server/
$ make
```

## mysql授权

1. 创建一个专用的用户名，尽量别用root用户
```bash
$ mysql -uroot -p

# 插入一个用户，localhost代表该用户只能在127.0.0.1的机器上连接mysql
$ insert into mysql.user(Host,User,Password) values("localhost","myuser",password("12345"));

# 单个IP白名单，这个用户可以在10.0.84.226机器上访问mysql
# $ grant all privileges on teamtalk.* to teamtalk@10.0.84.226 identified by '12345';

# 不限制IP，任何机器都可以连接mysql
grant all privileges on teamtalk.* to myuser@"%" identified by '12345';

# 生效
flush privileges; # 刷新系统权限表
```