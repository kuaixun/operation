# 后台

#### 部署信息
* tomcat目录：```/usr/local/program/cproduct_browser/tomcat/```
* tomcat日志：```/usr/local/program/cproduct_browser/tomcat/logs/catalina.out```

#### 自动上线脚本
```
set -e
if [ $# -eq 0 ]; then
	echo "please parse patch package name!!!"
	exit
fi

echo "auto deploy start"
cd /usr/local/program/cproduct_browser/tomcat/webapps
pwd
echo "------------------------------------------------------------------"
echo "1.backup current package..."
tar zcf back-$(date +%y%m%d%H%M).tar.gz back/
echo "------------------------------------------------------------------"
echo "2.stop tomcat..."
ps -ef|grep '/usr/local/program/cproduct_browser/tomcat/'|grep -v grep|cut -c 9-15|xargs kill -9
echo "ps after stop tomcat"
ps -ef|grep '/usr/local/program/cproduct_browser/tomcat/'
echo "------------------------------------------------------------------"
echo "3.copy and unpackge patch..."
cp /backup/$1 .
unzip -o $1
echo "------------------------------------------------------------------"
echo "4.start tomcat..."
../bin/startup.sh
echo "ps after start tomcat"
ps -ef|grep '/usr/local/program/cproduct_browser/tomcat/'
echo "------------------------------------------------------------------"
echo "5.check tomcat after sleep 5s..."
sleep 5
tail -n 100 ../logs/catalina.out
echo "------------------------------------------------------------------"
echo "auto deploy end"


```