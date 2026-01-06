# K230 AI小相机复刻指南

## 相关视频

![video_cover](image/video_cover.jpg "video_cover")
视频链接：https://www.bilibili.com/video/BV1dKBxBZEJ6

## 交流群

![Wechat_qun_qrcode](image/weixin_qun_qrcode.jpg "Wechat_qun_qrcode")


## 复刻流程

### 1、制作电路板

#### （1）主控板和扩展版工程文件

硬件开源地址：https://oshwhub.com/hexchip/k230ai-minicomputer


#### （2）PCB下单注意事项

- 板材选择**FR-4**，板厚需要选择**1.2mm**，可参考下图

![bancaihebanhouxuanze](image/bancai_houdu.png "bancaihebanhouxuanze")

- 在阻抗选择这里我们点击**更多层压结构**，在弹出的层压结构中选择**JLC0612H-1080A**这个选项

![gengduocengyajiegou](image/gengduocengyajiegou.png "gengduocengyajiegou")

![JLC06121H-1080A](image/JLC06121H-1080A.png "JLC06121H-1080A")

- **层压顺序、阻抗管控、阻焊覆盖、最小孔径/外径**按照下图红框内进行选择，其他**默认**设置即可

![zhongyaoxuanze](image/zhongyaoxuanze.png "zhongyaoxuanze")


### 2、固件烧录

#### 固件文件和烧录软件

固件和烧录软件下载地址: https://pan.baidu.com/s/1mLnL9TvsthU_Vq6_ZLLApQ
提取码: w84f

#### 烧录方式

- 使用**8GB**以上的tf卡进行固件烧录

##### 1、使用rufus软件烧录

- 将**tf卡**插入读卡器并连接电脑，按照下图依次点击并选择**固件**，即可开始烧录

![rufus_three_buzhou](image/rufus_three_buzhou.png "rufus_three_buzhou")

- 烧录开始后会出现如下弹窗，点击**确定**即可

![rufu_warning_1](image/rufu_warning_1.png "rufu_warning_1")

![rufus_warning_2](image/rufus_warning_2.png "rufus_warning_2")

- 等待烧录完成

![rufus_waitburn](image/rufus_waitburn.png "rufus_waitburn")

- 烧录成功

![rufus_burnwin](image/rufus_burnwin.png "rufus_burnwin")

##### 2、使用BalenaEtcher软件烧录

- 将**tf卡**插入读卡器并连接电脑，选择要烧录的固件

![balenaetcher_choose_file](image/balenaetcher_choose_file.png "balenaetcher_choose_file")

- 点击**现在烧录！**

![balenaetcher_click_burn](image/balenaetcher_click_burn.png "balenaetcher_click_burn")

- 等待烧录完成

![balenaetcher_waitburn](image/balenaetcher_waitburn.png "balenaetcher_waitburn")

- 烧录成功

![balenaetcher_burnwin](image/balenaetcher_burnwin.png "balenaetcher_burnwin")

### 怎么玩这个小相机

#### 1、拨轮控制使用

- 向上向下滚动来选择命令，按下拨轮可以理解为敲下回车按键

![boun_choose_cmd](image/boun_choose_cmd.jpg "boun_choose_cmd")

![bolun_ls_display](image/bolun_ls_display.jpg "bolun_ls_display")

#### 2、终端控制使用

##### （1）串口连接

- 使用任意**USB转串口模块**与下图**红框**内的接口连接，引脚定义可在**主控板工程文件**内找到

![Uart_hardware_link](image/Uart_hardware_link.png "Uart_hardware_link")

- 推荐使用**串口调试助手+TCP/UDP**进入**终端调试模式**，调试助手下载地址如下：
- https://apps.microsoft.com/detail/9nblggh43hdm?hl=zh-CN&gl=CN

![TCPUDP_terrmial_mode](image/TCPUDP_terrmial_mode.png "TCPUDP_terrmial_mode")

- 输入基础的linux命令，即可通过串口实现对小相机的控制。（TAB键也可以对命令进行补全）

![Uart_control](image/Uart_control.png "Uart_control")

##### （2） 键盘连接

- 将键盘的收发器通过转接器与小相机进行连接，屏幕上出现如下数据表示连接成功

![keyboard_link](image/keyboard_link.jpg "keyboard_link")

- 通过键盘可以像串口控制一样，输入基础的linux命令，实现对小相机的控制

![Uart_control](image/Keyborad_ls_cd.jpg "Uart_control")

#### 3、串口助手使用方法

- 以拨轮操作为例，小相机连接要读取的设备后会弹出如下日志，会识别串口芯片型号并注册一个**ttyUSB0**

![Uart_read_usb](image/Uart_read_usb.png "Uart_read_usb")

- 滚动拨轮选**择__show_all_app__这**个命令，进入后再找**到/sdcard/app/helloworld命**令。

![show_all_app](image/show_all_app.png "show_all_app")

![sdcard_app_helloword](image/sdcard_app_helloword.png "sdcard_app_helloword")

- 按下拨轮就可以看到屏幕上读取到了串口的数据。

![Uart_readwin](image/Uart_readwin.jpg "Uart_readwin")

#### 4、Micropython使用方法

- 想要知道**Micropython**的用法我们可以在任意界面输**入/sdcard/Micropython -h这**条命令，在终端中就会出现如下的配置信息

![Micropython_help](image/Micropython_help.png "Micropython_help")

- 在这里简单介绍三种常用的用法。

##### （1）进入py交互模式界面

- 在任意界面输**入/sdcard/Micropython -i这**条命令

![run_micropython](image/run_micropython-i.png "run_micropython")

- 当终端出现如下显示时证明已成功进入**Micropython**的交互式模式了

![Micropython_join_-i](image/Micropython_join_-i.png "Micropython_join_-i")

##### （2）执行py脚本

- 首先要知道执行的**py脚本**放置在哪个文件夹下，以**face_mesh.py**这个脚本为例
- 在任意界面下输**入/sdcard/micropython /sdcard/examples/05-AI-Demo/face_mesh.py这**条命令

![Micropython_join_-i](image/py_face_mesh.png "Micropython_join_-i")

- 就会去执行**face_mesh.py**脚本了，效果如下图所示

![run_face_mesh](image/run_face_mesh.jpg "run_face_mesh")

##### （3）进入CanMV IDE实时运行脚本

- 在任意界面下输**入/sdcard/micropython -X ide这**条命令，出现如下日志后证明可以进行连接了

![py_CanMV_IDE](image/py_CanMV_IDE.png "py_CanMV_IDE")

- 我们打开**CanMV IDE**软件点击左下角连接按钮，有切换动作和下图一样证明连接成功了

![CanMV_IDE_link](image/CanMV_IDE_link.png "CanMV_IDE_link")

- 这时我们就可以在**CanMV IDE**软件内运行我们自己编写的**py脚本**了。

***以上就是K230 AI小相机的复刻指南的基本内容了，欢迎大家复刻和使用我们的小相机去实现自己的demo，有什么建议和想法也欢迎大家加入我们的微信群积极讨论。***









    