### docker安装 卸载

> - https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-from-a-package
> - docker命令集合: https://docs.docker.com/engine/reference/commandline/export/#description

### docker 免sudo

> ```
> sudo groupadd docker
> sudo usermod -aG docker $USER
> sudo service docker restart
> newgrp - docker
>
> ```

### dockerhub 提交仓库

> ```
> docker login
> docker logout
>
> ```

### docker 拷贝本地东西进入容器中 [或者容器内文件拷贝到主机]

> `apple js docker cp xxx container-name:path docker cp ikcest:/root/ikcest/ikcest_main.py ./`

### 命令行进入正在运行中的container

> `apple js docker exec -it name /bin/bash`

### 一次性删掉所有container

> ```
> docker stop $(docker ps -a -q)
> docker rm $(docker ps -a -q)
>
> ```

### docker容器中的 配置时区问题

> \```apple js 在dockerfile中最后添加两条(把本主机的 /etc/localtime 和 /etc/timezone 文件拷贝到 与 dockerfile 同目录中):
>
> 1. COPY localtime /etc/
> 2. COPY timezone /etc/ ```

### docker迁移目录

> `apple js https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux`

### docker仓库

> https://github.com/docker-library

### docker查看版本

> docker –version

### docker 查看更多的细节

> docker info

### docker 命令

> ```
> docker run -itd -p 127.0.0.1:8899:8888 -v /home/minus/Public/share_volume:/home/minus/share_volume ubuntu16.04:newest /bin/sh```
> docker run -itd -p 127.0.0.1:8080:8080 -p 127.0.0.1:30008:30008 -v /home/minus/Public/share_volume:/home/minus/share_volume ubuntu16.04:vte /bin/sh```
>
> ```

#### 查看容器ip信息命令

> - `docker inspect 571532934bbb | grep IPAddress`

#### 查看容器名称

> - `docker inspect -f "" aed84ee21bde`

#### 退出容器而不退出容器do（通过attach进入容器方式）

> - `Ctrl + p + q`

#### Dockerfile 配置

> - http://www.alauda.cn/2015/07/17/dockerfileinstructions/

### 容器网络

> ```
> 1. docker network connect mynet wy-ubuntu-3  - 将容器加入一个网络
> 2. docker network rm  -  删除自定义网络
> 3. 不推荐使用默认的　bridge 网卡，　--link　通信方式也已经被标记为过去式
>
> ```

### 容器-> 镜像 ->　本地文件

> 1. 容器保存为镜像
>    - `docker commit b59 minus/wy-ubuntu:v1`
> 2. 镜像导出
>    - `docker save -o wy-ubuntu.tar minus/wy-ubuntu:v1`
> 3. 镜像导入
>    - `docker load --input xxxx.tar | docker load < xxxx.tar`

### 镜像上传

> 1. 打标签（user为注册账号名）
>    - `docker tag test:latest user/test:latest`
> 2. push
>    - `docker push user/test:latest`

### 删除所有docker 已停止的容器

> 1. `docker rm `docker ps -a |awk '{print $1}' | grep [0-9a-z]`

#### docker 测试命令

> ```
> * remote - mongo : mongo5:30003
> * remote - elasticsearch: mongo5:30004/30006
> * local - vte_api port : 8080
> * local - vte port : 30008
> * ```curl -v 'http://127.0.0.1:8080/vte/v1/systemsetting/user/login?username=1000215&password=111111\'```
>
> ```

#### docker - percona

> - https://www.percona.com/blog/2014/05/26/installing-three-node-percona-xtradb-cluster-5-6-docker/

#### docker更新　最新版本的二进制

> - https://docs.docker.com/engine/installation/binaries/

### docker 基本命令

> ```
> 1. docker container ls
> 2. docker container stop xxx
> 3. docker image ls
> 4. docker image rm [-f] xxx
>
> ```

### docker run 参数

> `apple js --rm : 在Docker容器退出时，默认容器内部的文件系统仍然被保留，以方便调试并保留用户数据;设置--rm选项,在容器退出时就能够自动清理容器内部的文件系统; -d : 后台运行容器 -i : 以交互模式运行容器,通常与-t同时使用 -t : 为容器重新分配一个伪输入终端,通常与-i同时使用 --name: 为容器指定一个名称 -e username="minus": 设置环境变量 --env-file=[]: 从指定文件读入环境变量; --expose=[]: 开放一个端口或一组端口 -m : 设置容器使用内容最大值 --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；`

## Swarm

### Docke-engine - Swarm 内置版本（超级简单好用）

> 1. (官方教程)[https://docs.docker.com/engine/swarm/swarm-tutorial/inspect-service/]
> 2. 常用命令 ```
>    - docker初始化 `docker swarm init --force-new-cluster --advertise-addr=`
>    - 节点离开集群(节点端) `docker swarm leave`
>    - 节点离开集群(manage端) `docker node rm --force <node_name / id>`
>    - swarm 节点更新 `docker swarm update`
>    - swarm 中部署服务 `docker service create --replicas 1 --name helloworld alpine ping docker.com`
>    - node运行集群container `docker exec -it helloworld.1.99sac60en06h5yjusq1f7ers4 /bin/sh`
>    - 查询swarm中服务信息 `docker service inspect --pretty helloworld`
>    - 查看节点部署情况 `docker service ps <service-name>`
>    - swarm 中动态扩展服务 `docker service scale helloworld=5`
>    - swarm　中删除服务 `docker service rm helloworld` ```
> 3. Etcd安装(采用 upstart 来启动)
>    1. etcd创建集群博客[http://blog.scottlowe.org/2015/04/15/running-etcd-20-cluster/]　
>       - 注意　ETCD_ADVERTISE_CLIENT_URLS　和　ETCD_LISTEN_CLIENT_URLS　都设置成　’127.0.0.1’

=============================================================================

### docker进入到容器命令[pg容器为例]

> \``` docker-machine Git-Repository: https://github.com/docker/machine/releases 安装 docker-machine: https://docs.docker.com/machine/install-machine/ -t : 让docker分配一个伪终端并绑定到容器的标准输入上 -i : 让容器的标准输入保持打开 docker账号: miminus lianxiang docker login : 登陆 Docker public registry,　用于登陆 docker run -idt –name pg9.6 -e POSTGRES_PASSWORD=lianxiang -d pg_9.6 docker exec -it name /bin/bash docker start container-name - 启动未启动的docker容器 docker stop container-name - 关闭正在运行的docker容器 docker tag image-name username/repository:tag - 给已有的镜像打标签,以备上传
>
> docker push miminus/get-started:web - 本地docker上传到　DockerHub(先登陆 docker login)

> 显示所有容器(包括未运行): docker ps -a 只显示在运行中的容器: docker ps 删除未运行容器: docker rm 删除镜像: docker rmi repository:tag ```

### 镜像集

> 1. postgresql [https://github.com/docker-library/postgres/tree/bef8f02d1fe2bb4547280ba609f19abd20230180] ``` /usr/share/postgresql/9.6 /usr/lib/postgresql/9.6/bin 配置文件：/var/lib/postgresql/data
>
> docker run -idt –name pg9.6 -e POSTGRES_PASSWORD=lianxiang -d pg_9.6 docker exec -it container-name /bin/bash docker start container-name docker run -d -it –name devtest –mount source=myvol2,target=/app nginx:latest ```

### docker Swarm

> ```
> docker swarm init [--advertise-addr 192.168.1.9] 多个ip情况下　- docker 初始化
> docker stack deploy -c docker-compose.yml getstartedlab<new-name>
> docker service ls               - get service-id for the one service
> docker container ls -q          - list all 5 containers-ids
> docker service ps <service>     - 查看状态和它们的IDs
> docker stack ps getstartedlab   - 查看各个容器运行状况
> docker stack rm getstartedlab   - take down the app but not swarm [one-node swarm is still up and running]
> docker node ls                  - 查看节点情况
> docker swarm leave --force      - take down the swarm
>
> ```

### docker-machine

> ```
> docker-machine create --driver virtualbox myvm1             # 创建一个 virtualbox 的虚拟机
> docker-machine ls                                           # 获取 vm 信息及其 ips
> docker-machine ssh myvm1                                    # ssh进入到 虚拟机终端
> docker-machine ssh myvm1 "docker stack rm getstartedlab"    # 关闭服务器上的服务
> docker-machine ssh myvm2 "docker swarm leave"               # run on worker
> docker-machine ssh myvm1 "docker swarm leave --force"       # run on manager
> docker-machine env 和 docker-machine ssh
> docker-machine scp <file> <machine>:~
>
> 删除虚拟机docker-machine
>
> 1. docker-machine stop myvm1
> 2. docker-machine rm myvm1
> 3. docker-machine start myvm1   # 启动关闭的machine
>
> ```

### docker-network

> ```
> docker network ls                               # 查看网络
> docker network disconnect bridge networktest    # 将一个容器取消绑定一个网络
> docker network create -d bridge my_bridge       # 创建一个桥接网络
> docker network inspect my_bridge                # 查看一个网络的情况（包括IP子网范围）
> docker run -d --net=my_bridge --name db training/postgres   # 将容器启动添加入指定网络
> docker inspect --format='' db<container-name>      # 查看某个容器的所在网络
> docker inspect --format='' web  # 获取某个容器的IP
>
> * 一个容器可以绑定到多个网络中，不同网络中的容器之间不能通信的，
> docker network connect my_bridge web            # 将正在运行的容器绑定到新的网络中
>
> ```

### docker volume[https://docs.docker.com/engine/admin/volumes/volumes/#start-a-container-with-a-volume]

> ```
> docker volume create my-vol
> docker volume ls
> docker volume rm my-vol
>
> ```

### Dockerfile

> ```
> reference: https://docs.docker.com/engine/reference/builder/#run
>
> .dockerignore 用于排除不用的文件或者文件夹
> 将应用解耦到多个容器中，有利于水平扩展和容器复用　－　“one process per container”
>
> docker build -t xxx/xxx path
>
> ```
>
> - 命令 ``` FROM: # 第一条指令必须为FROM指令，如果在一个Dockerfile中创建多个镜像时，可以使用多个FROM指令。 FROM xxx/xxx
>
> MAINTAINER: # 指定维护者信息。 MAINTAINER name@email.com
>
> USER # USER 指令和 WORKDIR 相似，都是改变环境状态并影响以后的层
>
> - WORKDIR 是改变工作目录，USER 则是改变之后层的执行 RUN, CMD 以及 ENTRYPOINT 这类命令的身份
>
> ARG XXX=latest # 定义常量　使用方法 ${XXX}：　构建参数和 ENV 的效果一样，都是设置环境变量。所不同的是，ARG 所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的
>
> ENV # 指定环境变量 ENV NAME World
>
> WORKDIR # 指定工作目录　指定工作目录（或者称为当前目录），以后各层的当前目录就被改为指定的目录，如该目录不存在，WORKDIR 会帮你建立目录
>
> ADD # 复制指定的到容器中的。可以为相对路径，URL或者tar文件均可。　- 自动解压缩文件 ADD . /APP # HOST主机当前目录下文件加入并解压到容器中 /APP的目录下
>
> - 最好的使用是用来拷贝一个压缩文件进去并进行解压
>
> COPY # 复制本地主机的到容器中的。 源文件的元数据会保留，如读／写／权限／时间等
>
> VOLUME # 创建一个可以从本地主机或其他容器挂载的挂载点，一般用来存放数据库和需要保持的数据等。 VOLUME [“/data”, “”, …] VOLUME
>
> RUN # 每一个 RUN 都是启动一个容器、执行命令、然后提交存储层文件变更 RUN apt-get update && apt-get install -y \ bzr \ cvs \ git \ mercurial \ subversion RUN [‘executable’, ‘param1’, ‘param2’]
>
> ENTRYPOINT # 当指定了 ENTRYPOINT 后，CMD 的含义就发生了改变，不再是直接的运行其命令，而是将 CMD 的内容作为参数传给 ENTRYPOINT 指令 ENTRYPOINT [ “curl”, “-s”, “http://ip.cn” ] ENTRYPOINT [“docker-entrypoint.sh”] EXPOSE # 指定Docker容器开放的端口号 EXPOSE 80 ```