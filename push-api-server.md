# 服务端消息组件

#### **启动** 
1. ```cd /usr/local/program/cpush```
1. ```./run_server.sh &```

#### **停止** 
1. ```ps -ef|grep push```会看到一个进程
2. 针对ps显示的进程```kill -9 进程号```

#### **备份** 
每次上线必须备份，备份方法为
1. ```cd /usr/local/program/cpush```
2. ```cp push-api-server-1.0-SNAPSHOT.jar push-api-server-1.0-SNAPSHOT.jar.日期时间戳```
3. 拷贝新的```push-api-server-1.0-SNAPSHOT.jar```包到当前目录