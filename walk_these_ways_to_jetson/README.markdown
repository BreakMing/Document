# walk_these_ways 部署到 Jetson orin

## 在Jetson orin上安装运行的必要程序

### 第一步：下载docker镜像

执行以下脚本下载docker镜像
> cd go1_gym_deploy/scripts && ./send_to_unitree.sh

### 第二步：通过SSH连接到Jetson板

> ssh unitree@192.168.123.15

### 第三步：将传输的docker镜像导入到本地的docker环境中

> chmod +x installer/install_deployment_code.sh
> cd ~/go1_gym/go1_gym_deploy/scripts
> sudo ../installer/install_deployment_code.sh

## 启动GO1 并执行程序

### 第一步：启动GO1并进去阻尼模式

- First：[L2+A]  
- Sencond:[L2+B]
- Third:[L2+L1+Start]

### 第二步：使用SSH连接Jetson 执行新的LCM控制服务

> ssh unitree@192.168.123.15

> cd ~/go1_gym/go1_gym_deploy/autostart
> ./start_unitree_sdk.sh

### 第三步：执行makefile文件

> cd ~/go1_gym/go1_gym_deploy/docker
> sudo make autostart

在这个makefile文件中主要执行了以下几个操作

- 1：根据之前传入的dockerfile构建一个新的docker镜像
- 2：将本地目录下的GO1_GYM挂载到容器内步
- 3：以这个镜像构建一个新的容器
- 4：运行setup.py这个文件来安装go1_gym这个依赖
- 5：到go1_gym_deploy/scripts目录下执行deploy_policy.py，执行策略模型

后面根据按键提示进行操作