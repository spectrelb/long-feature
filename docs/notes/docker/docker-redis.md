- Docker学习

## Docker-mysql

[dockerhub-redis](https://hub.docker.com/_/redis)

###### 在用户目录下新建以下目录，文件。官方下载[redis.conf](http://download.redis.io/redis-stable/redis.conf)：

```text
mkdir /Applications/www/docker/data/redis
mkdir /Applications/www/docker/data/redis/conf

把redis.conf放到conf目录下
修改启动默认配置(从上至下依次)：

bind 127.0.0.1 #注释掉这部分，这是限制redis只能本地访问

protected-mode no #默认yes，开启保护模式，限制为本地访问

daemonize no#默认no，改为yes意为以守护进程方式启动，可后台运行，除非kill进程，改为yes会使配置文件方式启动redis失败

databases 16 #数据库个数（可选），我修改了这个只是查看是否生效。。

dir  ./ #输入本地redis数据库存放文件夹（可选）

appendonly yes #redis持久化（可选）

requirepass  密码 #配置redis访问密码

```

###### 下载镜像
```text
docker pull redis

docker run -p 6379:6379 --name redis -v /Applications/www/docker/data/redis/conf/redis.conf:/etc/redis/redis.conf -v /Applications/www/docker/data/redis/data:/data -d redis redis-server /etc/redis/redis.conf  --appendonly yes 


docker logs -f -t --tail 100 redis  查看日志

docker ps -a

-d                                                  -> 以守护进程的方式启动容器
-p 6379:6379                                        -> 绑定宿主机端口
--name myredis                                      -> 指定容器名称
--restart always                                    -> 开机启动
--privileged=true                                   -> 提升容器内权限
-v /root/docker/redis/conf:/etc/redis/redis.conf    -> 映射配置文件
-v /root/docker/redis/data:/data                    -> 映射数据目录
--appendonly yes                                    -> 开启数据持久化

```


docker run -p 6379:6379 --name redis -v /Applications/www/docker/data/redis/conf/redis.conf:/etc/redis/redis.conf -v /Applications/www/docker/data/redis/data:/data -d redis redis-server --requirepass "123456"
