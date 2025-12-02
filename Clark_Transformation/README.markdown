# 克拉克变换

## 一、 基础理论知识

### 1、无刷电机三相电流的120°相位差如何矢量化（方向，大小化）

![三相无刷电机示意图](image/Motor_Analot.png "Motor_Analot")

根据无刷电机的物理结构与它的绕线方式可知，三个线圈的角度插值120°，流过各个电感线圈的电流方向也是双向的，所以我们可以根据这个特性做出如下所示的平面三相坐标系

![电流矢量坐标系](image/Current_vector.png "Current_vector")

在这个坐标系中每个维度相差120°，这三个指向箭头代表的只有电流的大小和方向。因为电流在线圈中会产生磁场，因此也可以等效理解为磁场的大小和方向，我们将三个矢量相加减，就可以得到等效的磁场方向，如下图所示。

![每相的电流在同一坐标系下的示意图](image/Current_Sampling.png "Current_Sampling")

![电流在矢量图中的变化](image/Current_Vector_Change.png "Current_Vector_Change")

其中蓝绿红三个颜色代表的就是在同一时刻，流过无刷电机的三相电流的大小和方向。中间的磁铁就是拟合出来的磁场的方向

### 2、如何从平面三相坐标系转换成平面二相坐标系

这个过程可以理解为是一个力的分解过程，线圈在通电后可以等效为一个磁铁，这个‘磁铁’‘吸力’的大小和流过的电流大小有关，可以通过以下过程进行理解。

![电流转变为磁力的关系](image/Current_To_Magnetism.png "Current_To_Magnetism")

对于力的分解，我们先看这个图

![电流产生的磁力的分解](image/resolution_of_Magnetism.png "resolution_of_Magnetism")

将ia ib ic沿α和β坐标系做分解，
黄色为ia，蓝色为ib，红色是ic  

那么这三个矢量在α和β坐标轴上的分量可以如下表示

> **Iα = ia - ib * sin30°-  ic * cos60°**
> **Iβ = ib * cos30°-  ic * cos30°**

把三角函数替换为具体的数值

> **Iα = ia - 1/2ib - 1/2ic**
> **Iβ = √3/2ib - √3/2ic**

根据上面这两个公式我们可以得出下面这个矩阵式子

![克拉克变换公式](image/Clark_Equation.png "Clark_Equation")

### 3、然而在多数情况下通常会添加系数2/3或√2/√3，分别是等幅值变换和等功率变换，为什么需要这么做呢

#### 等幅值变换

主要原因：变换后的α和β所合成的矢量峰值与原始三相峰值不同

根据基尔霍夫电流定律
> **ia + ib + ic = 0**
我们将这个公式代入到**Iα**公式中
> **Iα = ia - 1/2ib - 1/2ic**‘
可以得出以下式子
> **Iα = 3/2 ia**
可以发现我们输入的是ia但输出的Iα是1.5倍的ia，在数值上会进行一个放大，所以我们会在矩阵公式前乘上一个系数后，就变成了如下形式。

![等幅值克拉克变换公式](image/Same_Amplitude_Clark.png "Same_Amplitude_Clark")

这样做可以在源头控制了数值的统一，不用再为后续的计算增加额外的系数，根据数值就可以反映出实际的电流大小

#### 等功率变换

一般应用在电力系统、电机设计中要计算功率和效率的地方，保持功率不变能更准确地评估能耗。

## 克拉克变换测试 

通过ADC采集无刷电机任意两相的电流信号进行克拉克变换所需的时间。

### 1、 在STM32平台上

#### （1） 在72M主频下

72Mhz   只有计算            95.5us

![等幅值克拉克变换公式](image/stm32_72M_jisuan.jpg "Same_Amplitude_Clark")

72Mhz   采样加计算          105.5us

![等幅值克拉克变换公式](image/stm32_72M_caiyangjisuan.jpg "Same_Amplitude_Clark")

#### （2） 在48M主频下

48Mhz   只有计算            114.5us

![等幅值克拉克变换公式](image/stm32_48M_jisuan.jpg "Same_Amplitude_Clark")

48Mhz   采样加计算          127.5us

![等幅值克拉克变换公式](image/stm32_48M_caiyangjisuan.jpg "Same_Amplitude_Clark")

### 1、 在CW32平台上

#### （1） 在96M主频下

96Mhz  软件计算             379us

![等幅值克拉克变换公式](image/cw32_96M_ruanjisuan.jpg "Same_Amplitude_Clark")

96Mhz  硬件计算             329us

![等幅值克拉克变换公式](image/cw32_96M_yingjisuan.jpg "Same_Amplitude_Clark")

96Mhz  采样加软件计算        385us

![等幅值克拉克变换公式](image/cw32_96M_caiyangruanjisuan.jpg "Same_Amplitude_Clark")

96Mhz  采样加硬件计算       334us     

![等幅值克拉克变换公式](image/cw32_96M_caiyangyingjisuan.jpg "Same_Amplitude_Clark")

#### （2） 在48M主频下

48Mhz  软件计算             456us

![等幅值克拉克变换公式](image/cw32_48Mhz_ruanjisuan.jpg "Same_Amplitude_Clark")

48Mhz  硬件计算             400us

![等幅值克拉克变换公式](image/cw32_48M_yingjisuan.jpg "Same_Amplitude_Clark")

48Mhz  采样加软件计算        463us

![等幅值克拉克变换公式](image/cw32_48M_caiyangruanjisuan.jpg "Same_Amplitude_Clark")

48Mhz  采样加硬件计算       406us  

![等幅值克拉克变换公式](image/cw32_48M_caiyangyingjisuan.jpg "Same_Amplitude_Clark")





