# First row of Android codes

## 开启第一行代码

### Android 系统架构（四层）

1. Linux内核层

   为设备的各种硬件提供了底层的驱动，如显示驱动、音频驱动、照相机驱动、蓝牙驱动、Wi-Fi驱动、电源管理等

2. 系统运行库层

   这一层通过一些C/C++库来为安卓系统提供了主要的特性支持。如SQLite库提供的数据库支持，OpenGL|ES库提供的3D绘图支持，Webkit库提供的浏览器内核支持等

   - 同样在这一层的还有Android运行时库，主要提供一些核心库，能够允许开发者使用Java语言来编写Android应用。另外，Android运行时库嗨包含了Dalvik虚拟机（5.0系统之后改为ART运行环境），它使得每一个Android应用都能够运行在独立的进程当中，并拥有自己的一个Dalvik虚拟机实例。相较于Java虚拟机，Dalvik是专门为移动设备定制的，针对手机内存、CPU性能有限等情况做了优化处理

3. 应用框架层

    主要提供了构建应用程序时可能用到的各种API

4. 应用层

   所有安装在手机的应用都属于这一层