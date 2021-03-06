# 接口

#### 部署信息
* tomcat目录：```/usr/local/program/apache-tomcat-7.0.30/```
* tomcat日志：```/usr/local/program/apache-tomcat-7.0.30/logs/catalina.out.yyyy-MM-dd```

#### 接口测试
接口的47和48两台服务器，有对应的外网IP，上线时先上一台测试没问题之后再上其他几台；同时，47的nginx配置了其他几台的访问路径，全部上线后也可以通过指定机器测试任意一台服务器接口。

#### 自动上线脚本
```
set -e
if [ $# -eq 0 ]; then
	echo "please parse patch package name!!!"
	exit
fi

echo "auto deploy start"
cd /usr/local/program/apache-tomcat-7.0.30/webapps
pwd
echo "------------------------------------------------------------------"
echo "1.backup current package..."
tar zcf suferDeskInteFace-$(date +%y%m%d%H%M).tar.gz suferDeskInteFace/
echo "------------------------------------------------------------------"
echo "2.stop tomcat..."
ps -ef|grep '/usr/local/program/apache-tomcat-7.0.30/'|grep -v grep|cut -c 9-15|xargs kill -9
echo "ps after stop tomcat"
ps -ef|grep '/usr/local/program/apache-tomcat-7.0.30/'
echo "------------------------------------------------------------------"
echo "3.copy and unpackge patch..."
cp /backup/$1 .
unzip -o $1
echo "------------------------------------------------------------------"
echo "4.start tomcat..."
../bin/startup.sh
echo "ps after start tomcat"
ps -ef|grep '/usr/local/program/apache-tomcat-7.0.30/'
echo "------------------------------------------------------------------"
echo "5.check tomcat after sleep 5s..."
sleep 5
tail -n 100 ../logs/catalina.out.20$(date +%y-%m-%d)
echo "------------------------------------------------------------------"
echo "auto deploy end"
```