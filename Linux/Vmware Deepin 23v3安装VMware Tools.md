>Ubuntu22是最新推出的一款支持桌面操作的Linux操作系统，界面友好，操作方便，受到了很多朋友的欢迎。但由于不熟悉，很多同学在安装VMware-tools时，会遇到一些问题，现将相关问题汇总解决如下：

### （1）虚拟机菜单栏工具条“重新安装VMware Tools(T)"无法点击

当之前安装过低版本的Ubuntu系统，或者是再交安装Ubuntu时，由于之前安装过VMware Tools，因此，虚拟机的菜单栏处是无法点击的，如下图所示：

![](https://pic4.zhimg.com/80/v2-a79146ea7bda51b42cb644b1fcde17cf_720w.webp)

此时，需要先卸载ubuntu中的vmware tools工具后，才能重新安装。在Ubuntu中点击右键，打开终端，输入卸载指令即可，卸载执行代码如下：

```
sudo apt purge open-vm-tools-desktop
```

输入上面指令后，回车即可卸载。注意，如果设定了用户密码，需要输入用户密码。完成后，先关闭虚拟机，然后再重新启动。再次进入系统后，就可以看到虚拟机上的菜单栏可以点击了。如果再次进入时，菜单仍无法点击，可以再次重复执行一次上面的指令。

### （2）重新安装VMware Tools的注意事项

点击虚拟机菜单栏的“重新安装VMware Tools(T)"，Ubuntu桌面上会出现一个文件，打开后，将名字为“VMwareTools-10.3.22-15902021.tar.gz"的文件直接拖到桌面（Ubuntu22支持此操作），然后，在文件上鼠标右键点击，在弹出菜单中点击”提取到此处“，将该压缩文件直接释放到桌面上。打开解压后的文件夹，点击右键->”在终端打开“，输入以下安装指令(此处是进入解压后文件夹，运行其中的vmware-install.pl文件)：

```
./vmware-install.pl
```

然后就开始安装，为了确保安装成功，请仔细阅读其中的英文提示，特别是后面的提示要覆盖原来的文件（针对多次安装不成功的，或安装一半中断的情况），要输入YES，然后再回车，不能全部采用默认值。

`安装完成后，直接关闭虚拟机，然后再启动虚拟机。（注意：不要直接点重启虚拟机，会出现错误。）`

系统重启后，可以打开/mnt/hgfs/目录，查看安装时设定的共享文件夹。

### （3）解决无法复制粘贴问题

重新安装后，很有可能还是无法在虚拟机与原主机之间进行复制粘贴，此时，可以再次打开终端，输入并执行以下指令：

```
sudo apt-get autoremove open-vm-tools
sudo apt-get install open-vm-tools
sudo apt-get install open-vm-tools-desktop
```

上面的指令是再次卸载，然后安装。指令执行完毕后，关闭虚拟机，再次重启，此时，多半会大功告成！如果还不行，请再次执行上面的指令。（再次提醒：不要直接点击重启，要先关闭虚拟机，然后再启动。）

指令执行情况，如下图所示：

```
sudo apt-get autoremove open-vm-tools sktop
```

![[assets/Vmware Deepin 23v3安装VMware Tools/da345632179c6c85136f59fe9792b0ce_MD5.jpg]]

```
sudo apt-get install open-vm-tools
```

![[assets/Vmware Deepin 23v3安装VMware Tools/b895d32e325df864ae145e746b633f23_MD5.jpg]]

``` 
sudo apt-get install open-vm-tools-desktop 
```

![[assets/Vmware Deepin 23v3安装VMware Tools/241193195c8e5c10fe71d991566fe36c_MD5.jpg]]

最后，放上一张Ubuntu22的自带桌面图，非常漂亮！

![[assets/Vmware Deepin 23v3安装VMware Tools/b311aaf5a828b7673baa838fc9157696_MD5.jpg]]