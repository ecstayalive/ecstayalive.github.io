---
title: Physics Feasibility Guided Optimization
date: 2025-04-04 16:36:00 +0800
categories: [AI, DeepReinforcementLearning]
tags: [AI]
author: ecstayalive
comments: true
toc: true
math: true
description: Efficient Learning of A Unified Policy For Whole-body Manipulation and Locomotion Skills
---

# Efficient Learning of A Unified Policy For Whole-body Manipulation and Locomotion Skills

![](assets/img/physics_feasibility_guided_optimization/ZhejiangUniversityLogotype.svg){: .normal width="220" height="80" } ![](assets/img/physics_feasibility_guided_optimization/UniversityCollegeLondonLogo.svg){: .normal width="300" height="80" } ![](assets/img/physics_feasibility_guided_optimization/iros2025logo.png){: .normal width="200" height="80"} ![](assets/img/physics_feasibility_guided_optimization/AprilLogo.png){:.normal width="400" height="250" } ![](assets/img/physics_feasibility_guided_optimization/StateKeyLogo.png){: .normal width="200" height="80"}

<big>**Dianyong Hou, Chengrui Zhu, Zhen Zhang, Zhibin Li, Chuang Guo, and Yong Liu**</big>

<style>
.gradient-btn {
  display: inline-block;
  padding: 10px 20px;
  margin: 6px 0;
  background: linear-gradient(135deg, #3b82f6, #818cf8) !important;
  border: none;
  border-radius: 24px;
  color: white !important;
  font-weight: 600;
  font-size: 14px;
  text-decoration: none !important;
  transition: all 0.3s ease;
  box-shadow: 0 3px 12px rgba(59, 130, 246, 0.3);
  position: relative;
  overflow: hidden;
  z-index: 1;
  background-clip: padding-box !important;
  -webkit-background-clip: padding-box !important;
}
.gradient-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 18px rgba(59, 130, 246, 0.4);
  background: linear-gradient(135deg, #2563eb, #6366f1) !important;
  color: white !important;
  text-decoration: none !important;
}
.gradient-btn:active {
  transform: translateY(0);
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.5);
}
.gradient-btn:hover::after,
.gradient-btn:active::after {
  content: none !important;
}
.gradient-btn::before {
  content: "";
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  transition: all 0.6s ease;
  z-index: 2;
  pointer-events: none;
}
.gradient-btn:hover::before {
  left: 100%;
}
</style>
<center>
  <a href="#" class="gradient-btn">
    Paper
  </a>
</center>

## Abstract

Equipping quadruped robots with manipulators provides unique loco-manipulation capabilities, enabling diverse practical applications. This integration creates a more complex system that has increased difficulties in modeling and control. Reinforcement learning (RL) offers a promising solution to address these challenges by learning optimal control policies through interaction. Nevertheless, RL methods often struggle with local optima when exploring large solution spaces for motion and manipulation tasks. To overcome these limitations, we propose a novel approach that integrates an explicit kinematic model of the manipulator into the RL framework. This integration provides feedback on the mapping of the body postures to the manipulator's workspace, guiding the RL exploration process and effectively mitigating the local optima issue. Our algorithm has been successfully deployed on a DeepRobotics X20 quadruped robot equipped with a Unitree Z1 manipulator, and extensive experimental results demonstrate the superior performance of this approach.

## Methodology

![](assets/img/physics_feasibility_guided_optimization/iros2025_framwork.svg)
_Reinforcement learning framework for whole-body loco-manipulation_

To address this problem, we define a feasible state function\eqref{eq:feasible_states1} to map the relationship between the quadruped robot’s motion and the manipulator’s operational workspace. Specifically, given a target end-effector pose $^WT \in SE(3)$ in the world frame and the robot’s torso state $s_{torso}$, we consider $s_{torso}$ feasible if there exists a valid manipulator joint configuration $q_{arm}\in \Theta_{arm}$ such that $FK(s_{torso},q_{arm}) = {^WT}$, where $FK$ denotes the forward kinematics model of the legged manipulator and $\Theta_{arm}$ is the manipulator's joint configuration domain. When $FS(s_{torso}, {^WT}) =1$, the torso pose $s_{torso}$ is deemed to provide kinematic feasibility for the manipulator to eventually reach the target pose, even if not yet achieved physically. By rewarding such feasible torso states during training, the algorithm guides RL policy toward discovering collaborative motor skills more efficiently in early exploration stages. We refer to this method as the physical feasibility-guided(PFG) reward design method. The framework diagram of our training method is shown above.

$$
\begin{align}
\boldsymbol{FS}(s_{torso}, ^WT) &= \begin{cases}
1, & \text{if } \exists q_{arm}\in\Theta_{arm},
\\&\text{s.t. } FK(s_{torso},q_{arm})=^WT \\
0, & \text{otherwise}
\end{cases}\label{eq:feasible_states1}
\\&=\begin{cases}
1, & \text{if } \exists q_{arm}\in\Theta_{arm},
\\&\text{s.t. } FK_{arm}(q_{arm})=^BT(s_{torso}) \\
0, & \text{otherwise}
\end{cases} \label{eq:feasible_states2}
\end{align}
$$

## Experiments

{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/0.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/1.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/2.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/4.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/5.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/6.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/7.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/9.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/10.mp4' %}
{% include embed/video.html src='assets/video/physics_feasibility_guided_optimization/11.mp4' %}

## License

This post is powered by the Authors and the Zhejiang University April Lab.

## Questions && Answers

### Did we have to train more for pushing a cart? The Dynamics and Reaction forces changes in this task, were they modeled in reward design.

We did not train more for a specific task, and the dynamics of the robot's interaction with the environment were not modeled using rewards. In our training, there is a collision reward that encourages the robot to reduce body as well as manipulator collisions with the environment as well as with itself. Also, during training, domain randomization involves applying a random force at the end-effector of the manipulator and the torso.

### Is there a limit of the force that you can apply to the environment before this becomes a problem for the controller? Or is the limiting factor what the payload of the robot arm can be?

In the current system, the task of the manipulator is limited to 6d pose tracking in 3D space and does not include force control tasks. As a result, the stiffness of the entire arm is completely determined by the kp parameters of its joints. This property constrains both the force exerted by the manipulator on the environment and its load capacity. When the arm is subjected to an external force, its deviation from the target pose increases as the external force increases. Nevertheless, our system is robust enough: even if a very large external force causes the arm to deviate from the target, the system does not crash, but only stops when the arm enters damping mode due to overheating.

### What behavior of the robot does the paper mean by local optimum? And what is the behavior of the manipulation at the singularity?

A example of "local optimum" can be shown when the arm is at its home position: if the target pose is shifted slightly downward, the robot won't move the arm but instead tilts its body to reduce the distance (as demonstrated in our video). Similarly, when instructed to move at specific body velocities, the arm remains static while the quadruped robot maintains tilted motion. Although the arm maintains stability near singularities, its movement becomes more fast when the target pose changes near singular points.

### Does body movement affect how well the manipulator tracks?

Since the given command for the manipulator is relative to the arm's base coordinate system (though our implementation also supports another coordinate system), we must transform the target pose from the world coordinate system to the arm's base frame when performing tasks in world coordinates. At this point, the speed of the body can affect tracking performance. If the frequency of this coordinate transformation is too low, the arm will struggle to track the target effectively. However, when the arm is directly controlled via a joystick (providing commands in its base frame), body speed has a much smaller impact on performance.
