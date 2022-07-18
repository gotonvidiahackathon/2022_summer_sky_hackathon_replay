双系统方式安装linux18.04\
\
by 宋尧哲

## 1.制作U盘启动盘
### 1.1 [Ubuntu官网下载18.04.6LTS](https://releases.ubuntu.com/18.04.6/?_ga=2.81622804.201014072.1658110110-1011758718.1649071342)
### 1.2[下载UltralISO刻录软件](https://www.ultraiso.com/)
&emsp;&emsp;注意，不要用联想自带的商店安装，用下载的安装包安装。
### 1.3 刻录
&emsp;&emsp;打开UltraISO，菜单栏文件-打开-找到下载的Ubantu位置双击 然后菜单栏启动-写入硬盘镜像文件，弹出界面选中插入U盘，其他默认就行，点击写入，结束后确定退出，拔出U盘，插入到要安装Ubuntu系统的电脑。
## 2.安装Ubuntu系统
### 2.1 给ubuntu配置空间
&emsp;&emsp;在windows中分配愿意给ubuntu的空间。我的电脑右键--管理--磁盘管理。此时若报错：无法连接虚拟磁盘服务，需要卸载UltraISO。\
&emsp;&emsp;用压缩卷（如果这个盘有的空间还想用在windows上）、删除卷（一个磁盘全部给ubuntu）、删除分区（前两个操作完之后操作）腾出一块未分配的磁盘空间作为安装区。
### 2.1 设置U盘启动
&emsp;&emsp;重启电脑，开机时按F2(不同型号可能有差别，例如F2/F10/F11/F12，可以百度自己牌子电脑的启动项管理，设置为U盘启动)
### 2.2 安装设置
&emsp;&emsp;等候一段时间，进入到安装界面。
1. **前两页**是选语言和键盘布局，直接默认英文，因为中文路径在运行代码时可能会有意想不到的坑。
2. **第三页**wifi最好连上，方便后续安装更新。
3. **第四页**，更新选项上面选择正常安装(normal),可以勾选安装Ubuntu时下载更新。third party不要勾选，这时候装的软件可能版本不太对，可能出大问题。
4. **第五页**，安装类型选择最后一个else。
5. **开始给每个区分配大小**，可以看到有一个free space空间，就是之前windows系统中预留的给Ubuntu安装的空间。参考[这篇博客](https://blog.csdn.net/baidu_36602427/article/details/86548203)进行分配空间，**注意：**\
1). /boot最后装，不然可能会bug\
2). /home倒数第二装，给/boot留1024M空间就行。\
3). 如果是重装Ubuntu（之前电脑就有一个），记得勾选format保险起见\
4). 不想更改数据的盘不要动。
### 2.3 安装好之后一些基础配置
#### 2.3.1 换源
&emsp;&emsp;我换了之后装驱动以及其他软件反而出了一些问题，以及默认的源并不是很卡，所以这里不详述了
#### 2.3.2 安装输入法
1. 打开language support， 这时候会自动弹出窗口让安装依赖，直接安装就好了。
2. install/remove languages...打开，看下chinese（simplified）有没有勾选，没有的话选上，确认让自动安装依赖
3. 
```
sudo apt install ibus
sudo apt install ibus-gtk ibus-gtk3
sudo apt install ibus-pinyin
```
4. 重启。设置里打开region & language。如果是ubuntu18.04可以点下面加号添加chinese--chinese（intellegent pinyin）然后win+空格可以切换输入法使用了。如果是ubuntu20.0.4，settings--keyboard里可以找到这个设置。



到最后一步，settings -keyboard -- input sources +号 --添加中文