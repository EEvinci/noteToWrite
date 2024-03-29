# 学习进展回顾

在我们深入学习 Q-learning 算法前，先总结一下我们之前都学了什么。

我们学了两种基于价值的函数：

- 状态价值函数：如果**智能体在一个给定的状态开始行动，并在之后的行动中一直遵循该策略**，则输出该状态的期望回报。
- 动作价值函数：如果**智能体在给定的状态开始采取一个给定的行为，并在之后的行动中一直遵循该策略**，则输出该状态的期望回报。
- 在基于价值的方法中，相比于学习策略，我们**手动定义策略**并且学习一个价值函数。如果我们有一个最优价值函数，那就**有了一个最优的策略**。

还学了两种可以学习价值函数的策略的方法：

- 使用蒙特卡洛方法时，我们从完整的一轮游戏中更新值函数，并且**使用该轮游戏的真实、准确的折扣回报**。
- 使用TD学习方法时，我们从一个时间步中更新值函数，因此我们用一个**被称为TD目标的估计回报**替换我们没有的G_t。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/summary-learning-mtds.jpg" alt="Summary"/>