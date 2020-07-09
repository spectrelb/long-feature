- Docker学习

## Docker-mysql

![docker安装Mysql+图形化管理界面连接](https://www.jianshu.com/p/6d258e354766)

###### 下载镜像
```text
docker pull mysql:5.7
```

###### 在用户目录下新建以下目录，文件。
```text
mkdir /Applications/www/docker/data/mysql
mkdir /Applications/www/docker/data/mysql/config
mkdir /Applications/www/docker/data/mysql/data

在config目录下，创建my.cnf文件，内容如下所示：

[mysqld]
user=root
character-set-server=utf8
default_authentication_plugin=mysql_native_password
[client]
default-character-set=utf8
[mysql]
default-character-set=utf8
```

###### 本地需要挂在的文件目录和数据库配置文件都创建好了,下面开始创建容器
```text
docker run -d -p 3306:3306 --restart=always --name mysql -e MYSQL_ROOT_PASSWORD=123456 -v=/Applications/www/docker/data/mysql/config/my.cnf:/etc/mysql/my.cnf -v=/Applications/www/docker/data/mysql/data:/var/lib/mysql mysql:5.7

docker ps -a

参数详解：
-d创建守护式容器，创建完成后，容器会在后台运行
-p设置宿主机和容器端口的映射
--restart=always设置容器开机自启动
--name设置容器名称
-e设置环境变量
-v设置挂载本地目录，文件路径为绝对路径
命令执行完毕，mysql容器也创建好了

直接用图形化界面连接

Host:127.0.0.1
username:root
passowrd:123456
port：3306
```

