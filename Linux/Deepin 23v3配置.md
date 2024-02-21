## Deepin安装ssh服务并设置开机启动
### 方法一：

#### - 安装ssh服务

```
sudo apt-get install openssh-server
```

#### - 启动ssh服务

```
sudo /etc/init.d/ssh start
```

#### - 设置开机自启动

 ```
sudo systemctl enable ssh
```

#### - 关闭ssh开机自动启动命令

 ```
sudo systemctl disable ssh
```

#### - 单次开启ssh

 ```
sudo systemctl start ssh
```

#### - 单次关闭ssh

 ```
sudo systemctl stop ssh
```

#### - 设置好后重启

 ```
reboot
```
### 方法二：

#### - 1.安装ssh服务

```
sudo apt-get install ssh
```

#### - 2.编辑ssh配置文件

- 打开sshd_config文件

```
sudo nano /etc/ssh/sshd_config
```


- 在文件的末尾添加如下语句：

```
PasswordAuthentication yes
```

- 若想允许root用户远程登陆，再添加如下语句：

```
PermitRootLogin yes
```

- 保存退出

#### - 3.启动服务

```
sudo service ssh start
```

#### - 4.添加开机启动

```
sudo update-rc.d ssh enable
```

#### - 5.查看ssh服务状态

 ```
/etc/init.d/ssh status
```
## Deepin配置开发环境
### 安装vscode
#### 使用 apt 安装 Visual Studio Code
##### 方法一：
	- sudo apt update
	- sudo apt install com.visualstudio.code  
##### 方法二：
- Visual Studio Code 在官方的微软 Apt 源仓库中可用。想要安装它，按照下面的步骤来：
###### - 01.以 sudo 用户身份运行下面的命令，更新软件包索引，并且安装依赖软件：

  ```
  sudo apt update
  sudo apt install software-properties-common apt-transport-https wget
  ```
  
###### - 02.使用 wget 命令插入 Microsoft GPG key ：  
	
  ```
  wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
  ```
  启用 Visual Studio Code 源仓库，输入：  
  ```
  sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
  ```
###### - 03.一旦 apt 软件源被启用，安装 Visual Studio Code 软件包：  
  ```
  sudo apt install code
  ```
  当一个新版本被发布时，你可以通过你的桌面标准软件工具，或者在你的终端运行命令，来升级 Visual Studio Code 软件包：  
  ```
  sudo apt update
  sudo apt upgrade
```
