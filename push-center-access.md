# 客户端消息组件

客户端消息组件是提供给客户端连接获取推送消息的服务。

#### **部署机器** 

| 内网IP | 外网IP |
| -- | -- |
| 172.16.31.21 | 183.207.194.130 |
| 172.16.31.22 | 183.207.194.131 |
| 172.16.31.23 | 183.207.194.132 |
| 172.16.31.24 | 183.207.194.133 |
| 172.16.31.25 | 183.207.194.134 |
| 172.16.31.26 | 183.207.194.135 |

#### **启动** 
1. ```export CPUSH_local_host="上面表格对应的外网IP"```
1. ```cd /usr/local/program/cpush```
1. ```./run_server.sh 8881 &```
1. ```./run_server.sh 8882 &```

#### **停止** 
1. ```ps -ef|grep push-center-access```会看到两个进程
2. 针对ps显示的两个进程```kill -9 进程号```

#### **备份** 
每次上线必须备份，备份方法为
1. ```cd /usr/local/program/cpush```
2. ```cp push-center-access-1.0-SNAPSHOT.jar push-center-access-1.0-SNAPSHOT.jar.日期时间戳```
3. 拷贝新的```push-center-access-1.0-SNAPSHOT.jar```包到当前目录
