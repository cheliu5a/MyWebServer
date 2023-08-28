# WebServer
用C++实现的高性能WEB服务器，经过webbenchh压力测试可以实现上万的QPS

## 功能
*1、架构模式：采用Reactor模式和IO多路复用实现高并发的服务器。
*2、业务逻辑：应用层HTTP1.1协议，通过有限状态机和正则表达完成HTTP报文解析。
*3、读写处理：利用标准库容器封装char，实现自动增长的缓冲区。
*4、日志系统：采用单例模式、阻塞队列实现异步日志系统，记录服务器运行情况。
*5、任务处理：通过构建线程池完成多个读写任务和逻辑处理任务的并行处理。
*6、数据处理：利用RAII机制实现数据库连接池，同时实现用户登录注册功能。
*7、项目优化：应用层利用小根堆实现心跳机制检测不活跃连接，及时释放资源确保服务器在高并发下稳定与可靠。

## 环境要求
* Linux
* C++14
* MySql

## 目录树
```
.
├── code           源代码
│   ├── buffer
│   ├── config
│   ├── http
│   ├── log
│   ├── timer
│   ├── pool
│   ├── server
│   └── main.cpp
├── test           单元测试
│   ├── Makefile
│   └── test.cpp
├── resources      静态资源
│   ├── index.html
│   ├── image
│   ├── video
│   ├── js
│   └── css
├── bin            可执行文件
│   └── server
├── log            日志文件
├── webbench-1.5   压力测试
├── build          
│   └── Makefile
├── Makefile
├── LICENSE
└── readme.md
```


## 项目启动
需要先配置好对应的数据库
```bash
// 建立yourdb库
create database yourdb;

// 创建user表
USE yourdb;
CREATE TABLE user(
    username char(50) NULL,
    password char(50) NULL
)ENGINE=InnoDB;

// 添加数据
INSERT INTO user(username, password) VALUES('name', 'password');
```

```bash
make
./bin/server
```

## 单元测试
```bash
cd test
make
./test
```

## 压力测试
![image-webbench](https://github.com/markparticle/WebServer/blob/master/readme.assest/%E5%8E%8B%E5%8A%9B%E6%B5%8B%E8%AF%95.png)
```bash
./webbench-1.5/webbench -c 100 -t 10 http://ip:port/
./webbench-1.5/webbench -c 1000 -t 10 http://ip:port/
./webbench-1.5/webbench -c 5000 -t 10 http://ip:port/
./webbench-1.5/webbench -c 10000 -t 10 http://ip:port/
```
* 测试环境: Ubuntu:19.10 cpu:i5-8400 内存:8G 
* QPS 10000+

## TODO
* config配置
* 完善单元测试
* 实现循环缓冲区
* 增加logsys,threadpool测试单元(todo: timer, sqlconnpool, httprequest, httpresponse)
