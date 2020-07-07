- Docker学习

## Docker学习

###### 搜索星星>25的centos镜像
```text
docker search -f stars=25 centos
```

###### 列出镜像
> 列表包含了 仓库名、标签、镜像 ID、创建时间 以及 所占用的空间。
```text
docker image ls 

根据仓库名列出镜像
docker image ls mysql
```

###### 搜索星星>25的centos镜像
```text
docker run -it centos bash
```
