# server test

服务端测试

## Docker

Docker，起源于 2013，开源的应用容器引擎，基于 Go
- Docker 与虚拟机的区别
    - 虚拟机，应用与应用之间是完全的资源隔离；使用完全独立的内核
        - Server->Host OS->Hypervisor->(Guest OS->Bins/Libs->App A | Guest OS->Bins/Libs->App B)
    - Docker，容器与容器之间只是进程的隔离；使用宿主操作系统的内核
        - Server->Host OS->Docker Engine->(Bins/Libs->App A | Bins/Libs->App B)
- 组成
    - Client：docker build|pull|run
    - Docker_Host：Docker daemon、Containers、Images
    - Registry
- 概念
    - Docker 镜像（Docker Images），每一个镜像都可能依赖一个或多个下层的镜像组成的另一个镜像，AUFS 文件系统
    - Docker 仓库（Docker Registry），集中存放镜像的地方
    - Docker 容器（Docker Containers），镜像运行后的进程
- 安装
	- Ubuntu
		- 安装依赖：apt-get -y install apt-transport-https ca-certificates curl software-properties-common
		- 安装证书: curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
		- 添加源: add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release-cs) stable"
		- 安装 Docker: apt -y install docker-ce，社区版本
	- CentOs
		- 安装依赖：yum install -y yum-utils device-mapper-persistent-data lvm2
		- 添加源：yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/centos/docker-ce.repo
		- 安装 Docker：yum -y install docker-ce
		- 启动服务：systemctl start docker，ubuntu默认启动服务
- 配置
	- cd /etc/docker，添加 daemon.json 配置国内镜像源
		- `{"registry-mirrors":["https://registry.docker-cn.com","http://hub-mirror.c.163.com","https://docker.mirrors.ustc.edu.cn"]}`
- 常用命令
	- 镜像管理
		- 查看所有镜像：docker images
		- 搜索镜像：docker search $name
		- 拉取下载：docker pull $name:latest
		- 导出：docker save $name > $name.tar
		- 导入：docker load < $name.tar
		- 删除：docker rmi $name;latest，有容器在使用时需要先删除容器 docker rm -f 9a80d6685b89
		- 更改镜像名：docker tag $name:latest $name:test
		- 查看镜像创建历史：docker history $name
	- 容器管理，镜像运行之后就变成了容器
		- 运行容器：docker run -d --name=$name $name:last ping 114.114.114.114，ping 114.114.114.114可以删除，d 后台运行
		- 查看运行的容器：docker ps，docker ps -a，-a 可以查看所有运行过的容器
		- 查看容器中运行的进程：docker top $name
		- 查看资源占用：docker stats NAME，这里的 NAME 可以是 CONTAINER ID
		- 容器：docker start/restart/stop/kill $name
		- 暂停容器：docker pause/unpause $name
		- 强制删除容器：docker rm -f $name
		- 执行命令：docker exec -it $name ls /bin/ grep sh，查看容器支持的 sh
		- 进入到容器：docker exec -it $name /bin/bash，进入到 bash 中
		- 复制文件：`docker cp $name:/etc/hosts .`，复制容器中的 /etc/hosts 到当前目录
		- 查看容器日志：docker logs -f $name
		- 查看容器/镜像的元信息：docker inspect $name
			- 格式化输出：`docker inspect -f '{{.Id}}' $name`
			- Inspect 语法
		- 查看容器内文件结构：docker diff $name
		- 容器网络
			- 创建容器网络：docker network create testlink，创建名称为 testlink 的容器网络
			- 查看创建的所有容器网络：docker network ls

### registry

- registry，[地址](https://hub.docker.com/_/registry)
	- Docker registry 是存储 Docker image 的仓库，运行 push、pull、search 时，通过 Docker daemon 与 Docker registry 通信
		- 有时候使用 Docker Hub 这样的公共仓库可能不方便，就可以通过 registry 创建一个本地仓库
	- 运行：`docker run -d -p 5000:5000 -v ${PWD}/registry:/var/lib/registry --restart always --name registry registry:2`
		- 配置本地仓库 daemon.json：`{"registry-mirrors": ["192.168.0.128:5000"]}`
		- 打标签：docker tag nginx:1.17.9 192.168.0.128:5000/nginx:1.17.9
		- 上传：docker push 192.168.0.128:5000/nginx:1.17.9
		- 下载：docker pull 192.168.0.128:5000/nginx:1.17.9
	- 步骤：
		- docker pull registry:2
		- docker run -d -p 5000:5000 -v /usr/local/registry:/var/lib/registry --restart always --name registry registry:2
		- docker pull busybox
		- docker tag busybox localhost:5000/busybox:v1.0，localhost:5000表示镜像仓库的地址
			- 如果镜像打包的时候就按照`仓库地址/镜像名:版本号`的方式命名，就不需要重新打标记
				- 如 docker build -t localhost:5000/busybox:v1.0
		- docker push localhost:5000/busybox:v1.0
		- curl http://localhost:5000/v2/_catalog，查看仓库里的镜像
		

### 搭建 Web 服务器

- Nginx
	- 拉取：`docker pull nginx:1.17.9`
	- 运行：`docker run -d --name nginx -p 8088:80 nginx:1.17.9`
		- 如果运行时发生警告 WARNING:IPv4，需要修改配置文件 vim /usr/lib/sysctl.d/00-system.conf
			- 追加 net.ipv4.ip_forward=1
			- 重启网络 systemctl restart network
		- 如果是在 windows 虚拟机上运行的 Linux，打开网页输入的网址就不是 127.0.0.1，而是 Linux 的 ip 地址
	- 挂在目录：`docker run -d --name nginx1 -p 8089:80 -v ${PWD}/html:/usr/share/nginx/html nginx:1.17.9`
		- 把本地当前文件 ${PWD}/html，隐射到容器里，用于持久化，添加 index.html 就可以看到结果

### 搭建测试用例管理平台

- Testlink
	- 容器网络：docker network create testlink
	- 运行数据库：`docker run -d --name mariadb -e MARIADB_ROOT_PASSWORD=mariadb -e MARIADB_USER=bn_testlink -e MARIADB_PASSWORD=bn_testlink`
		- `-e MARIADB_DATABASE=bitnami_testlink --net testlink -v ${PWD}/mariadb:/bitnami bitnami/mariadb:10.3.22`
			- bitnami/mariadb:10.3.22 第三方定制的 mariadb 数据库
	- 运行 Testlink
		- `docker run -d -p 80:80 -p 443:443 --name testlink -e TESTLINK_DATABASE_USER=bn_testlink -e TESTLINK_DATABASE_PASSWORD=bn_testlink`
		- `-e TESTLINK_DATABASE_NAME=bitnami_testlink --net testlink -v ${PWD}/testlink:/bitnami bitnami/testlink:1.9.20`
		- 默认用户名 user，默认密码 bitnami，不需要先 pull，这个命令会自动 pull
		
### Jenkins

- Jenkins
	- Docker hub: [Jenkins](https://hub.docker.com/r/jenkins/jenkins/)
	- 运行：docker run -d --name=jenkins -p 8080:8080 jenkins/jenkins
	- 查看默认密码：docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
	- 挂载目录：chmod 777 jenkins
		- 运行：`docker run --name jenkins -d -p 8080:8080 -p 50000:50000 -v ${PWD}/jenkins:/var/jenkins_home jenkins/jenkins`

### Dockerfile

- Dockerfile 是由一系列指令和参数构成的脚本，一个 Dockerfile 里面包含了构建整个镜像的完整命令。通过 docker build 执行 Dockerfile 中的一系列指令自动构建镜像
	- FROM：基础镜像，FROM 命令必须是 Dockerfile 的首个命令
	- LABEL：为镜像生成元数据标签信息
	- USER：指定运行容器时的用户名或 UID，后续 RUN 也会使用指定用户
	- RUN：RUN 命令时 Dockerfile 指定命令的核心部分。它接受命令作为参数并用于创建镜像，每条 RUN 命令在当前基础镜像上执行，并且会提交一个和新镜像层
	- WORKDIR：设置 CMD 指明的命令的运行目录，为后续的 RUN CMD ENTRYPOINT ADD 指令配置工作目录
	- ENV：容器启动的环境变量
	- ARG：构建环境的环境变量
	- COPY：复制文件
	- CMD：容器运行时执行的默认命令
	- ENTRYPOINT：指定容器的“入口”
	- HEALTHCHECK：容器健康状态检查
		
### Docker-compose

Docker-compose
- 用于定义和运行多容器的 Docker 应用程序的工具，可以使用 YAML 文件来配置应用程序的服务
- 三个步骤：
	- 1.使用 Dockerfile 定义应用程序的环境，以便可以在任何地方复制它
	- 2.在 docker-compose.yml 中定义组成应用程序的服务，以便它们可以在隔离的环境中一起运行
	- 3.运行 docker-compose up，然后 Compose 启动并运行整个应用程序
- 安装
	- macOS、Windows 使用的 Docker Desktop 默认已经安装
	- Linux，[地址](https://github.com/docker/compose/releases)
		- `curl "https://github.com/docker/compose/releases/download/1.28.6/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`
		- 拷贝到可执行的地方：cp docker-compose /usr/local/bin/docker-compose
		- 更改权限：chmod +x /usr/local/bin/docker-compose
		- 查看版本：docker-compose version
- 常用命令
	- 查看配置：docker-compose config
	- 后台启动：docker-compose up -d
	- 构建镜像：docker-compose build
	- 下载镜像：docker-compose pull
	- 运行的：docker-compose ps
	- 进程：docker-compose top
	- 启动：docker-compose start
	- 停止：docker-compose stop
	
### 镜像构建

- 使用 Docker commit 构建
	- Docker commit 一般用作从一个运行状态的容器来创建一个新的镜像。定制镜像应该使用 Dockerfile 来完成
		- 默认 commit 镜像，对外不可解释，不方便排查问题，可维护性茶
		- docker commit 容器名 新镜像名:tag
- 使用 Dockerfile、Docker build 构建，
	- Docker build
		- 忽略文件：.dockerignore
		- 指定文件：docker build -f
		- 添加标签：docker build -t
		- 不使用缓存：docker build --no-cache
		- 构建时变量：docker build --build-arg
			- ARG 指定变量
		- 构建：docker build -t app:v1 -f Dockerfile .，构建到当前目录






	
	
	
	
	
	
	
	
	
	
	
	
	
	
	

## 持续集成

- 持续集成：频繁地（一天多次）将代码集成到主干
- CI（持续集成 Continuous Integration）：Develop->Build->Test
- CD（持续交付 Continuous Delivery）：Develop->Build->Test->Release
- CD（持续部署 Continuous Deployment）：Develop->Build->Test->Release->Deploy to production

### Jenkins

- docker部署
    - 1.拉取docker：docker pull jenkins/jenkins:lts
    - 2.创建docker地文件影射卷：docker volume inspect jenkins_home
    - 3.创建实例：docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home
        - -e JAVA_OPTS=-Duser:timezone=Asia/Shanghai jenkins/jenkins:lts
    - 4.获取初始管理员密码：docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
- slave 节点连接方式
    - 8080端口用于jenkins服务器的对外UI地址，50000端口用于slave节点与jenkins的通讯端口
    - wget http://docker.test-studio.com:8080/jnlpJars/agent.jar
    - java -jar agent.jar -jnlpUrl http://docker.test-studio.com:8080/computer/demo/slave-agent.jnlp
        - -secret e74f690d1bcd42749110c0087645b606ef0f7395 -workDir "/tmp/jenkins/"
- 无界面运行
    - `using_headless=os.environ["using_headless"]`















