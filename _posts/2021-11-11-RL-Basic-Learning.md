---
title: RL Basic Learning
date: 2021-11-11 19:47
description: to chase that brilliant
comments: true
mathjax: true
categories:
- learning-notes
tags:
- reinforcement-learning
---

# RL -Learning

## Defination of somehow

### Introduction

It's an inclusion of a certain kind of algorithm.

![强化学习是机器学习的学习方法之一](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-rl-position.png)

To quickly define some concepts: [3]

There is an entity in the environment, We don't know the reactions the environment do to the entity is based on **models** or not. The entity has two key arguments: its **states(s∈S)** and **actions(a∈A)**. The entity will transform its states through actions and once the action was taken, the environment will delivers a **reward(r∈R)** as feedback. Which states will the entity reach is decided by the possibilities between states(P).

### Classification [1]

The mainstream algorithm are Model-Free and Model-Based. The main difference of the two is that **can the AI entity get a full vision of its own learning environment.**

- Model-Based learning has **the pre-cognition of the environment**, so we can find the optimal solution through **Dynamic Programming(DP)**. However, it's performance has a lot to do with the difference between real world and model itself.
- Model-Free has **no need for model learning** or in other words, we don't know the model. It lacks efficiency but can be realized and be set to a better status easier.

![主流的强化学习算法分类](https://easy-ai.oss-cn-shanghai.aliyuncs.com/2019-04-17-fenlei.png)

## Core Concept and Terminology

![../_images/rl_diagram_transparent_bg.png](https://spinningup.readthedocs.io/zh_CN/latest/_images/rl_diagram_transparent_bg.png)

- 状态和观察(states and observations)
- 动作空间(action spaces)
- 策略(polices)
- 行动轨迹(trajectories)
- 不同回报公式(formulations of return)
- 强化学习优化问题(the RL optimization problem)
- 值函数(value function)

### Polices [3]

#### Sketch 

**Actions:** The agents **policy π(s)** indicates the optimal action to take in a certain state to **reach the maximum of total rewards.**  

**States:** Each states is associated with  a **value function  V(s)** which predicts the expected amount rewards we are able to receive in this state by acting the corresponding policy. 

![Categorization of RL Algorithms](https://lilianweng.github.io/lil-log/assets/images/RL_algorithm_categorization.png)

The interaction between the entity and environment involves a sequence of actions and observed rewards in time, $t=1,2,…,T$. During the process, the entity accumulates the information of the environment to learn the policy and make decisions on which actions to take to efficiently learn the optimal policy. If we label the state, action and reward at time step t as $S_t, A_t, R_t$, respectively. 

Thus, the interaction sequence is fully described by one **episode(or trial, trajectory)**. The sequence ends at $S_t$:

$$S_1, A_1, R_2, S_2, A_2, R_3,..., S_T$$

#### Terminology 

- **Model-based**: Rely on the model of the environment; either the model is known or the algorithm learns it explicitly.
- **Model-free**: No dependency on the model during learning.
- **On-policy**: Use the deterministic outcomes or samples from the target policy to train the algorithm.
- **Off-policy**: Training on a distribution of transitions or episodes produced by a different behavior policy rather than that produced by the target policy.

##### Model: Transition and Reward

It has two major parts: transition probability function P and reward function R.

One transition step can be described ad a tuple{s, a, s', r}.

The transition function P records the probability of transitioning from state s to s' after taking action a while obtaining reward r. use $\mathbb{P}$ as a symbol of “probability”.

$$P\left(s^{\prime}, r \mid s, a\right)=\mathbb{P}\left[S_{t+1}=s^{\prime}, R_{t+1}=r \mid S_{t}=s, A_{t}=a\right]$$

Thus the state-transition function can be defined as a function of$P\left(s^{\prime}, r {\mid}s, a\right)$

$$P_{s s^{\prime}}^{a}=P\left(s^{\prime} \mid s, a\right)=\mathbb{P}\left[S_{t+1}=s^{\prime} \mid S_{t}=s, A_{t}=a\right]=\sum_{r \in \mathcal{R}} P\left(s^{\prime}, r \mid s, a\right)$$

Reward function predicts the next reward triggered by one action:

$$R(s, a)=\mathbb{E}\left[R_{t+1} \mid S_{t}=s, A_{t}=a\right]=\sum_{r \in \mathcal{R}} r \sum_{s^{\prime} \in \mathcal{S}} P\left(s^{\prime}, r \mid s, a\right)$$

##### Policy

As the behavior function $\pi$, it tells which action to take in state s. It is a mapping from state s to action a and can be either deterministic or stochastic:

- Deterministic: $π(s)=aπ(s)=a.$
- Stochastic: $π(a\mid s)=\mathbb{P_\pi}[A=a\mid S=s]$

##### Value function

Value function measures the goodness of a state or how rewarding a state or an action by the prediction of future reward. The future reward, also know as **return**, is a total sum of discounted rewards going forward. To compute the return $G_t$ starting from time t:

$$G_{t}=R_{t+1}+\gamma R_{t+2}+\cdots=\sum_{k=0}^{\infty} \gamma^{k} R_{t+k+1}$$

The discounting factor $\gamma \in[0,1]$ penalize the rewards in the future, due to:

- The future rewards may have higher uncertainty; i.e. stock market.
- The future rewards do not provide immediate benefits; i.e. As human beings, we might prefer to have fun today rather than 5 years later ;).
- *Discounting provides mathematical convenience; i.e., we don’t need to track future steps forever to compute return.
- *We don’t need to worry about the infinite loops in the state transition graph.

The **state-value** of a state is the expected return if we are in this state at time t, $S_t = s$.

$$V_{\pi}(s)=\mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=s\right]$$

Similarly, we define the **action-value** (“Q-value”; Q as “Quality”) of a state-action pair as:

$$Q_{\pi}(s, a)=\mathbb{E}_{\pi}\left[G_{t} \mid S_{t}=s, A_{t}=a\right]$$

Additionally, since we follow the target policy $\pi$, we can make use of the probability distribution over possible actions and the Q-values to recover the state-value:

$$V_{\pi}(s)=\sum_{a \in \mathcal{A}} Q_{\pi}(s, a) \pi(a \mid s)$$

The difference between action-value and state-value is the action **advantage** function (“A-value”):

$$A_{\pi}(s, a)=Q_{\pi}(s, a)-V_{\pi}(s)$$


##### Optimal Value and Policy

The optimal value function produces the maximum return:

$$V_{*}(s)=\max _{\pi} V_{\pi}(s), Q_{*}(s, a)=\max _{\pi} Q_{\pi}(s, a)$$


The optimal value function produces the maximum return:

$$\pi_{*}=\arg \max _{\pi} V_{\pi}(s), \pi_{*}=\arg \max _{\pi} Q_{\pi}(s, a)$$

And of course, we have:

$$V_{\pi_{*}}(s)=V_{*}(s) \text { and } Q_{\pi_{*}}(s, a)=Q_{*}(s, a)$$

### Markov Decision Processes




#### Classification

- Deterministic strategy

- Random strategy

  - Categorical Policies(绝对策略)

    More for discrete space

  - Diagonal Gaussian Policies(对角高斯策略)

    More for continuous space

### Bellman Equation





## Reference

[1] [强化学习-Reinforcement learning RL](https://easyai.tech/ai-definition/reinforcement-learning/)

[2] [三分钟看懂强化学习系列05贝尔曼方程](https://zhuanlan.zhihu.com/p/139559442)

[3] [A (Long) Peek into Reinforcement Learning](https://lilianweng.github.io/lil-log/2018/02/19/a-long-peek-into-reinforcement-learning.html)
