用docker一键从NGC官网拉取tensorRT环境\
\
by 宋尧哲

在配置好linux系统（双系统/wsl2）后，可以通过在NGC官网一键拉取docker得到想要的tensorRT环境。

1. 进入[NGC官网](https://catalog.ngc.nvidia.com/),搜索TensorRT
2. 点击第一个，进入TensorRT
3. 在[这个网站](https://docs.nvidia.com/deeplearning/frameworks/support-matrix/index.html#framework-matrix-2020)中找到想要的TensorRT对应的日期版本
4. 在第二步的网站中点击Tags，找到要下载的版本，TensorRT7.1.3对应的日期为20.09和20.08，选择了20.08,找到它后点击pull Tag，会复制一个指令

5. 在linux终端，sudo {复制指令}，我的是
```
sudo service docker start #开启docker
sudo docker pull nvcr.io/nvidia/tensorrt:20.08-py3
```
6. 拉取后本地创建一个文件夹对docker进行绑定
```
mkdir trt7.1.3
nvidia-docker run -it --name trt7.1.3 -v ~/trt7.1.3:/target nvcr.io/nvidia/tensorrt:20.08-py3
pip list #可以看到tensorRT版本是7.1.3.4
```
7. 不是第一次进入，需要简化一下。
```
exit #退出docker
cd trt7.1.3
nano docker.sh
# 在弹出的窗口中输入
sudo service docker start
sudo nvidia-docker start -i trt7.1.3
# ctrl + s -- ctrl + b 保存退出
# 以后只要进入trt7.1.3文件夹后 sh docker.sh 就能进入docker
```