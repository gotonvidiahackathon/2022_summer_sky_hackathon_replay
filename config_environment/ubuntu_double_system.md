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
2). /home倒数第二装，给/boot留1024M空间就行; /var是后面docker容器下载镜像的默认路径，不要太小，给个20G吧。
3). 如果是重装Ubuntu（之前电脑就有一个），记得勾选format保险起见\
4). 不想更改数据的盘不要动。
### 2.3 安装好之后一些基础配置
#### 2.3.1 换源
&emsp;&emsp;我换了之后装驱动以及其他软件反而出了一些问题，以及默认的源并不是很卡，所以这里不详述了,所以直接更新就行。理论上前面连接了wifi并且勾选了自动更新后这里是不需要的，保险起见可以试一下，会发现确实没有更新库。
```
sudo apt-get update
sudo apt-get upgrade
```
#### 2.3.2 安装输入法
1. 打开language support， 这时候会自动弹出窗口让安装依赖，直接安装就好了。
2. install/remove languages...打开，看下chinese（simplified）有没有勾选，没有的话选上，确认让自动安装依赖
3. 
```
sudo apt install ibus
sudo apt install ibus-gtk ibus-gtk3
sudo apt install ibus-pinyin
```
4. 重启。设置里打开region & language。如果是ubuntu18.04可以点下面加号添加chinese--chinese（intellegent pinyin）然后win+空格可以切换输入法使用了。(如果是ubuntu20.0.4，settings--keyboard里可以找到这个设置。)
5. 到最后一步，settings -- keyboard -- input sources +号 -- 添加中文
#### 2.3.3 安装ssh
1. ifconfig查看IP，若报错显示没有net-tools，则根据报错提示命令安装
```
sudo apt install openssh-server
sudo apt install openssh-client
sudo nano /etc/ssh/ssh_config
```
2. 在跳出的窗口中，去掉PasswordAuthentication yes前面的#号，保存退出(ctrl+s--ctrl+x)

3. 重启ssh
```
sudo /etc/init.d/ssh restart
```
## 3. 安装CUDA、CUDNN
### 3.1 安装cuda-driver
1. nvidia-smi可以查看目前是否有驱动，如果报错，说明没装，如果是要卸载当前驱动：
```
之前通过ppa安装的，卸载如下：
sudo apt-get remove --purge nvidia*
以前是通过runfile安装的，卸载如下：
sudo ./NVIDIA-Linux-x86_64-384.59.run --uninstall #./..为安装时的runfile
```
2. 安装驱动\
和驱动配套的cuda版本见[官网](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)，记得根据自己想要装的cuda版本配置驱动版本。
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:graphics-drivers/ppa
ubuntu-drivers devices （查看NVIDIA显卡型号和推荐的驱动程序的模型）
sudo apt-get install nvidia-driver-  （输入这个然后按table补全看有哪些选择，选最新或者第二新，或者推荐的那个）
sudo reboot #重启后生效
nvidia-smi #如果没报错，则成功
```


### 3.2 安装cudatoolkit 11.0
1. 进入[官网](https://developer.nvidia.com/cuda-downloads)下载runfile，执行Base Installer部分代码，第二行后加一个--override
![在这里插入图片描述](https://img-blog.csdnimg.cn/cf93746dbfba454fa3f32a9b41370ad6.png)
```
wget http://developer.download.nvidia.com/compute/cuda/11.0.2/local_installers/cuda_11.0.2_450.51.05_linux.run
sudo sh cuda_11.0.2_450.51.05_linux.run --override
```
2. continue -- accept
3. 取消勾选cuda driver，即第一个，用方向键选中后回车取消勾选，再进入下一步

### 3.3 下载cudnn 8.0
1. 从[官网](https://developer.nvidia.com/rdp/cudnn-archive)下载：“Download cuDNN v8.0.4 (September 28th, 2020), for CUDA 10.1” --> cuDNN Library for Linux (x86)
2. 解压,将动态链接库和头文件放到cudatoolkit内相应目录
```
tar -zxvf cudnn-11.0-linux-x64-v8.0.4.30.tgz
sudo mv cuda/include/* /usr/local/cuda/include/
sudo mv cuda/lib64/* /usr/local/cuda/lib64/
```

3. 加入环境变量
```
sudo nano ~/.bashrc 
#添加这两行
export LD_LIBRARY_PATH=/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin
#ctrl+s  ctrl+x 保存退出
source ~/.bashrc
nvcc -V  #显示cuda版本说明安装成功
```

## 4.安装miniconda
```
wget -c https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
#一路enter yes
```


## 5. 安装tensorRT 7.1.3
1. [官网](https://developer.nvidia.com/nvidia-tensorrt-7x-download)下载\
TensorRT 7.1 GA \
TensorRT 7.1.3.4 for Ubuntu 18.04 and CUDA 11.0 TAR package
2. 安装\
可以参考[官网](https://docs.nvidia.com/deeplearning/tensorrt/archives/index.html#trt_7),选择对应版本(TensorRT 7.1.3),选择TensorRT Installation Guide，找到里面用tar file安装步骤，或者如下
 
 ```
 #给trt的库放在一个conda虚拟环境
 conda create -n trt
 conda activate trt
 conda install python==3.7
 tar -zxvf TensorRT-7.1.3.4.Ubuntu-18.04.x86_64-gnu.cuda-11.0.cudnn8.0.tar.gz
 cd TensorRT-7.1.3.4/python
 pip install tensorrt-7.1.3.4-cp37-none-linux_x86_64.whl
 cd ../uff
 pip install -i https://pypi.tuna.tsinghua.edu.cn/simple  uff- #回车补全
 cd ../graphsurgeon
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple gra #回车补全
cd ../samples
make
 ```

3. 添加环境变量
```
sudo nano ~/.bashrc
#末尾添加：
export LD_LIBRARY_PATH={自己地址}/TensorRT-7.1.3.4/lib${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export PATH={自己地址}/TensorRT-7.1.3.4/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH={自己地址}/TensorRT-7.1.3.4/lib/:$LD_LIBRARY_PATH
# ctrl + s -- ctrl +x 保存退出
source ~/.bashrc
#检验一下
trtexec #不会报错
```

4. 安装其他有用的库
```
#polygraphy
python -m pip install colored polygraphy --extra-index-url https://pypi.ngc.nvidia.com

#onnxruntime 根据官网选择版本 
#https://onnxruntime.ai/docs/execution-providers/CUDA-ExecutionProvider.html
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple onnxruntime==1.8.0
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple onnxruntime-gpu==1.8.0
```

5. 安装pycuda(optional)
https://blog.csdn.net/blueblood7/article/details/117419608

## 6.根据**config_environment\sky黑客松-知识图谱-2022q2.pdf**安装后续环境




# 安装opencv：
https://cloud.tencent.com/developer/beta/article/1657529
