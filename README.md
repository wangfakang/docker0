# docker0
docker Use guide 
`` docker的简单使用：
``


 #  docker相关的使用:


一.docker的安装(ubuntu 14.04版本)    
========  
使用apt安装:    
sudo apt-get install docker  (1.0.1)      





二.简单的使用    
=======    

部分命令的使用

sudo docker pull dl.dockerpool.com:5000/ubuntu:12.04  进行获取相应的镜像    
sudo docker images                                    列出本地的docker镜像    
sudo docker run -t -i ubuntu:12.04 /bin/bash          使用docker运行一个容器    
sudo docker rmi training/sinatra                      删除镜像    


docker commit 24dd5b15abf2  docker/ubuntu 24dd5b15abf2  将一个container固化为一个新的image    
docker push   id    
docker pull id    
docker search                                         搜索镜像

docker pull [image]  
docker run [image] [cmd]                               从指定image里生成一个container并在其中运行一个命令
docker run -i -t [imag] [cmd]                          在container里运行交互式命令，比如shell
docker run -d [image] [cmd]                            在container里运行后台任务

docker start/stop/restart <container>                  开启/停止/重启container   
docker start -i <container>                            启动一个container并进入交互模式   
docker attach <container>                              attach一个运行中的container      
docker run <image> <command>                           使用image创建container并执行相应命令，然后停止    
docker run -i -t <image> /bin/bash                     使用image创建container并进入交互模式, login shell是/bin/bash    
docker run -i -t -p <host_port:contain_port>           将container的端口映射到宿主机的端口    
docker commit <container> [repo:tag]                   将一个container固化为一个新的image，后面的repo:tag可选     
docker build <path>                                    寻找path路径下名为的Dockerfile的配置文件，使用此配置生成新的image    
docker build -t repo[:tag]                             同上，可以指定repo和可选的tag     
docker build - < <dockerfile>                          使用指定的dockerfile配置文件，docker以stdin方式获取内容，使用此配置生成新的image    
docker port <container> <container port>               查看本地哪个端口映射到container的指定端口，其实用docker ps 也可以看到      

docker images   查看镜像
docker save    busybox-1 > /home/save.tar              主要是用来进行保存进行的    

docker ps -aq   查看容器
docker export <CONTAINER ID> > /home/export.tar        是用来导出容器的    


以上两个命令的区别是比较大的,其中save的结果会比export的结果大,save是进行image的保存,而export是对    
容器的保存,而且save会保存整个进行的依赖关系,但是export不会的只是保存最终的提交所以也比较小一点.    

docker rm $(docker ps -q -a) 一次性删除所有的容器   
docker rmi $(docker images -q) 一次性删除所有的镜像。    

导入容器:    
cat /home/export.tar | sudo docker import -busybox-1 -export:latest    
加载镜像:     
docker load < /home/save.tar


dockerfile的编写:
===========

example one:
@pull down centos image
@FROM <image>:<tag>
FROM centos                 #表示基础镜像来源
MAINTAINER myu myu@live.cn  #MAINTAINER命令用来指定维护者的姓名和联系方式
 
@copy jdk and tomcat into image
ADD ./apache-tomcat-7.0.52.tar.gz /root    #从src复制文件到container的dest路径:
ADD ./jdk-7u51-linux-x64.tar.gz /root
 
@set environment variable
ENV JAVA_HOME /root/jdk1.7.0_51  #用于设置环境变量
ENV PATH $JAVA_HOME/bin:$PATH    #用于设置环境变量
 
@define entry point which will be run first when the container starts up
ENTRYPOINT /root/apache-tomcat-7.0.52/bin/startup.sh && tail -F /root/apacher-tomcat-7.0.52/logs/catalina.out

example two:
FROM ubuntu:test
ENTRYPOINT echo "Welcome!"

利用dockerfile进行创建image:
docker build -t ubuntu:newtest - < Dockerfile


container 和 host 文件互相拷贝：

1、从container往host拷贝文件：

docker cp <container_id>:/root/hello.txt .

2、从host往container里拷贝文件，比较麻烦一点，首先停止Contaitner（当然不停止也能拷贝）

docker stop <container_name_or_ID>

然后执行拷贝操作，完成之后就可以在Contaitner里看到文件拉



三.注意点
=========

   1.在进行commit的时候,只有在该容器没有退出的时候才会有效.     
   2.save与export的区别,save是保存整个进行也有历史记录,而export只会保存当前容器.    
   3.Docker attach可以attach到一个已经运行的容器的stdin，然后进行命令执行的动作,   
     但是需要注意的是，如果从这个stdin中exit，会导致容器的停止。    
   4.上述问题可以使用docker exec来进行解决,如docker exec -it bb2 /bin/sh,其中   
     bb2是该容器的名称.    









## 有问题反馈
在使用中有任何问题，欢迎反馈给我，可以用以下联系方式跟我交流

* 邮件(1031379296#qq.com, 把#换成@)
* QQ: 1031379296
* weibo: [@王发康](http://weibo.com/u/2786211992/home)


## 感激

### chunshengsterATgmail.com


## 关于作者

### Linux\nginx\golang\c\c++爱好者
### 欢迎一起交流  一起学习# 
