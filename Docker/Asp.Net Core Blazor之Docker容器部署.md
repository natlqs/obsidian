# 写在前面

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200628163154367-1257632442.png)

　　Docker 作为开源的应用容器引擎，可以让我们很轻松的构建一个轻量级、易移植的容器，通过 Docker 方式进行持续交付、测试和部署，都是极为方便的，并且对于我们开发来说，最直观的优点还是解决了日常开发中的环境配置与部署环境配置上的差异所带来的种种疑难杂症，从此推脱产品的措辞也少了——“我电脑正常啊！”。总之，Docker 伴随着 “真香定理” 的存在。

## 以 windows10 下安装 Ubuntu 子系统为例

**1. 1 在微软应用商店安装 Ubuntu**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624155616790-1561138729.png)

**1.2 启动并设置密码**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624155712931-1072754478.png)

 

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624155717011-1320725320.png)

另外，如果想要安装图像界面，就自行百度吧，这里就不安装了，真男人都是直接撸命令行的。

## Ubuntu 下安装 Docker

命令汇总：
```
sudo apt-get remove docker docker-engine docker-ce docker.io
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
      "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) \
      stable"
apt-cache madison docker-ce 
sudo apt-get install docker-ce
sudo service docker start
```

**2.1 移除 apt 官方旧的 docker 版本**

```
sudo apt-get remove docker docker-engine docker-ce docker.io
```

**2.2 更新 apt**

```
sudo apt-get update
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624160400324-79081933.png)

 **2.3 配置 apt 可以通过 HTTPS 使用拉取镜像**

```
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
```

**2.4 设置 Docker 官方的 GPG 密钥**

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624160932670-1421575028.png)

 **2.5 添加 stable 存储库**

```
sudo add-apt-repository \
     "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) \
     stable"
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624161055578-170390948.png)

 **2.6 安装 docker-ce 社区版**

```
sudo apt-get install docker-ce
```

**![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624161345030-1195695802.png)**

**2.7 启动 docker** 

```
sudo service docker start
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624161242197-1566803739.png)

 **2.8 结束了吗？还没，查看 docker 运行状态**

```
sudo service docker status
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624161538207-1494762848.png)

然后使用以下命令查看 docker 版本，会发现只有 Client，没有 server。所以这个就是在 windows 下的子系统的特殊性。需要额外下载 [**Docker for windows**](https://www.docker.com/products/docker-desktop)，作为 Docker 的服务端。

```
docker version
```

**2.9 安装并运行 Docker for windows**

**安装完成后，会自动重启电脑**，所以你熬夜写的代码要记得先保存。重启之后，再设置即可。

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624162312981-145949473.png)

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624162324904-677287336.png)

 **2.9.1 配置及刷新环境变量**

```
echo "export DOCKER_HOST='tcp://0.0.0.0:2375'" >> ~/.bashrc
source ~/.bashrc
```

**![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624162606686-1084906612.png)**

 在这端口为什么是 2375，注意看上面的 docker for windows 的配置，再次查看版本

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624162709202-1068387764.png)

 终于安装好 docker。

## 发布 Blazor

**3.0  因为 Blazor WebAssembly App 暂未支持 docker，所以新建一个 Blazor Server 项目**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624163140642-736235757.png)

**3.1 添加 docker 文件**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624163519389-1582777841.png)

 **3.2 选择 Linux 并修改 Dockerfile 文件**

```
# 使用运行时镜像
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
# 设置工作目录
WORKDIR /app
# 把目录下的内容都复制到当前目录下
COPY . .
# 运行镜像入口命令和可执行文件名称
ENTRYPOINT ["dotnet", "BlazorApp.dll"]
```
**3.3 发布（此过程有点久）**

**![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624163807104-1204946681.png)**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624163735895-994345386.png)

##  发布至 Docker

 　　在 windows10 子系统中，我们无需像独立的 Linux 需将文件拷贝至 Linux 系统中，通过以下命令查看到磁盘情况，会发现其实已经帮我们挂载好了，无需复制拷贝发布的文件，又是真香。

```
df -h
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624164337069-99882632.png)

 **4.1 直接 cd 进入发布路径**

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624164456468-1853688878.png)

 **4.2 构建镜像**

```
docker build -t blazorapp .
```

_注意，不能用大写，这里提示必须用小写来命名，并且有一个【.】在结尾_ 

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624164528511-557099534.png)

 **4.3 创建容器**

```
docker run -d -p 8072:80 blazorapp
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624164938728-861377311.png)

_说明：容器暴露 80 端口，并指定宿主机 8072 端口与其通信 (宿主机端口：容器暴露端口)。_

 **4.4 查看当前镜像**

```
docker image ls
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624165034749-1129954439.png)

_Nginx 请忽略，是后面我才安装的。_

## 完成发布

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200624165301907-834973166.png)

## docker 发布到私有仓库

发布到私有仓库，这里用 docker Hub 做示例，首先在 docker Hub 上注册好账号，然后进行推送。

**6.1 打上标记**

```
docker tag blazorapp liohuang/blazorapp
```

如未登录，会提示先登录账户。

**6.2 推送至仓库**

```
docker push liohuang/blazorapp
```

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200628152304179-624745027.png)

![](https://img2020.cnblogs.com/blog/720686/202006/720686-20200628150228440-1105270707.png)

 下次使用的时候使用 pull 命令拉取即可。

 **本文已独家授权给 DotNetGeek（ID:dotNetGeek）公众号发布**

全文完
