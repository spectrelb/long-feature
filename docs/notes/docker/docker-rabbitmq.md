- Docker学习

## Docker-mysql

[dockerhub-rabbitmq](https://hub.docker.com/_/rabbitmq)

###### 下载镜像
```text
docker pull rabbitmq:3.8.3-management 拉取3.8.3-management版本（带有web页面的）的rabbitMq镜像

docker run -d --name rabbitmq -e RABBITMQ_DEFAULT_USER="mq_jjb" -e RABBITMQ_DEFAULT_PASS="pwJJB@%*258" -e RABBITMQ_DEFAULT_VHOST="my_vhost" -p 15672:15672 -p 5672:5672 rabbitmq:3.8.3-management

docker ps -a
```
