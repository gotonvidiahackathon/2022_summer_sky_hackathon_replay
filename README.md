# 2022_NVIDIA_sky_hackathon_replay
**2022_NVIDIA_sky_hackathon复盘**\
&emsp;&emsp;2022_NVIDIA_sky_hackathon比赛主要分为三个任务：1.语音识别2.目标检测3.语音合成。具体场景为：下班回到小区，在校区门口与自助机器人大白进行对话：“你好大白，我是xx，请让我进入小区。”大白机器人识别到语音后，对目标进行检测，判断目标是否正确佩戴、是否佩戴口罩。若正确佩戴口罩，则通过语音合成出“你好xx，欢迎回家。”\
&emsp;&emsp;该项目利用了NVIDIA开发的NEMO组件和TAO组件，尽可能的达到zero_coding的效果。\
&emsp;&emsp;最后，该项目在jetson nano上进行了部署。\
&emsp;&emsp;由于时间有限以及主次分明的考虑，比赛的侧重点是让参与者体会模型从训练--剪枝--部署的一套流程，而不必过分拘泥于每个模型的泛化能力。语音识别部分允许将测试的语音进行训练，因此我们队及其他几乎每个队都拿到了满分。在口罩检测部分，比赛中baseline利用Tao组件进行zero_coding，可以实现Mobilenet、ResNet等经典backbone+ssd的检测算法，并正确部署到jetson nano上。但根据ken老师说:\
&emsp;&emsp;“是这样的，我们的nano上如果你要用yolo那就需要batchedNMS的plugin， 所以你就需要trt_oss的8.0+版本。本来升级也无所谓，但是升级了之后你的nemo可能就会运转出现问题;所以，我建议你如果还想用yolo，那么就用别的框架训练，然后在nano上转换生成engine”\
&emsp;&emsp;此外，tts部分想要生成一条好的语音，也需要掌握一些训练策略。\
&emsp;&emsp;因此，本项目的复盘，主要集中于两点：
1. yolov5网络的部署
2. tts训练策略改进

**目录**\
**1.安装环境**\
**2.ASR**\
**3.TTS**

## 1.安装环境
[**ubuntu18.04+cudatoolkit11.0+cudnn8.0+tensorrt7.1**](https://github.com/gotonvidiahackathon/2022_summer_sky_hackathon_replay/blob/main/config_environment/config_environment.md)

由于没钱买jetson nano，只能在装系统的时候模拟jetson nano的环境。本次比赛中nano的tensorRT环境为7.1.3,ubuntu最高支持18.04，最终环境为：\
**ubuntu18.04+cudatoolkit11.0+cudnn8.0+tensorrt7.1**\
\
装双系统：
https://www.jianshu.com/p/08a4f1a36c54 \
wsl2：
###