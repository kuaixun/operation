# 上线说明

#### 自助上线平台
* 地址 [http://183.207.194.129:9091/SelfServiceOnline/](http://183.207.194.129:9091/SelfServiceOnline/)
* 用户名密码使用老付的账号和验证码
* 使用cplatform用户连接服务器，sudo su获取root权限

#### FTP的使用
* 地址 121.40.50.28
* 本地使用工具如winscp上传下载文件
* 自助上线平台使用sftp命令连接ftp上传和下载文件，如```sftp -oPort=22 sam@121.40.50.28```
* 冲浪机房的/backup为共享目录，上传补丁包选择传任意一台即可

#### 一般上线步骤
* 确认服务器IP和应用地址
* 备份现有应用，如管理后台应进入tomcat的webapps目录下面：```tar zcvf back-201605091000.tar.gz back/```
* 通过ps和kill命令杀掉现有进程，如管理后台先```ps -ef|grep 'cproduct_browser/tomcat'```，然后```kill -9 pid```
* 将通过ftp上传的补丁包拷贝到webapps目录下，然后解压，如```unzip suferDeskInteFace-201605061710.zip```
* 启动tomcat，如```../bin/startup.sh```
* 查看进程和日志确保正常后通知测试人员

