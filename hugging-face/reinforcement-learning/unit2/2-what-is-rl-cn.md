# 回顾: 什么是RL?

In RL, we build an agent that can **make smart decisions**. For instance, an agent that **learns to play a video game.** Or a trading agent that **learns to maximize its benefits** by deciding on **what stocks to buy and when to sell.**

在强化学习中，我们构建一个能做智能决策的智能体。例如，一个学习玩电子游戏的智能体，或一个能够通过决定商品的购入种类和售出时间从而最大化收益的贸易智能体。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/rl-process.jpg" alt="RL process"/>

But, to make intelligent decisions, our agent will learn from the environment by **interacting with it through trial and error** and receiving rewards (positive or negative) **as unique feedback.**

但是为了做出比较聪明的决策，我们的智能体需要**通过反复试验与环境交互**并接受奖励（正向或负向）**作为唯一反馈**，以此进行学习。

Its goal **is to maximize its expected cumulative reward** (because of the reward hypothesis).

智能体的目标是最大化累计期望奖励（基于奖励假设）

**The agent's decision-making process is called the policy π:** given a state, a policy will output an action or a probability distribution over actions. That is, given an observation of the environment, a policy will provide an action (or multiple probabilities for each action) that the agent should take.

智能体的决策过程称作策略Π：给定一个状态，一个策略将输出一个动作或一个动作的概率分布。也就是说，给定一个环境的观察，策略将会输出一个行动（或每一个动作的概率），智能体将会执行该动作。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/policy.jpg" alt="Policy"/>

**Our goal is to find an optimal policy π* **, aka., a policy that leads to the best expected cumulative reward.

我们的目标是找到一个最优的策略π*，也就是一个能够获得最好的累计期望奖励的策略。

And to find this optimal policy (hence solving the RL problem), there **are two main types of RL methods**:

并找到这个最优策略（为解决强化学习问题），有两种主要的强化学习方法：

- *Policy-based methods*: **Train the policy directly** to learn which action to take given a state.
- 基于策略的方法：根据给定的状态训练智能体直接学习要执行的动作。
- *Value-based methods*: **Train a value function** to learn **which state is more valuable** and use this value function **to take the action that leads to it.**
- 基于价值的方法：训练一个价值函数来学习更具有价值的状态，并用这个价值函数采取行动

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches.jpg" alt="Two RL approaches"/>

And in this unit, **we'll dive deeper into the value-based methods.**