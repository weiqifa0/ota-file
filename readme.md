# 什么是OTA升级？
OTA是Over-the-Air的简称，OTA升级可以理解为用户正常使用过程中进行升级，OTA 升级旨在升级基础操作系统、系统分区上安装的只读应用和/或时区规则。

# 什么是Android AB系统更新
A/B 系统升级，也叫做无缝更新，A/B系统升级，顾名思义是有两个系统，在磁盘上开辟两个存储空间A/B存储空间，在升级过程中保证有一个可以正常运行的系统，采用这种方式可以大大提升更新的成功性，使用这种更新后，在ota 更新过程中，即使用户手机掉电，也能保证系统再次上电后可以正常运行。

# A/B系统更新的好处

* OTA 更新（往存储空间写入升级包时）可以在系统运行期间进行，而不会打断用户。
* 如果 OTA 失败，设备会启动到 OTA 之前的磁盘分区，并且仍然可以使用。
* 更新包可以流式传输到 A/B 设备，因此在安装之前不需要先下载更新包。
* 缓存分区不再用于存储 OTA 更新包，因此无需调整缓存分区的大小。

# A/B OTA系统和普通系统分区
普通系统只需要一个boot存储空间和一个system存储空间，但是A/BOTA系统需要开辟两个boot存储空间和两个system存储空间。
这是为了保证升级分区不对运行分区产生影响，保证系统OTA不宕机的一个保障。
![](https://img-blog.csdnimg.cn/20200214181409652.png)

# OTA升级流程
![](https://img-blog.csdnimg.cn/20200214183239458.png)

# 差分包升级和全包升级
全包升级是升级boot.img和system.img两个分区的所有内容，差分包升级的话，只升级增量部分，就是在基础版本上做差分升级。

因为只有修改部分的版本。所以差分包OTA升级文件会比全包OTA升级文件小很多，这样可以节省云端存储空间和下载流量。

但是因为差分包维护版本的需要特别小心，如果0.0.1版本想升级到0.0.3版本，中间有一个0.0.2版本没有升级，直接升级到0.0.3版本，在差分升级的情况下是会出错的，所以在每次出版本时需要专人维护。

基于以上原因，我们原来公司在选择上，选择了全包升级方式，一个升级包在500M左右，实际速度还满足要求。

# 升级包的制作
这部分google有自己的机制，在此基础上，rockchip和mtk也有自己相对应的文档，我们需要基于厂商的文档来制作升级包。

# 如何判断开机运行的分区地址
正常开机的时候，会出现每个分区的执行地址，可以在串口日志先观察开机时候运行的地址。
##Booting Android Image at 0x0207f800 ...

# 工作分工
* BSP 同事需要负责系统部分和recovery.cpp 部分验证和修改。
* app 同事需要和后台调试接口，包括下载update.zip包和给系统下发指令接口调试。


# 参考资料
[google-ota介绍](https://source.android.google.cn/devices/tech/ota/dynamic_partitions/implement#partitioning-changes"%3Ehttps://source.android.google.cn/devices/tech/ota/dynamic_partitions/implement#partitioning-changes)
[android-ota-系统启动](https://blog.csdn.net/guyongqiangx/article/details/72604355"%3Ehttps://blog.csdn.net/guyongqiangx/article/details/72604355)
