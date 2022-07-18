# 2022_NVIDIA_sky_hackathon_replay
**2022_NVIDIA_sky_hackathon复盘**\
&emsp;&emsp;2022_NVIDIA_sky_hackathon比赛主要分为三个任务：1.语音识别2.目标检测3.语音合成。具体场景为：下班回到小区，在校区门口与自助机器人大白进行对话：“你好大白，我是xx，请让我进入小区。”大白机器人识别到语音后，对目标进行检测，判断目标是否正确佩戴、是否佩戴口罩。若正确佩戴口罩，则通过语音合成出“你好xx，欢迎回家。”\
&emsp;&emsp;该项目利用了NVIDIA开发的NEMO组件和TAO组件，尽可能的达到zero_coding的效果。\
&emsp;&emsp;最后，该项目在jetson nano上进行了部署。

**目录**\
**1.安装环境**\
**2.ASR**\
**3.TTS**

## 1.安装环境
按照“sky黑客松-知识图谱-2022q2.pdf”进行环境的配置
