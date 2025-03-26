## 在开始学习前，您所需要有的基础：掌握Linux基础命令
### Docker是什么？
&emsp;&emsp;Docker 是一种开源的容器化平台，用于开发、部署和运行应用程序。它通过将应用程序及其依赖（如库、配置文件等）打包到一个轻量级、可移植的容器中，实现快速部署和环境一致性。说白了docker就是一个通用的平台，可以加载不同平台上的软件，解决兼容性问题。
&emsp;&emsp;用个实际点的例子吧，比如说我在Windows系统下开发了一个软件，别人想要使用这个软件是不是要和我开发使用到的某些东西的各种版本差不多才能运行，这时候我们想要运行测试就得去配置繁琐的参数，下载各种包啊，库啊什么的，这费时费事又费力，太麻烦了。此时，一个名叫“docker”的靓仔出现了，它说把软件给我，我来帮你。不一会儿，我们就可以看到程序运行的结果了。其实不仅仅是这种情况，各种兼容性的问题（又比如Windows不能运行macOS的程序）几乎都可以用docker解决。没错！docker就是这么劲！这么强！这么霸！口也！
### 容器化技术与虚拟机技术的区别是什么？
以下内容由deepseek总结
1. **虚拟化层级**  
   - **容器**：操作系统级虚拟化，共享宿主机内核，轻量级。  
   - **虚拟机**：硬件级虚拟化，每个VM运行独立Guest OS，重量级。

2. **资源占用**  
   - **容器**：MB级镜像，启动快（秒级），资源利用率高。  
   - **虚拟机**：GB级镜像，启动慢（分钟级），资源开销大。

3. **隔离性**  
   - **容器**：进程级隔离（依赖Linux Namespace/Cgroups），安全性较弱。  
   - **虚拟机**：硬件级隔离（Hypervisor），安全性更强。

4. **适用场景**  
   - **容器**：微服务、CI/CD、无状态应用、高密度部署。  
   - **虚拟机**：传统应用、强隔离需求（如多租户）、完整OS环境。

**一句话总结**：容器轻量高效适合云原生，虚拟机隔离性强适合传统负载。

[Docker官方网站](https://www.docker.com/)&emsp;&emsp;[Docker官方镜像源网站(dockerhub)](https://hub.docker.com/)

当遇到不会使用的命令时，可以在终端输入`command_name --help`，查看命令的使用方法，这是一个程序员必须具备的基本素养。
### docker入门命令
```linux
#查看docker状态
sudo systemctl status docker
#启动docker服务
sudo systemctl start docker
#检索镜像
docker search
#拉取（下载）镜像
docker pull image_name<:version>
#查看本地docker中现有镜像
docker images
#删除镜像
docker rmi image_name<:version>
#启动容器
#运行镜像
docker run
#查看所有容器状态等简明信息
docker ps -a
#停止容器
docker stop
#启动容器
docker start container_id/container_name
#重启容器
docker restart container_id/container_name
#查看容器状态
docker stats
#查看容器日志
docker logs
#进入容器内部
docker exec
#删除容器，必须停止容器后删除/加-f强制删除
docker rm
```
### 详解docker部分命令
1. docker run
```linux
#其中-d是让容器后台运行，--name指定容器名字，-p指定端口，分号前为主机端口，后为容器端口
#需要注意的是端口号不能重复，同时容器名必须唯一
docker run -d --name <customize_name> -p host_port:container_port image_name
```
2. docker exec
```linux
#其中-it是指以交互模式进入，交互方式我们可以选/bin/bash
docker exec -it <container_name> <interaction_method>
#在内部我们可以以最原始的方式写入数据
#其中">"是覆盖全部，而">>"是在文件末尾追加
#需要注意的是有些字符需要用转义字符来写入
echo data > file_name
echo data >> file_name
```
当我们制作了一个程序或修改了一个程序之后，想要保存或者是分享到社区该怎么做呢？
```linux
#提交容器
docker commit -m "informations" <container_name>
#保存镜像
docker save <customize_name>:<customize_label>
#生成镜像
docker load -o customize_name.tar <customize_name>:<customize_label>
```
### 保存容器
```linux
#登录到您的docker账号
docker login
#为您修改的镜像命名与添加标签，标签最好为lastest，因为下载时默认是lastest，这样不用指定版本号
docker tag source_name:source_label customize_name:customize_label
#推送您的镜像到dockerhub
docker push
```
### docker存储
```linux
#批量删除容器，其中"$"是指将括号中命令的结果传递
#aq中的“a”指“all（所有）“，“q”指“quiet（只显示容器ID）”
docker rm $(docker ps -aq)
```
#### 卷是什么？
在计算机中通常指的是一种存储设备，它可以是硬盘、SSD、软盘等，用于存储数据和程序，简单点来说就是不以`/`和`./`开始的文件。
```linux
#目录挂载，即目录映射
docker -v host_directory/container_directory
#卷映射
docker -v host_volume/container_volume
```
如果要修改容器卷中的文件，可以进入主机的`/var/lib/docker/volume/volume_name`
也可以使用以下命令直接在终端中对卷操作
```linux
docker volume <command>
#如查看相应卷的信息
docker volume inspect volume_name
```
需要注意的是，当容器被删除时，主机的相应文件不会删除。

### docker网络
#### 实现容器之间互相访问
容器之间有一个内部网络，每个容器的网关均为系统中docker0的ip，docker会为每个容器分配唯一ip，所以容器之间用对方的ip和容器端口即可访问。
但是以上方法因种种原因ip可能会改变，docker0（网络名为Bridge 桥接网络）默认不支持主机域名，可以创建自定义网络，容器名即稳定域名/ip，访问时记得加上端口号。
```linux
#创建自定义网络
docker network create network_name
#启动自定义网络
docker run --network network_name
#查看容器详情
docker inspect container_name
```
## Docker初步进阶
### Docker Compose
Docker Compose是Docker官方提供的一个工具，用于定义和运行多容器的应用程序。通过一个YAML文件来配置应用的服务，然后用一条命令来创建和管理这些容器。假设您有多个容器想要启动，做一系列事情，您目前可能就会`docker run`一个一个容器，相当麻烦，Docker Compose使得您成为一个将军，可以用一条命令指挥多个士兵，它解决了手动运行多个`docker run`命令，非常方便快捷。
要使用Docker Compose，我们首先需要编写`compose.yaml`文件，编写该文件需要有一定的规范，文件内容主要有以下几个顶级元素：name（名称），services（服务），networks（网络），volumes（卷），configs（配置），secrets（密匙）。
需要注意以下几点：
1. 若有挂载卷，则需要在volumes顶级元素中声明该卷。
2. 同一个服务中环境的变量写法可以为`"="/kv`，不能混着写。
3. 服务中的depends_on可以用来指定依赖容器，即可设定当指定容器启动后再启动该容器。
4. 如果有网络，也需要在顶级元素networks中声明。
Docker Compose主要有以下命令：
```linux
#上线，创建并启动yaml文件，如果不指定file_name，则默认启动当前路径下名为compose.yaml的文件
docker compose -f yaml_file_name up -d
#下线
docker compose down
#使用yaml文件启动容器
docker compose start container1 container2 container3
#停止
docker compose start container1 container2 container3
#启动多个相同的容器，等于后面为容器个数
docker compose scale container2=3
```
当再次使用上线命令`docker compose up`时，docker会自动更新yaml文件的修改。
这里补充一条较为常用的命令`docker compose -f yaml_file_name down --rmi all -v`，其中“-f”代表“强制”，“--rmi all”代表“删除所有容器“，“-v”代表“相关的卷”。
### Dockerfile
Dockerfile是一个文本文件，用于定义一个Docker镜像的构建过程。它包含了一系列的指令，这些指令告诉Docker如何准备运行环境、安装软件、设置配置等。类似于Windows的iso文件。
我们可以从docker官网的reference中获取Dockerfile的编写方式/命令，如`FORM`指定镜像基础环境，`RUN`运行自定义命令等。
详细可以前往[Dockerfile references](https://docs.docker.com/reference/dockerfile/)查看Dockerfile的使用方式及参数配置。
下面举个简单的例子：
```dockerfile
FORM openjdk:17
LABEL author=wiki
COPY app.jar /app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app.jar"]
```
然后运行
```linux
#"-f"代表文件，"-t"代表镜像名，最后的"."代表了上下文目录即当前目录
docker build -f Dockerfile -t myjavapp:v1.0 .
```
我们可以从官网的[guides](https://docs.docker.com/guides/)中得知其他语言镜像的制作方式。
### 镜像分层存储机制
类似于git中版本控制的增量存储，但其是每一个命令分一层，同一层的东西存储到同一位置，共用存储，每个容器都有自己的读写层。容器一删除，读写层就会丢失，根据前面所学，只要挂载到本机就可以防止丢失。
### 实例应用
请跟着作者一步一步走，写完这些实例就大概掌握了docker的初步使用知识。

## 使用docker时遇到的问题：
1. permission denied. 当前用户无权限访问Docker 守护进程(/var/run/docker.sook)
首先，我们先检验当前用户是否已经被添加到docker组
```terminate
~groups
```
如果不存在，我们则把当前用户添加到docker组中
```terminate
~sudo usermod -aG docker $USER
```
然后重启系统，检查是否运行
```terminate
docker ps
```
2. 搜索镜像超时
方案一（不可行）：打开`/etc/docker/daemon.json`，添加`"dns":["8.8.8.8", "8.8.4.4"]`，权限不够的话使用sudo，然后重载配置文件`sudo systemctl daemon-reload`，最后重启服务`sudo systemctl restart docker`。
方案二：更换镜像源，打开`/etc/docker/daemon.json`，添加网上查找的docker镜像源或是自己搭建一个镜像源，添加如以下内容：
```linux
{
    "registry-mirrors": [
        "https://registry.hub.docker.com",
        "http://hub-mirror.c.163.com",
        "https://docker.mirrors.ustc.edu.cn",
        "https://registry.docker-cn.com"
    ]
}
```
然后重载配置文件`sudo systemctl daemon-reload`，最后重启服务`sudo systemctl restart docker`，使用`docker info`或直接拉取一个镜像查看是否更换成功。
方案三：不能search，却可以pull，那我们可以直接去[Docker官方镜像源网站(dockerhub)](https://hub.docker.com/)搜索对应的镜像，复制相关命令后进行拉取即可。
### 结语
如果您看完文章还有部分没有理解的，建议多看几遍，同时作者推荐观看这个视频，讲得非常好，图文并茂，简洁明了：[尚硅谷3小时速通Docker教程，雷神带练docker部署到实战！](https://www.bilibili.com/video/BV1Zn4y1X7AZ)