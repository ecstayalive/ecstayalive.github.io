---
title: Motion Imitation
date: 2025-04-15 14:19:00 +0800
categories: [AI, Robot]
tags: [AI]
author: ecstayalive
comments: true
toc: true
math: true
description: A methodological study of humanoid robots that imitate human motion
---
# Motion Imitation

## Survey

### 宇树官方开源kinect_teleoperate
[kinect_teleoperate](https://github.com/unitreerobotics/kinect_teleoperate)
该项目能够实现人体动作到宇树h1的动作映射。该项目使用Azure Kinect相机。Azure Kinect SDK包含两个部分，一个部分是camera SDK，用于获得视觉输入，一个部分是body tracking SDK用于身体骨架检测。这两个SDK配置具体可以看[Readme文档](https://github.com/unitreerobotics/kinect_teleoperate/blob/main/README.md)。下面是一个demo:
![demo](assets/img/motion_imitation/teleoperate.jpg)
这个程序实现了对H1机器人双手臂的遥控功能，其中，单个手臂控制范围包括4个自由度：包括肩部俯仰关节、肩部滚动关节、肩部偏航关节和肘部俯仰关节。因此，整个系统可以同时控制双臂共 8 个自由度。同时官方视频演示中仅仅展示了遥操unitree h1机器人的demo，由于unitree g1机器人身体比例与人不一样，因此，该项目是否能够迁移到g1机器人，以及迁移到g1机器人上效果均保持疑问。
[官方文档](https://support.unitree.com/home/zh/Teleoperation/kinect_teleoperate)

### FLAM
该项目全称是[Foundation Model-Based Body Stabilization for Humanoid Locomotion and Manipulation](https://xianqi-zhang.github.io/FLAM/)。目前强化学习是人形机器人控制的主要方法，但是之前的方法仅仅依赖task rewards，使得控制器对人形机器人的平衡控制效果不好。这篇论文提出了FLAM系统，该系统引入一个stabilizing reward function，使得控制器能够有效的学习人形机器人稳定的姿态。该系统具体流程如下：首先将机器人位姿映射到一个虚拟的3D人体模型。接着，通过一个motion reconstruction model去稳定与重建当人体位姿。紧接着使用重建之前的位姿与重建之后的位姿来计算稳定姿态奖励。具体流程如下：
![flam_pipeline](assets/img/motion_imitation/flam_pipeline.png)
_The pipeline of flam system_
这个idea听起来不错，但是没有实物实验只有仿真实验，并且运动效果看起来不算太好。和我们的要求也不太一样，我们的需求不要求执行复杂的操作任务，而是人形机器人能够模仿人类的上肢动作。

### I-CTRL
[I-CTRL: Imitation to Control Humanoid Robots Through Constrained Reinforcement Learning](https://evm7.github.io/I-CTRL/)

仿人机器人具有以高视觉保真度模仿人类动作的潜力，但将这些动作转化为实际的物理执行仍是一项重大挑战。图形学领域的现有技术往往优先考虑视觉保真度，而不是基于物理的可行性，这对在实际应用中部署双足系统构成了巨大挑战。本文通过引入一种约束强化学习算法来解决这些问题，该算法可在腿部仿人机器人上生成基于物理的高质量运动模仿，在成功遵循参考人体轨迹的同时增强运动相似性。[I-CTRL](https://evm7.github.io/I-CTRL/)将运动模仿重新表述为对非基于物理的重定向运动的约束精炼。I-CTRL在动作模仿方面表现出色，其奖励简单独特，适用于四种机器人。此外，我们的框架还能通过独特的 RL 代理跟踪大规模运动数据集。所提出的方法标志着在推进双足机器人控制方面迈出了关键的一步，强调了将视觉和物理逼真性结合起来以成功实现运动模仿的重要性。
![i_ctrl_pipeline](assets/img/motion_imitation/i_ctrl.png)
_The pipeline of I-CTRL system_
尽管效果看起来不错，但是作者没有放出源代码，并且没有实物试验验证方法的可迁移性。

### Exbody2
[Exbody2:Advanced Expressive Humanoid Whole-Body Control](https://exbody2.github.io/)

这篇文章使人形机器人能够在像人类一样做出富有表现力的动作。这篇论文提出的Exbody2是一个通用的全身跟踪框架，它可以接受任何参考运动输入，并控制仿人机器人模仿该运动。该模型通过强化学习进行模拟训练，然后sim2real到现实世界中。该论文将关键点跟踪与速度控制分离开来，并有效地利用teacher-student结构，将精确的模仿技能提炼到目标学生策略中，从而能够高保真地复制跑步、蹲下、跳舞等动态动作以及其他具有挑战性的动作。这篇论文在两个人型机器人上进行了实验，展示了众多动作，证明了方法的有效性。
{% include embed/video.html src='assets/video/motion_imitation/exbody2_demo1.mp4' %}

这篇论文效果看起来不错，感觉具有复现价值。
