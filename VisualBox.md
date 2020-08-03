<!--
 * @version: Python 3.7.3
 * @Author: Louis
 * @Date: 2020-08-03 10:58:15
 * @LastEditors: Louis
 * @LastEditTime: 2020-08-03 10:58:25
-->
[运行卡顿解决办法](https://www.xuzefeng.com/post/85.html)

1. 开启CPU的硬件虚拟化功能
    * 现在的CPU几乎都支持硬件虚拟化功能，英特尔称之为VT-x技术，AMD称之为AMD-V技术。在百度搜索你的笔记本型号或主板型号+开启虚拟化，就可以找到相应的开启方法。一般是开机进入bios，然后找到虚拟化技术的选项，将disabled改为enabled。据我所知，用英特尔CPU的电脑，虚拟化技术的选项名称大概含有“virtualization technology”的字眼。
    * BIOS开启成功后，在虚拟机的设置中，启用硬件加速。
2. 给虚拟机分配足够的内存
    * 既然本机有4GB的内存，那么可以分配1GB供虚拟机上的Ubuntu使用。内存大小根据虚拟机系统的需要来定，如果你跑XP，那么分配512MB已经很足够了。
3. 开启3D加速，分配足够显存
    * 笔者觉得当初Ubuntu界面卡顿很有可能跟显卡方面的设置有关。于是开启了3D加速，分配了32MB的显存给VirtualBox。性能提升很明显。
4. 安装VirtualBox增强功能
    * 启动虚拟机。单击虚拟机菜单中的“设备”>“安装增强功能”，也可以按快捷键Host+D。Host键就是虚拟机窗口右下方显示的键，默认为Right Ctrl，即右边的Ctrl键。然后系统会加载增强功能所在的虚拟光盘。点击运行，按提示完成安装，重启虚拟机。