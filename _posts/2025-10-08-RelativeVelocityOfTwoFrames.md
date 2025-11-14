---
title: Relative Velocity of Two Frames
date: 2025-10-08 14:20:00 +0800
categories: [Robot]
tags: [Robot]
author: ecstayalive
comments: true
toc: true
math: true
description:
---

## Introduction
Suppose there is an inertial coordinate system $\mathcal{I}$ and two moving coordinate systems, one is $\mathcal{A}$ and the other is $\mathcal{G}$. Suppose we know that the angular velocity and linear velocity of these two moving coordinate systems relative to the inertial coordinate system, that are $^\mathcal{I}v_\mathcal{A}$, $^\mathcal{I}\omega_\mathcal{a}$, $^\mathcal{I}v_\mathcal{g}$ and $^\mathcal{I}\omega_\mathcal{g}$ respectively. And the transformation matrices of the two relative to the inertial coordinate system $\mathcal{I}$ at this moment are known, that are $^\mathcal{I}\_\mathcal{A}T$ and $^\mathcal{I}_\mathcal{G}T$. So how to calculate the relative linear velocity and the relative angular velocity of $\mathcal{G}$ relative to the $\mathcal{A}$ coordinate system?

## For the linear velocity
Observing the origin of the $\mathcal{G}$ coordinate system in the $\mathcal{I}$ coordinate system, we have:

$$
{^\mathcal{I}O_\mathcal{G}} = {^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}P_\mathcal{G}} + {^\mathcal{I}O_\mathcal{A}}
$$

Calculate the derivative of both sides simultaneously, we have:

$$
\dot{^\mathcal{I}O_\mathcal{G}} = \dot{^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}P_\mathcal{G}} + {^\mathcal{I}_\mathcal{A}R} \dot{^\mathcal{A}P_\mathcal{G}} + \dot{^\mathcal{I}O_\mathcal{A}}
$$

$$
{^\mathcal{I}v_\mathcal{g}} = [^\mathcal{I}\omega_\mathcal{a}] {^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}P_\mathcal{G}} + {^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}v_\mathcal{g}} + {^\mathcal{I}v_\mathcal{A}}
$$

$$
{^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}v_\mathcal{g}} = {^\mathcal{I}v_\mathcal{g}} - {^\mathcal{I}v_\mathcal{A}} - [^\mathcal{I}\omega_\mathcal{a}] {^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}P_\mathcal{G}}
$$

$$
{^\mathcal{A}v_\mathcal{g}} = {^\mathcal{I}_\mathcal{A}R}^T ({^\mathcal{I}v_\mathcal{g}} - {^\mathcal{I}v_\mathcal{A}} - [^\mathcal{I}\omega_\mathcal{a}] {^\mathcal{I}_\mathcal{A}R} {^\mathcal{A}P_\mathcal{G}})
$$

## For the angular velocity
For the rotation matrix of two coordinate systems, we have:

$$
{^\mathcal{I}_\mathcal{G}R} = {^\mathcal{I}_\mathcal{A}R} \cdot {^\mathcal{A}_\mathcal{G}R}
$$

Calculate the derivatives of the both sides:

$$
\dot{^\mathcal{I}_\mathcal{G}R} = \dot{^\mathcal{I}_\mathcal{A}R} \cdot {^\mathcal{A}_\mathcal{G}R} + {^\mathcal{I}_\mathcal{A}R} \cdot \dot{^\mathcal{A}_\mathcal{G}R}
$$

$$
[^\mathcal{I}\omega_\mathcal{g}] {^\mathcal{I}_\mathcal{G}R} = [^\mathcal{I}\omega_\mathcal{a}] {^\mathcal{I}_\mathcal{A}R} \cdot {^\mathcal{A}_\mathcal{G}R} + {^\mathcal{I}_\mathcal{A}R} [^\mathcal{A}\omega_\mathcal{g}] {^\mathcal{A}_\mathcal{G}R}
$$

$$
\begin{aligned}
[^\mathcal{I}\omega_\mathcal{g}] &= [{^\mathcal{I}\omega_\mathcal{a}}] + {^\mathcal{I}_\mathcal{A}R} \cdot [^\mathcal{A}\omega_\mathcal{g}] \cdot (^\mathcal{I}_\mathcal{A}R)^T \\
&= [{^\mathcal{I}\omega_\mathcal{a}}] +  [{^\mathcal{I}_\mathcal{A}R}^\mathcal{A}\omega_\mathcal{g}]
\end{aligned}
$$

$$
^\mathcal{A}\omega_\mathcal{g} = ({^\mathcal{I}_\mathcal{A}R})^T \cdot({^\mathcal{I}\omega_\mathcal{g}} - {^\mathcal{I}\omega_\mathcal{a}})
$$
