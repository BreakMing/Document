# 将walk_these_ways部署到RDK S100

## 前置条件
 - RDK S100 开发板
 - 19V 5A以上的电源适配器
 - Type-C数据线
 - Unitree GO1
 - 网线

## 准备工作

### 第一步：使用SSH服务连接到RDK S100

打开VS Code,点击左下角的**蓝色图标**

![VS_Code_Windows](image/VS_Code_Windows.png "VS_Code_Windows")

点击**红色方框**内的选项

![VS_Code_connect_host](image/Vs_Code_connect_host.png "VS_Code_connect_host")

继续点击**红色方框**内的选项

![VS_Code_Config_SSH_hosts](image/VS_Code_Config_SSH_hosts.png "VS_Code_Config_SSH_hosts")

根据自己的**用户名**选择对应的目录路径

![VS_Code_Config_SSH_hosts](image/VS_Code_choose_user.png "VS_Code_Config_SSH_hosts")

配置**config**文件

 > - Host:你的设备名（自己定义的）
 > - HostName：设备连接的网络IP
 > - User:登录设备的用户名

![VS_Code_Config_SSH_hosts](image/VS_Code_config_setting.png "VS_Code_Config_SSH_hosts")

配置完成后，**保存**并退出

重复执行前面的步骤，在下面这个界面里，可以看到我们新添加的一个名为**RDK**的设备，点击即可通过SSH连接

![VS_Code_Config_SSH_hosts](image/VS_Code_connect_device.png "VS_Code_Config_SSH_hosts")

输入**用户密码**，就完成了连接，

### 第二步：安装docker内核（引擎）
前往docker官网，根据[文档教程](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)一步步执行，即可在Ubuntu系统上安装**Docker Engine**

### 第三步：下载dockerfile，并传入RDK S100

使用百度网盘下载**walk-these-ways**的dockerfile

> 链接：？？？
> 提取码：？？？

进入[Xfhp官网](https://www.xshell.com/zh/free-for-home-school/ "Xthp")

你将看到以下界面，点击红框所示的**下载**按钮

![VDownload_Xft](image/Download_Xftp.png "Download_Xft")

下载完成后，打开安装包，一步步操作完成**Xftp**的安装

点击运行**Xftp 8**软件，会出现如下界面，点击**红色方框**内的按钮

![Xftp_display](image/Xftp_display.png "Xftp_display")

出现以下窗口后，键盘按下**Ctrl+n**新建会话

![Xftp_display_windows](image/Xftp_display_windows.png "Xftp_display_windows")

弹出一个新的窗口，根据实际情况更改红色方框内的内容

> 名称：自定义的名称
> 主机：设备的IP4地址
> 用户名：登录设备的用户名
> 密码：登录设备的密码


![Xftp_RDK_config](image/Xftp_RDK_config.png "Xftp_RDK_config")

设置完成后即可连接到我们的设备

将**dockerfile**文件传输到设备**用户目录**下的Dockerfile目录下

> Note：Dockerfile目录需要自己创立

### 第四步：配置RDK的静态IP

执行以下命令查看本机**网络接口**名称

> ip addr

以下图为例红色方框内以eth开头的是网线接口，蓝色方框内以wlan开头的是wifi接口

![IP_show](image/RDK_IP_show.png "IP_show")

执行以下命令使用vim工具编辑 Netplan 配置文件

> sudo vim /etc/netplan/01-hobot-net.yaml 

> Note：文件名可能有所区别，但路径都是一样的

修改 Netlan 配置文件中的**eth0**配置成静态的IP4，ip地址为**192.168.123.233**

![IP_change](image/RDK_IP_change.png "IP_change")

## 环境配置？？？

执行以下命令进入到Dockerfile目录下

> cd ~/Dockerfile/

执行以下命令构建一个镜像，镜像名叫hexchip/walk_these_ways

> docker build -t hexchip/walk_these_ways .

启动go1，进入到**挂载模式**

根据hexchip/walk_these_way这个镜像生成一个容器

> docker run -it --rm hexchip/walk_these_ways

进入容器后，顺序执行以下命令

> cd ~/deploy/go1_gym_deploy/scripts
> python deploy_policy_s100.py

因为我们需要在执行这个代码的时候执行另外一个代码，所以我们可以新起一个Terminal，执行以下命令，就可以在运行的容器中执行命令了

> docker exec -it {Contarin ID} bash

> Note：Contarin ID在命令行中查找可运行下面这个命令，它会列出所有的容器信息
> docker ps -a --no-trunc

顺序执行以下命令

> cd ~/deploy/go1_gym_deploy/unitree_legged_sdk_bin
> ./lcm_position

根据提示操作键盘和遥控器






