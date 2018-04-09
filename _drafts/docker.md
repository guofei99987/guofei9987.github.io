Docker三个基本概念  
1. Image：一组文件，分层构建
2. Container：是Image的一个实例，有自己的root/网络/进程，和 Image 的关系如同类和对象的关系
3. Repository：



一个 Docker Registry 中包含多个 Repository，每个 Repository 包含多个 Tag ，每个 Tag 包含一个镜像。  


列出已经安装的镜像
```
docker image ls # 显示顶层镜像
docker image ls -a #显示包括中间层在内的所有镜像
```

删除已经安装的镜像
```
docker image rm [镜像]
```


使用 nginx 这个镜像，启动一个 Contrainer 命名为 webserver ，并且映射了80端口  
```
docker run --name webserver -d -p 80:80 nginx
```



```
docker commit [选项] [容器名] # 将容器的存储层保存为镜像。慎用，因为很多文件被添加进来，导致镜像极为臃肿
docker history [容器名] # 查看镜像的历史记录

```

## 定制镜像：Dockerfile
在空目录中建立一个文本文件，命名为`Dockerfile`
```
FROM nginx
RUN ...
```
- FROM nginx 指定基础镜像，可以在docker网站上下载
- RUN
    - RUN一个shell
    - RUN一个exec `RUN [可执行文件,参数1,参数2]`


每个RUN会建立一层，所以注意尽量用一个RUN，并且用&&把多个SHELL命令串联起来
