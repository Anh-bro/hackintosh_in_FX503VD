# 华硕笔记本黑苹果OC文件分享
## 配置
- 型号ASUS FX63VD
- CPU intel i5-7300HQ （kaby lake代号）核显 hd630
- 有线网卡RTL8111
- 无线网卡intel Dual Band Wireless-AC 8265
- 500G的nvme固态硬盘
## 实现情况
MacOS13 Ventura日常使用 内置显示器亮度调节 声音调节 蓝牙、wifi、有线网络正常 几个USB口都正常（USB接口定制）hdmi视频输出 睡眠唤醒
## 未实现内容
1. 没有驱动独立显卡GTX1050（高系统版本无解）
2. 没有驱动触控板（这个笔记本不插电源不能用，所以我也不打算驱动触控板）
3. 隔空投送、屏幕镜像搜不到我手上的ipad（可能因为ipad系统是13，太老了）
4. 无法设置OC为第一启动项 每次只能手动进bios选择OC的启动项(应该跟OC配置无关，猜测是华硕主板的问题)
## 配置过程遇到的问题以及处理方式
- 无法调节内置显示器屏幕亮度
    - 打开config.plist NVRAM -> Add -> 第三个7C436开头的 -> 在键boot-arg的值中添加 -wegnoegpu 即禁用独立显卡只使用核显
- hdmi无法输出视频
    - 具体表现插上hdmi线外接显示器没反应 hackintool里看不到外界显示器
    - 试过接口定制失败了 最后是在github上搜索华硕黑苹果找了个配置差不多的 说是hdmi输出正常 我就看了他的config.plist里的devicePropertise中的Pci（0x2,0x0）中的数据 hd630的平台id要设置成hd620的59160000 改过之后插上hdmi就能用了
    - 也许还会遇到开机不能点亮外接显示器 需要拔插hdmi线 需要在刚刚提到的boot-arg中添加一个参数：igfxonln=1
## 总结
制作EFI文件 

-> 寻找系统镜像并刻录至U盘 

-> 将制作好的EFI文件放到U盘的启动分区  
-> 
启动U盘上的OC，经过数次重启安装MacOS 

-> 
将U盘中的启动分区上的/EFI/OC文件夹复制到
硬盘的启动分区上的/EFI文件夹中 

-> 设置/EFI/OC/OpenCore.efi为第一启动项 

-> end

[国光写的教程](https://apple.sqlsec.com/)
