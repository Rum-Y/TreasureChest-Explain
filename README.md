# TreasureChest Explain
## 声明
该文件记录有关TreasureChest项目的说明及相关实操记录。
该项目处于持续开发，优化阶段，并且将利用空闲时间长期进行新功能添加，以及优化已实现的功能。所以暂时不打算公开自认为不太成熟的源码，以免误导他人。
## 项目简介
由于好奇心和兴趣驱使，想利用空闲时间研究并实现一些生活中可能会用到的计算机相关功能，这些功能有些用于提高工作效率，有些就纯纯是根据个人兴趣爱好去实现的。为了将众多功能汇聚于一体，并且只需要打开浏览器就能访问而无需单独下载客户端应用程序，所以有了该web项目的诞生。这是一个以前后端分离模式开发的项目。
## 开发环境
- JDK 17.0.8 LTS
- Mysql 8.1.0 (直接拉取最新版本的docker镜像运行生成容器的mysql版本)
- Redis 7.2.1 (同mysql)
- Maven 3.9.6
- Node 16.20.2
- 后端: Springboot、SpringSecurity、Mybatis、JWT
- 前端: Vue3、Element Plus、Axios、Vue Router
- 开发工具: IDEA、VS Code
## 部署环境
### 系统环境
- CNware 虚拟化云平台
- Linux (CentOS 7)
- Docker 24.0.6
### 容器相关
- 基于MySql官方镜像生成的Mysql容器
- 基于Redis官方镜像生成的Redis容器
- 通过Dockerfile构建的以OpenJDK作为基础镜像构建的后端服务容器，将后端项目部署在其中生成容器。
- 直接以官方Nginx镜像作为支撑，将前端项目打包好的dist文件夹部署在其中生成容器。
## 演示视频地址

## 演示图
## 部署相关
### 记录一下会用到的docker部署命令
``` shell
docker search <镜像名>          # 搜索要拉取的镜像

docker pull <镜像名>            # 拉取镜像到本地

docker images                  # 查看已经拉取到本地的镜像

docker run -d --name <自定义容器名> -p <端口号>:<映射端口号> <镜像名>            # 运行镜像生成容器

docker start <容器名/容器ID>    # 启动容器

docker stop <容器名/容器ID>     # 停止容器

docker rm <容器名/容器ID>       # 移除容器

docker ps                      # 查看正在运行的容器

docker ps -a                   # 查看所有容器，包含已经停止运行的容器

docker cp <源路径> <目的路径>   # 可在容器和linux文件夹之间转移文件

docker build -t <自定义镜像名> .      # 在Dockerfile文件所在目录处执行构建镜像

docker exec -it <容器名> /bin/bash         # 进入正在运行的容器

docker logs <容器名>            # 查看容器日志
```
### 记录一下后端项目部署步骤
1. 点击IDEA右侧的Maven选项卡下的Lifecycle下的package一键打包
2. 在项目target目录下找到打包好的xxx.jar
3. 把jar上传到Linux服务器
4. 在jar包所在目录编写Dockerfile文件，大致内容就是以openjdk为基础镜像构建需要的镜像，暴露***8080***端口
5. 运行docker构建镜像命令构建出***spring-app-container***容器
6. 从浏览器部署访问接口能够得到响应数据表示部署成功
### 记录一下前端项目部署步骤
1. 使用***npm run build***命令将前端项目打包得到一个dist文件夹
2. 在项目目录中找到dist文件夹，上传到Linux服务器
3. 这里有两种方法可以制作镜像，一种是编写Dockerfile文件的方式，一种是直接使用官方镜像。
4. 该项目用后一种方式，执行命令在生成容器的同时将dist文件夹和nginx.conf文件上传至容器中的指定目录
5. 上一步操作命令示例如下
```
docker run -d --name frontend-container -v /path/to/dist:/usr/share/nginx/html -v /path/to/nginx.conf:/etc/nginx/nginx.conf -p 80:80 nginx
```
6. 也可以先编写好nginx.conf配置文件（包含反向代理等配置），等执行命令生成容器后再用docker cp命令将该文件传至容器中指定位置
