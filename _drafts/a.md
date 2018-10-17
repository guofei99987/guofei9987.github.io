

## 其它
上面列出的是一些linux风格或git风格的命令，其中一些命令也有其它表达形式，虽然不太好记  

```bash
# 列出正在运行的容器
docker ps

# 列出所有容器（包括Exited）
docker ps -a

# 删除指定镜像（删除镜像前须先停止并删除容器）
docker rmi <image id>


# 删除指定容器
docker rm <container id>
# 删除所有容器
docker rm $(docker ps -aq)
# 停止并删除容器
docker stop $(docker ps -q) & docker rm $(docker ps -aq)

```

------------------------------------
win7版本的安装讲解  
https://blog.csdn.net/u013810234/article/details/78483055


docs.docker.com/engine/userguide
---------------------------------

## 案例
docker jekyll

挂载目录
- win7比较麻烦  
step1：先在 virtual Box 中“共享文件夹”下配置挂载  
step2：重启
step3：  
```bash
docker run --rm --label=jekyll --volume /abc:/abc -it -p 127.0.0.1:4006:4000 jekyll/jekyll bash
```
