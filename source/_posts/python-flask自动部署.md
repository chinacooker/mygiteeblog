---
title: python+flask自动部署
date: 2020-05-20 10:51:07
categories:
	- python
tags:
	- python
	- flask
	- docker
	- k8s
	- kubectl
	- ci
	- deploy
	- apply
	- svc
	- pod
	- grep
	- yaml
---
## 本地环境
python3.6.8,Flask==0.12,
mac10.14.6,Docker=19.03.8
python,flask和docker的下载方式就不再赘述了
## 项目
为了演示方便，我们建立一个简单的项目文件夹，就叫flask-service，文件夹下就一个文件app.py和依赖requirements，app.py代码如下：
```
from flask import Flask
app = Flask(__name__)
@app.route('/', methods=['GET'])
def home():
    return '<h1>Home</h1>'
if __name__ == '__main__':
    app.run("0.0.0.0")
```
tips：requirements的生成办法可以下载pip install pipreqs，到项目目录下使用pipreqs .即可生成依赖文件。
<!-- more -->
## docker部署
我们首先尝试用docker构建这个项目的镜像，并且部署这个服务。
首先，我们再项目文件夹下建一个docker构建文件Dockerfile，代码如下：
```
FROM python:3.6.8
COPY requirements.txt /app/
COPY . /app/
WORKDIR /app/
RUN pip install -r requirements.txt
EXPOSE 5000
ENTRYPOINT [ "python" ]
CMD ["app.py"]
```
到Dockerfile所在目录后，本地构建image：
```
docker build -t python-project:2.0 .
```
构建完后，用可以查看我们构建的镜像：
```
$docker iamges
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
python-project               2.0                 bb9738219ecb        13 seconds ago       940MB
```
可以看到，镜像的名字是python-project，标签是2.0，这些都是我们自己指定的。<br>
然后就是部署了：
```
$docker run -d -p 5000:5000 --name flaskpro python-project:2.0
```
表示我们把本地5000端口（前面一个5000）映射容器的5000端口，-d是后台运行，--name是知道容器的名字，而python-project:2.0则是image的信息。
我们就可以访问[http://localhost:5000/](http://localhost:5000/)来访问我们的项目了。
我们来看下容器运行的情况：
```
$docker ps -a
CONTAINER ID        IMAGE                COMMAND             CREATED              STATUS              PORTS                    NAMES
5955224fca64        python-project:1.0   "python app.py"     About a minute ago   Up About a minute   0.0.0.0:5000->5000/tcp   flaskpro
```
可以看到容器的一些信息：容器id，image，运行命令，创建时间，当前状态，端口映射情况，我们取的名字。
## gitlab构建和k8s部署
这里我们的gitlab环境已经有了，且runner也已经配置了对应的镜像仓库地址，镜像仓库也配置了gitlab的私有token；并且runner有k8s某个namespace的权限。
我们在远端建立一个同名的仓库，在本地的项目里加上.gitlab-ci.yml文件，配置如下：
```
image: docker
variables:
  tag: registry.gitlab.baixing.cn/xxx/yyy:${CI_BUILD_REF_NAME}
  image: registry.gitlab.baixing.cn/xxx/yyy:${CI_PIPELINE_ID}
stages:
- build
- master-deploy
build:
  stage: build
  only:
  - master@xxx/yyy
  script:
  - docker login -u gitlab-ci-token -p $CI_BUILD_TOKEN registry.gitlab.xxx.cn
  - docker build --pull -t $image .
  - docker tag $image $tag
  - docker push $image
  - docker push $tag
  - docker rmi $image
master-deploy:
  stage: master-deploy
  only:
  - master
  tags:
    - k8s-dev
  script:
    - kubectl set image -n namespace deployment/flask-tools flask-tools=$image
```
### gitlab镜像构建
首行是拉取docker镜像，如果你的环境有的话，这里可以忽略。
<br>
```
docker login -u gitlab-ci-token -p $CI_BUILD_TOKEN registry.gitlab.xxx.cn
```
-u,-p指定runner登录镜像仓库的用户名和密码（token名和token），registry.gitlab.xxx.cn这个则是你的gitlab的仓库地址。
<br>
```
docker build --pull -t $image .
```
.表示根据当前目录的Dockerfile构建image，而--pull表示尝试pull镜像的最新版本，-t则是标签（这里用变量image的值）
<br>
```
docker tag $image $tag
```
tag是将标记为变量image的镜像标记为变量tag，并且新建一个镜像出来，这里其实就是把CI_BUILD_REF_NAME作为tag加上。
tips:docker tag详解
假如我们有这样一个镜像test:v1，docker tag test:v1 test:v2会建出新的镜像test:v2，用docker tag test test:v2可以达到一样的效果。
```
docker push $image
docker push $tag
```
这没什么好说的，直接push镜像到镜像仓库，这里我们将2个镜像都push了过去。
<br>
```
docker rmi $image
```
这个则是把$image这个镜像删除。
<br>
上面只是一次尝试，还是有些多余的部分的，其中对于标签的处理和镜像的推送以及删除上面，为什么要搞2个不同的tag，这里其实我也是参考，后续如果发现没有必要会进行修改。
### k8s镜像和服务部署
不难发现我们的ci脚本里其实只有一句是关于后续的k8s部署的：
```
kubectl set image -n namespace deployment/flask-tools flask-tools=$image
```
这里的含义是在指定namespace下的部署空间flask-tools使用指定的image进行部署。前一个flask-tools是指你已经提前创建的部署空间，后一个是部署容器的名字。
<br>
所以我们看到了，我们在k8s的namespace下还没有flask-tools这个部署空间。我们首先切到k8s的namespace，在本地创建一个yaml配置文件，代码如下：
```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: flask-tools
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: flask-tools
    spec:
      containers:
      - command:
        - python
        - app.py
        image: registry.gitlab.baixing.cn/xxx/yyy:pipelineid
        imagePullPolicy: IfNotPresent
        name: flask-tools
        ports:
        - containerPort: 5000
      imagePullSecrets:
      - name: zzz
```
上面的pipelineid可以用之前gitlab里面最新的一个pipelineid，imagePullSecrets是私有库的拉取秘钥的定义。
然后将这个文件推到namespace下：
```
kubectl apply -f deploy.yaml
```
执行后我们可以看到容器已经创建：
```
$ kubectl get pod | grep flask-tools
flask-tools-6c95b87b4c-757cf                   1/1     Running            0          37m
```
这时我们已经有了一个容器，并且运行在port5000，但是我们无法跟他交互。
我们还需要一个服务。
类似的，我们本地需要一个配置yaml文件：
```
apiVersion: v1
kind: Service
metadata:
  name: flask-tools
spec:
  ports:
  - nodePort: 30999
    port: 5000
    protocol: "TCP"
    targetPort: 5000
  selector:
    app: flask-tools
```
我们需要保证targetPort与之前我们起的容器里服务的port一致。
同样的kubectl apply -f 文件到k8s即可。
我们可以通过下面的命令看到服务的情况：
```
$ kubectl get svc | grep flask-tools
flask-tools                    NodePort    10.108.238.169   <none>        5000:30999/TCP   7d
```
可以看到，我们起来个服务给外部访问，端口是30999，映射的是本地的5000端口，而本地的5000端口映射的是容器的5000端口。
这时我们可以直接通过http://本地id:30999来访问你的服务了。
同时，你在你的gitlab上push代码到master分支也会触发自动的cicd。
### 总结
我们再来回顾一下我们要做哪些工作，自己的k8s环境，镜像仓库，gitlab，然后利用gitlab ci实现初始image的push，在k8s上配置好部署（需要用到image）和服务，几个流程走好后原则上我们就实现了自动部署了。