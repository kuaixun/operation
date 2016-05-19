# 上线说明
#### 自助上线平台
* 地址 [http://183.207.194.129:9091/SelfServiceOnline/](http://183.207.194.129:9091/SelfServiceOnline/)
* 用户名密码使用老付的账号和验证码
* 使用cplatform用户连接服务器，sudo su获取root权限

#### FTP的使用
* 地址 121.40.50.28
* 本地使用工具如winscp上传下载文件
* 自助上线平台使用sftp命令连接ftp上传和下载文件，如```sftp -oPort=22 sam@121.40.50.28```
* 冲浪机房的/backup为共享目录，上传补丁包选择任意一台即可

#### 一般上线步骤
* 确认服务器IP和应用地址
* 备份现有应用，如管理后台应进入tomcat的webapps目录下面：```tar zcvf back-201605091000.tar.gz back/```
* 通过ps和kill命令杀掉现有进程，如管理后台先```ps -ef|grep 'cproduct_browser/tomcat'```，然后```kill -9 pid```
* 将通过ftp上传的补丁包拷贝到webapps目录下，然后解压，如```unzip suferDeskInteFace-201605061710.zip```
* 启动tomcat，如```../bin/startup.sh```
* 查看进程和日志确保正常后通知测试人员

#### 自动上线脚本
为了提高上线效率并减低出错几率，针对各应用均提供了相应的自动上线脚本，所有脚本均位于/backup共享目录下，对应关系如下：

| 应用 | 脚本名称 |
| -- | -- |
| 接口 | auto.sh |
| 后台 | auto-back.sh |
| 抓取 | auto-ext.sh |
| 轮询推送 | auto-push.sh |
以后台为例说明自动上线脚本步骤：
* 使用自助上线平台连接相应服务器
* 获取root权限```sudo su```
* 进入共享目录```cd /backup```
* 连接FTP```sftp -oPort=22 sam@121.40.50.28```
* 输入密码
* 查看FTP文件```ls```
* 下载补丁包文件```get ***.zip```
* 退出FTP连接```exit```
* 执行上线脚本```sh /backup/auto-back.sh ***.zip```

#### 应用部署说明
| 服务器 | 应用 | tomcat日志
| -- | -- | -- |
| 172.16.32.11 | 后台 | /usr/local/program/cproduct_browser/tomcat/logs
| 172.16.32.43 | 抓取主 | 
| 172.16.32.44 | 抓取从 |
| 172.16.31.44 | 抓取从 |
| 172.16.32.47 | 接口 |
| 172.16.32.48 | 接口 |
| 172.16.32.49 | 接口 |
| 172.16.32.50 | 接口 |
| 172.16.32.51 | 接口 |
| 172.16.32.55 | 接口 |
| 172.16.32.56 | 接口 |
| 172.16.32.57 | 接口 |
| 172.16.32.58 | 新推送代理 | |

#### Maven项目打包方法
1. 查看svn历史记录Generate ChangeLog到文件；
2. 执行TestPack.testPackAll()方法生成修改记录；
1. POM中build元素下面添加assembly配置
   ```
		<plugins>
			<plugin>
				<artifactId>maven-assembly-plugin</artifactId>
				<version>2.2.1</version>
				<configuration>
					<descriptors>
						<descriptor>zip.xml</descriptor>
					</descriptors>
				</configuration>
				<executions>
					<execution>
						<id>make-zip</id>
						<phase>package</phase>
						<goals>
							<goal>single</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
   ```
1. 添加zip.xml文件，配置包名和需要打包的文件
   ```
   <assembly
      xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.0 http://maven.apache.org/xsd/assembly-1.1.0.xsd">
      <id>201605121350</id>
      <formats>
          <format>zip</format>
      </formats>
      <fileSets>
          <fileSet>
              <directory>${project.basedir}\target\suferDeskInteFace</directory>
              <includes>
                  <include>WEB-INF/classes/interfaceSetup.properties</include>
                  <include>WEB-INF/classes/com/c_platform/suferdesk/ext/rss/CommonRssTask*.class</include>
              </includes>
              <outputDirectory>\</outputDirectory>
          </fileSet>
      </fileSets>
  </assembly> 
   ```
1. 选择pom.xml，run as Maven build执行clean package -P product,成功后会在target目录下生成相应的补丁包；
2. 如果修改涉及common，应该先从线上下载最新的common包，然后用补丁包里面的文件覆盖更新，如果修改了配置文件，也要在线上最新配置的基础上修改；
