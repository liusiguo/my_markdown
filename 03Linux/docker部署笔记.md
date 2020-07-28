# Docker部署

### docker部署springboot项目

##### 预备工作

- linux 安装docker

- linux 安装java环境(JDK8),gradle安装依赖java环境

  ~~~markdown
  - 傻瓜般安装方式
  		yum install -y java-1.8.0-openjdk-devel.x86_64
  ~~~

- linux 安装gradle

  ~~~markdown
  - 项目打包指令
  		gradle clean build -x test
  ~~~

##### 步骤一：准备Dockerfile

~~~dockerfile
FROM centos:7               
VOLUME /tmp   
RUN ln -fs /data/bms_service/bms_service/build/libs/bms_service-2.0.0.jar  bms_service.jar
RUN ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN yum install -y java-1.8.0-openjdk-devel.x86_64
ADD bms_service-2.0.0.jar bms_service.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/bms_service.jar"]
~~~

- 说明

  ~~~ markdown
  - FROM centos:7  
  		基础镜像，因为要部署java项目，需要java环境，因此可选择自定义的带有jdk的系统镜像，这里使用纯净版centos7
  - VOLUME /tmp 
  		自动挂载匿名存储卷，一般启动时设置-v参数，因此Dockerfile的该指定一般用不到
  - ADD bms_service-2.0.0.jar bms_service.jar  
  		将打包好的jar包拷贝至docker容器下
  - ln -fs /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  
  		软连接挂载时间设置,-f表示强制执行 -s软链接
  - RUN yum install -y java-1.8.0-openjdk-devel.x86_64  
  		因centos7镜像未安装jdk8，因此这里在容器内安装jdk8，如果使用的基础系统镜像含有jdk8则此命令可省去
  - ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/bms_service.jar"]
  		设置容器启动时执行jar包命令
  ~~~

##### 步骤二：构建容器镜像

~~~markdown
- 进入springboot项目根目录，执行
		docker build -t bms_service:v1 .
~~~

##### 步骤三：启动容器

~~~shell
docker run -d --privileged=true -v /data/logs:/app/logs -p 8080:7080 --name bms_service  bms_service:v1
~~~

- 说明

  ~~~markdown
  - -d 
  		以后台的方式启动容器
  - --privileged=true 
  		以特权方式启动容器，目的是防止容器内日志挂载出来时出现权限拒绝，也可在linux中永久设置，设置方法为改
  		/etc/sysconfig/selinux文件，将将SELINUX的值设置为disabled。
  - -v /data/logs:/app/logs
  		挂载共享存储卷，将容器内的日志挂载出来，/data/logs为linux服务器目录（容器外），/app/logs为容器内项目生成的日志目		录。
  - -p 8080:7080 
  		容器暴露端口，容器内项目端口为7080，容器对外暴露端口为8080。
  - --name bms_service 
  		指定容器名称。
  - bms_service:v1
  		镜像名称:版本号。
  ~~~

##### 步骤四：查看容器是否启动

~~~markdown
docker ps | grep bms_service
~~~

----

##### 说明

~~~markdown
	上图中的bms_service为我要部署的java项目，具体部署时请参考自己项目自行修改
~~~

