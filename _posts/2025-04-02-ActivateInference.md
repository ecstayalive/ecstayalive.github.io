---
title: Activate Inference
date: 2025-04-02 16:36:00 +0800
categories: [AI, Activate Inference]
tags: [AI]
author: ecstayalive
comments: true
toc: true
math: true
description: A post with a brief introduction to Activate Inference
---

# Activate Inference

## Jensen's inequality

If $\lambda_1$ and $\lambda_2$ are two arbitrary nonnegative real numbers such that $\lambda_1 + \lambda_2 = 1$, then convexity of $\phi$ implies:

$$\forall x_1, x_2: \phi(\lambda_1 x_1 + \lambda_2 x_2) \le \lambda_1 \phi(x_1) + \lambda_2 \phi(x_2)$$

This can be generalized: if $\lambda_1, ... , \lambda_n$ are nonnegative real numbers such that $\sum_{i=1}^{n} \lambda_i = 1$, then:

$$\phi \left(\sum_{i=1}^{n}\lambda_i x_i \right) = \sum_{i=0}^{n} \lambda_i \phi(x_i)$$

for $\forall x_1, ...., x_n$.
The finite form of the Jensen's inequality can be proved by induction: by convexity hypotheses, the statement is true for $n=2$. Suppose the statement is true for some $n$, so

$$\phi \left(\sum_{i=1}^{n}\lambda_i x_i \right) = \sum_{i=0}^{n} \lambda_i \phi(x_i)$$

for $\forall \lambda_1,..., \lambda_n$such that $\lambda_1 + ....+ \lambda_n = 1$
One needs to prove it for $n+1$. At least one of the $\lambda_i$is strictly smaller than 1, say $\lambda_{n+1}$;therefore by convexity inequality:

$$\begin{align}
\phi \left( \sum_{i=1}^{n+1}\lambda_i x_i \right) &= \phi\left( (1-\lambda_{n+1})\sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}x_i + \lambda_{n+1} x_{n+1} \right) \\ &\le (1-\lambda_{n+1})\phi\left( \sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}x_i \right) + \lambda_{n+1}x_{n+1}
\end{align}$$

Since $\lambda_1 + ... + \lambda_{n+1} = 1$,

$$\sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}=1$$

applying the inductive hypothesis gives

$$\phi \left( \sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}x_i \right) \le \sum_{i=0}^n \frac{\lambda_i}{1-\lambda_n} \phi(x_i)$$

therefore

$$\begin{align}
\phi \left( \sum_{i=1}^{n+1}\lambda_i x_i \right) &= \phi\left( (1-\lambda_{n+1})\sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}x_i + \lambda_{n+1} x_{n+1} \right) \\ &\le (1-\lambda_{n+1})\phi\left( \sum_{i=1}^n\frac{\lambda_i}{1-\lambda_{n+1}}x_i \right) + \lambda_{n+1}x_{n+1} \\ &\le (1-\lambda_{n+1}) \sum_{i=0}^n\frac{\lambda_i}{1-\lambda_{n+1}} \phi(x_i) + \lambda_{n+1} x_{n+1}
\\&= \sum_{i=1}^{n+1} \lambda_i \phi(x_i)
\end{align}$$

We deduce the inequality is true for $n+1$, by induction it follows that the result is also true for all integer $n$ greater than 2. In the context of probability theory, it is generally stated in the following form: if $x$ is a random variable and $\phi$ is a convex function, then

$$\phi \left( \mathit{E}(x) \right) = \phi\left( \int p(x)x dx \right) \le  \int p(x) \phi\left(x\right)dx = E\left[ \phi(x) \right]$$

## Variational Inference

$$
\begin{align}
\ln p(o) &= ln \int p(o|s) p(s) ds \\
&= \ln \left[ \int \frac{p(o,s) q(s)}{q(s)}ds \right] \\
&= \ln \left[ \mathbb{E}_{q(s)}\left[ \frac{p(o,s)}{q(s)} \right] \right]\\
&\ge \mathbb{E}_{q(s)} \left[ \ln(\frac{p(o,s)}{q(s)}) \right]
\end{align}
$$

$$
\begin{align}
\mathbb{E}_{q(s)} \left[ \ln(\frac{p(o,s)}{q(s)}) \right] &= \mathbb{E}_{q(s)} \left[ \ln(\frac{p(o|s) p(s)}{q(s)}) \right]  \\
&= \mathbb{E}_{q(s)} \left[ \ln(\frac{p(s|o) p(o)}{q(s)}) \right]
\end{align}
$$

$$
\begin{align}
\mathbb{E}_{q(s)} \left[ \ln(\frac{p(o|s) p(s)}{q(s)}) \right] &= -D_{KL}(q(s) || p(s)) + \mathbb{E}_{q(s)} \left[ \ln(p(o|s)) \right]
\end{align}
$$

$$
\begin{align}
\mathbb{E}_{q(s)} \left[ \ln(\frac{p(s|o) p(o)}{q(s)}) \right] &= -D_{KL}(q(s) || p(s|o)) + \mathbb{E}_{q(s)} \left[ \ln(p(o)) \right] \\
&= -D_{KL}(q(s)||p(s|o)) + \int p(s) \ln(p(o)) ds \\
&= -D_{KL}(q(s)||p(s|o)) + \ln(p(o))
\end{align}
$$

$$
-D_{KL}(q(s)||p(s|o)) + \ln p(o) = -D_{KL}(q(s) || p(s)) + \mathbb{E}_{q(s)} \left[ \ln(p(o|s)) \right] \\
$$

$$
\begin{align}
\ln p(o) &= -D_{KL}(q(s) || p(s)) + \mathbb{E}_{q(s)} \left[ \ln(p(o|s)) \right] + D_{KL}(q(s)||p(s|o)) \\
&= -F + D_{KL}(q(s)||p(s|o))
\end {align}
$$
We wish for the state estimated from observations to resemble our prior estimation state as closely as possible, meaning $D_{KL}(q(s)||p(s|o))$ should be minimised. Within a dataset, the probability $p(o)$ is constant; thus, when the KL divergence is minimised, the problem transforms into one of minimising free energy $F=D_{KL}(q(s) || p(s)) - \mathbb{E}_{q(s)} \left[ \ln(p(o|s)) \right]$. This is variational inference: converting a difficult-to-optimise problem into one that can be optimised.

## Activate Inference and World Model

$$
\begin{align}
-G(π) &\triangleq D_{KL} \left[ q(o,s|\pi) || p(o,s) \right] \\
&= \mathbb{E}_{q(o, s|\pi)} \left[ ln\frac{q(o, s|\pi)}{p(o, s)} \right] \\
&=  \mathbb{E}_{q(o, s|\pi)} \left[ \ln\left[q(o, s|\pi)\right] - \ln \left[ p(o, s) \right]  \right] \\
&= \mathbb{E}_{q(o, s|\pi)} \left[ \ln \left[ q(s|\pi) \right] - \ln \left[ p(o, s) \right] +\ln\left[q(o|s,\pi)\right] \right]\\
&= \mathbb{E}_{q(o, s|\pi)} \left[ \ln \left[ q(s|\pi) \right] - \ln \left[ p(o) \right] - \ln \left[ p(s|o) \right] +\ln\left[q(o|s,\pi)\right] \right]
\end{align}
$$

According to active inference, creatures infer actions based on the minimisation of expected free energy. The below is the original free energy formula.

$$
G(\pi, s_0) = \sum_{t=0}^{N} \left[ \mathbb{E}_{q(o_{t+1}|s_{t+1}) q(s_{t+1}|s_t, a_t)} \left[ \ln \frac{q(s_{t+1}|o_{\le {t+1}}, a_{\le t})}{q(s_{t+1}|s_t, a_t)} \right] + \mathbb{E}_{q(r_{t+1}|s_{t+1}) q(s_{t+1}|s_t, a_t)} \ln p(r_{t+1}) \right]
$$

We note that when this problem is extended to an infinite time domain, the objective becomes:

$$
G(\pi, s_0) = V_{\pi}(s_0) = \sum_{t=0}^{\infty} \left[ \gamma^{t} \mathbb{E}_{s_{t}(t\ge1), a_{t}} \left[ g(s_{t}, a_{t}) \right] \right] \\
$$

$$
\begin{align}
g(s_{t}, a_{t}) &= \mathbb{E}_{q(o_{t+1}|s_{t+1}) q(s_{t+1}|s_t, a_t)} \left[ \ln \frac{q(s_{t+1}|o_{\le {t+1}}, a_{\le t})}{q(s_{t+1}|s_t, a_t)} \right] + \mathbb{E}_{q(r_{t+1}|s_{t+1}) q(s_{t+1}|s_t, a_t)} \ln p(r_{t+1}) \\
&= \mathbb{E}_{q(o_{t+1}|s_t, a_t)} \left[ \ln \frac{q(s_{t+1}|o_{\le {t+1}}, a_{\le t})}{q(s_{t+1}|s_t, a_t)} \right] + \mathbb{E}_{q(r_{t+1}|s_t, a_t)} \ln p(r_{t+1})
\end{align}
$$

In this context, $G(\pi, s_0)$ comprises two components: one is epistemic value, which drives the agent to search for actions yielding the greatest internal state information gain. The other component is extrinsic value (reward), incentivising the agent to perform specific tasks to obtain greater rewards. $p$ denotes the expected reward probability distribution. Clearly, $G$ is a policy-dependent function, similar to the value function $V$, requiring sampling and fitting using the current policy $\pi_{now}$. Of course, we can also train it using the [SAC](https://arxiv.org/abs/1801.01290) approach.

$$
G_{\pi}(s_0, a_0) = Q_{\pi}(s_0, a_0) = g(s_0, a_0) + \mathbb{E}_{q(s_1|s_0, a_0)}[V_{\pi}(s_1)]
$$

The employment of the Q-function enables the avoidance of the reconstruction of observations, thereby accelerating the training speed. Consequently, a Q-function-based training method will be introduced here.


It is imperative that an alternative approach to optimisation is adopted. In active inference, the reinforcement learning optimisation problem is transformed into a inference problem. In other words, the preceding optimisation problem – namely, the action to be taken in order to maximise the expected cumulative reward – is thus rendered: knowing the existence of a successful state that maximises the expected cumulative reward, what action is most likely to have been taken? This view has also been used in [MPO](https://arxiv.org/abs/1806.06920)

However, relying solely on $G(\pi, s_0)$ does not appear to achieve learning as efficient as that of biological organisms. I suspect one reason is that, although we select actions based on free energy, we do not actively seek higher-value states. One example is that when undertaking tasks, we contemplate the future target state and then derive a reasonable course of action based on the current state. For state $s_t$ and our generative model, we believe it can reach any state after a finite number of time steps. One objective of the actor is to find a suitable path along this trajectory. Particularly, if we possess a structured world model, it seems unreasonable not to actively seek out states within this structured space(We use RSSM currently).One question is how we should incorporate this cost into our objective function? And how should we design this architecture?

## RL as Inference

## How creature learn and act in the world?

## A Method
