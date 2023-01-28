# 什么是强化学习？

让我们从一张图片开始理解强化学习。

## 示例图

强化学习背后的思想就是智能体在环境中，通过与环境进行交互（反复试验），并且接受奖励（正向的或负向的）作为执行动作的回馈，以此进行学习。

从与环境的互动中进行学习这种学习方法来源于我们的自然经验。

例如，想象一下你给你弟弟一个游戏手柄，让他自己玩一个他之前从来没有玩过的游戏。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/Illustration_1.jpg" alt="Illustration_1" width="100%">

你弟弟按了右键（动作）与环境（游戏）进行交互，他得到了一个金币，这意味着一个+1的正向奖励，他就明白了他要在这个游戏中获得更多的金币。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/Illustration_2.jpg" alt="Illustration_2" width="100%">

之后，他继续按右键，但碰到了一个敌人，游戏输了，这意味着一个-1的负向奖励。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit1/Illustration_3.jpg" alt="Illustration_3" width="100%">

通过与环境进行反复的交互，你弟弟明白了他需要在避免碰到敌人的同时获取更多的金币。

在没有任何监督的情况下，你弟弟会玩的越来越好。

以上简要说明了人和动物通过交互来进行学习的方式，而强化学习就是一种从行动中进行学习的算法。


### 常规定义

我们采取一种常规的定义：

<Tip>
强化学习是一种解决控制任务（也称为决策问题）的框架。通过构建在环境中学习的智能体，该智能体与环境进行反复交互并接受奖励（正向或负向）作为唯一的反馈。

</Tip>

但强化学习是如何运作的呢？