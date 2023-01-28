# 强化学习框架

## 强化学习过程

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/RL_process.jpg" alt="The RL process" width="100%">
<figcaption>强化学习过程：一个包含状态、动作、奖励和下一个状态的循环</figcaption>
<figcaption>来源: <a href="http://incompleteideas.net/book/RLbook2020.pdf">Reinforcement Learning: An Introduction, Richard Sutton and Andrew G. Barto</a></figcaption>
</figure>

为了理解强化学习的过程，让我们想象一个智能体正在学习玩一个游戏：

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/RL_process_game.jpg" alt="The RL process" width="100%">

- 智能体从环境中接收到一个状态 **\\(S_0\\)** — 接收到了游戏（环境）的第一帧。
- 根据状态**\\(S_0\\)**，智能体采取了一个动作**\\(A_0\\)** — 智能体将向右移动。
- 环境进入下一个状态**\\(S_1\\)** — 新的一帧。
- 环境给智能体一些奖励 **\\(R_1\\)** — 智能体成功存活（正向奖励+1）

强化学习循环输出一个包含**状态、动作、奖励和下一个状态**的序列。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/sars.jpg" alt="State, Action, Reward, Next State" width="100%">

智能体的目标是最大化累计奖励**即预期收益**。

## 奖励假设：强化学习的核心思想

⇒ Why is the goal of the agent to maximize the expected return?

⇒ 为什么智能体的目标是最大化预期收益？

Because RL is based on the **reward hypothesis**, which is that all goals can be described as the **maximization of the expected return** (expected cumulative reward).

因为强化学习是基于**奖励假设**的，这也就是说所有的目标都可以被描述为预期收益（预期累计奖励）的最大化。

That’s why in Reinforcement Learning, **to have the best behavior,** we aim to learn to take actions that **maximize the expected cumulative reward.**

这也是为什么在强化学习中，我们致力于学习采取那些能够最大化预期累计奖励的动作，以此来获得最好的表现。


## Markov Property [[markov-property]]

In papers, you’ll see that the RL process is called the **Markov Decision Process** (MDP).

在论文中，你将了解到强化学习的过程被称之为马尔科夫决策过程（MDP）

We’ll talk again about the Markov Property in the following units. But if you need to remember something today about it, it's this: the Markov Property implies that our agent needs **only the current state to decide** what action to take and **not the history of all the states and actions** they took before.

我们在之后的章节中还会继续讨论马尔可夫性。但是在本单元中关于马尔可夫性你需要记住的是：

## Observations/States Space [[obs-space]]

Observations/States are the **information our agent gets from the environment.** In the case of a video game, it can be a frame (a screenshot). In the case of the trading agent, it can be the value of a certain stock, etc.

There is a differentiation to make between *observation* and *state*, however:

- *State s*: is **a complete description of the state of the world** (there is no hidden information). In a fully observed environment.


<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/chess.jpg" alt="Chess">
<figcaption>In chess game, we receive a state from the environment since we have access to the whole check board information.</figcaption>
</figure>

In a chess game, we have access to the whole board information, so we receive a state from the environment. In other words, the environment is fully observed.

- *Observation o*: is a **partial description of the state.** In a partially observed environment.

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/mario.jpg" alt="Mario">
<figcaption>In Super Mario Bros, we only see a part of the level close to the player, so we receive an observation.</figcaption>
</figure>

In Super Mario Bros, we only see a part of the level close to the player, so we receive an observation.

In Super Mario Bros, we are in a partially observed environment. We receive an observation **since we only see a part of the level.**

<Tip>
In this course, we use the term "state" to denote both state and observation, but we will make the distinction in implementations.
</Tip>

To recap:
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/obs_space_recap.jpg" alt="Obs space recap" width="100%">


## Action Space [[action-space]]

The Action space is the set of **all possible actions in an environment.**

The actions can come from a *discrete* or *continuous space*:

- *Discrete space*: the number of possible actions is **finite**.

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/mario.jpg" alt="Mario">
<figcaption>Again, in Super Mario Bros, we have only 5 possible actions: 4 directions and jumping</figcaption>

</figure>

In Super Mario Bros, we have a finite set of actions since we have only 4 directions and jump.

- *Continuous space*: the number of possible actions is **infinite**.

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/self_driving_car.jpg" alt="Self Driving Car">
<figcaption>A Self Driving Car agent has an infinite number of possible actions since it can turn left 20°, 21,1°, 21,2°, honk, turn right 20°…
</figcaption>
</figure>

To recap:
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/action_space.jpg" alt="Action space recap" width="100%">

Taking this information into consideration is crucial because it will **have importance when choosing the RL algorithm in the future.**

## Rewards and the discounting [[rewards]]

The reward is fundamental in RL because it’s **the only feedback** for the agent. Thanks to it, our agent knows **if the action taken was good or not.**

The cumulative reward at each time step **t** can be written as:

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/rewards_1.jpg" alt="Rewards">
<figcaption>The cumulative reward equals to the sum of all rewards of the sequence.
</figcaption>
</figure>

Which is equivalent to:

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/rewards_2.jpg" alt="Rewards">
<figcaption>The cumulative reward = rt+1 (rt+k+1 = rt+0+1 = rt+1)+ rt+2 (rt+k+1 = rt+1+1 = rt+2) + ...
</figcaption>
</figure>

However, in reality, **we can’t just add them like that.** The rewards that come sooner (at the beginning of the game) **are more likely to happen** since they are more predictable than the long-term future reward.

Let’s say your agent is this tiny mouse that can move one tile each time step, and your opponent is the cat (that can move too). The mouse's goal is **to eat the maximum amount of cheese before being eaten by the cat.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/rewards_3.jpg" alt="Rewards" width="100%">

As we can see in the diagram, **it’s more probable to eat the cheese near us than the cheese close to the cat** (the closer we are to the cat, the more dangerous it is).

Consequently, **the reward near the cat, even if it is bigger (more cheese), will be more discounted** since we’re not really sure we’ll be able to eat it.

To discount the rewards, we proceed like this:

1. We define a discount rate called gamma. **It must be between 0 and 1.** Most of the time between **0.99 and 0.95**.
- The larger the gamma, the smaller the discount. This means our agent **cares more about the long-term reward.**
- On the other hand, the smaller the gamma, the bigger the discount. This means our **agent cares more about the short term reward (the nearest cheese).**

2. Then, each reward will be discounted by gamma to the exponent of the time step. As the time step increases, the cat gets closer to us, **so the future reward is less and less likely to happen.**

Our discounted expected cumulative reward is:
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/rewards_4.jpg" alt="Rewards" width="100%">