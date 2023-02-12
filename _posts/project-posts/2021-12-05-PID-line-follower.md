---
title: PID Line Following Robot
date: 2021-12-05 00:00:00 -10
categories: []
tags: []
img_path: /assets/project-img/2021-12-05-PID-line-follower/
toc: true
comments: false
math: true
---

A classic undergraduate project one may build in an embedded systems, mechatronics, or robotics course is a line following robot based on PID control. This ties together a lot of key ideas in mechanical, electrical, and computer engineering and is a good exercise in cross-functional design that one encounters in industry. Here I describe a line following robot system I built for my undergraduate mechatronics final project. This project is an Arduino based _PID_ (Proportional Integral Derivative) control line following. The robot is designed to complete a determinisitic obstacle course.

![Robot Assembled](assemble_view2.jpg){: width="450" height="500" }
_**Figure 1** - The fully assembled robot!_



# Overview

This project was designed to deterministically complete an obstacle course with the below constraints:

1. The robot must autonomously complete a sequence of tasks on the designed course. The course cannot be modified in any way.
2. There is a starting point near the right-center of the course where the black line starts. Your robot needs to start in that area.
- First you must navigate through the corridor of the first order base. (watch out for the two garbage chutes). One possible path is marked with 1” wide black tape. Your robot must move through this corridor.
- At the end of this corridor, four stormtroopers are standing guard. You must knock them down. If they get back up – run.
- After you pass the guard station, there is an unmarked passage to the armory on your left. When you enter the armory, you will find 5 light sabers mounted to the back wall. Two will be captured Jedi light sabers. These glow green. Three are Sith light sabers. These glow red. If you are endowed with the Force, you may press the handle of a Sith light saber, and it will turn from red to green. If you press the handle of a Jedi light saber, you will send it back to the dark side. You must get all 5 light sabers to glow green.
- Once all 5 light sabers are green, you must escape the armory. No part of your robot can remain in the room. The light sabers will take control of the ship and destroy all evil aboard. They may look strange while this is happening.
3. You can backup and retry as much as you want.
4. Teams will be given several runs on competition day. Best run counts.

If you couldn't tell, the theme for this course is Star Wars! Now let's get into the details on the theory and design aspects of this design.


# System Structure & Operation

![Block Diagram](PID-line-follower.png){: width="650" height="4000" }
_**Figure 2** - Block diagram of the designed PID line follower robot. Grey blocks indicate control elements, while blue blocks indicate sensing elements._

## Robot Assembly

![Robot Mehcanical](chassis_mechanical.png){: width="650" height="4000" }
_**Figure 3** - Mechanical assembly drawing of the robot chassis. Dimenions are optimized for component placement and also the position of the wheels with respect to the axis of rotation (required for achieving stable PID control)._

# PID Line Following

## What is PID Control?

![PID Control Block](PID_control_block.png){: width="500" height="4000" }
_**Figure X** - A block diagram of a PID controller in a feedback loop. r(t) is the desired process value or setpoint (SP), and y(t) is the measured process value (PV) [^PID_wiki]._

$$
u(t)= K_p e(t) + K_i \int_{0}^{t} e(\tau)\partial\tau + K_d \frac{\partial e(t)}{\partial t}
 \tag{1}
$$

$$
e(t) = \textrm{error} = \textrm{Desired Setpoint} - \textrm{Measured Process Value} = r(t) - y(t)
\tag{2}
$$

The __proportional control term__ mostly controls the response time of the control loop. Strictly speaking, a larger vlaue of $$K_p$$ means the response time will be faster. This term is _proportional_ to the current measured setpoint error. A large measured error means a large positive response term will be applied to the system control to bring the measured process value closer to the setpoint such that the error residual is zero. The speed at which this occurs depends on the loop bandwidth of the system which is related to the value of $$K_p$$. Note that $$K_p$$ must be set to achieve a specific damping, while maintaining control loop stability. 

The __integral control term__ is used to improve the steady state response and eliminate steady state error (over time). So the goal of the integral term is just to eliminate residual errors in the steady state. Some benefits of integral control:
- Small errors are integrated over time to establish sizeable control.
- Integral control term dominates the steady state behavior is $$K_p$$ is small and $$K_d$$ is zero.

Some challenges of integral control are:
- Computing the integral term.
- Integral "windup" - condition where a large change in setpoint occurs and the integral term accumulates sufficient error during the rise (windup), thus overshooting and continuing to increase as this accumulated error is unwound [^windup].
- Can lead to oscillations/instability at high $$K_i$$.

The __derivative control term__ is used to improve the transient response, specificially to eliminate overshoot. The derivative term is mainly used to best esimtat the future trend of the setpoint/process-value error based on the current rate of change of the system. The more rapid the change in the process value, the greater the controlling effect of the derivative term. Some benefits of the derivative term:
- Derivative of the error term reduces the control loop effort as the process value approaches the setpoint.

Some challenges of derivative control:
- Computing the derivative.
- The derivative control is prone to being impacted by noise coupled into the system observables.
- The derivative term can lead to instability easily in the case of dramatic changes in setpoint.


## PID Code Structure

### PID Tuning

{% include embed/youtube.html id='AFce0tXYSrY' %}

# Obstacle Course

## Detecting Obstacles

## Objectives

## State Transitions

## Demo

{% include embed/youtube.html id='SOCrXqQRcyQ' %}
This robot ended up scoring the fastest amongst the competition in the field with a completion time of 43s!





# Bibliography

[^PID_wiki]: [Wikipedia] https://en.wikipedia.org/wiki/PID_controller

[^windeup]: [Wikipedia] https://en.wikipedia.org/wiki/Integral_windup

