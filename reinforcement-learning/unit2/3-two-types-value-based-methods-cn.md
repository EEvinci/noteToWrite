# 两种基于价值的方法

In value-based methods, **we learn a value function** that **maps a state to the expected value of being at that state.**

在基于价值方法中，**我们学习一个价值函数**，它能**将状态映射到当前状态的期望价值上**。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/vbm-1.jpg" alt="Value Based Methods"/>

**根据我们的策略**，如果智能体**从当前状态开始并行动**，那么该状态的价值就是智能体能得到的**期望折扣回报**。

<Tip>
但是什么是根据我们的策略行动呢？毕竟我们还没有一个基于价值方法的策略，因为我们训练的是一个价值函数而非是一个策略。
</Tip>

记住一个**强化学习智能体**的目标是拥有一个**最优的策略 π\***

为了找到这个最优的策略，我们要学习两种不同的方法：

- 基于策略的方法：**直接训练策略**来选择在给定状态下将要执行的动作（或一个基于当前状态的动作概率分布）。在这种情况下，我们**不需要价值函数**。

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches-2.jpg" alt="Two RL approaches"/>

该策略将一个状态作为输入并输出在该状态下将要执行的动作（确定性策略：一种可以在给定状态下输出一个动作的策略。与其相对应的是随机策略：输出一个动作的概率分布）

And consequently, **we don't define by hand the behavior of our policy; it's the training that will define it.**

并且在最后，**我们不需要手动定义策略的行为，策略会在训练中被定义**。

- 基于价值的方法：通过**间接训练一个价值函数**输出一个状态的价值或者一个状态-价值对。在给定价值函数的情况下，我们的策略**将会采取一个动作**。

因为策略是不能被训练或者是学习的，所以**我们需要指定它的行为**。举个例子，在给定价值函数的情况下，如果我们想要一个策略总是采取能够获得最大奖励的动作，**那我们就要定义一个贪心策略**。

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches-3.jpg" alt="Two RL approaches"/>
  <figcaption> 给定一个状态，我们的行为价值函数（已训练好的）会输出在该状态下的每一个动作的值。然后我们预先定义的贪心策略将在给定状态或状态价值对的情况下选择有最大值的动作。</figcaption>
</figure>
最终，无论你使用什么方法来解决问题，**你都将获得一个策略**。对于基于值的方法，你不需要训练策略：你的策略**只是一个简单的预先指定的函数**（例如：贪心策略），这个函数**使用价值函数给出的值**来选择动作。

所以两种方法之间的差别是：

- 在基于策略的方法中，**最优的策略（表示为π\*）是通过直接训练策略得到的。**
- 在基于价值的方法中，**找到一个最优的价值函数（表示为Q\* or V\*，我们会在之后的章节中学习它们之间的差别）从而获得一个最优的策略。**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="Link between value and policy"/>

实际上，在基于价值的方法中，我们通常会使用**一个 epsilon 贪心策略**来处理探索和利用之间的平衡问题；我们将在本单元的第二部分学习 Q-learning 算法的时候学习它。

我们有两种基于价值的函数：

## 状态价值函数

我们基于策略 π 对价值函数进行描述：

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/state-value-function-1.jpg" alt="State value function"/>

对于每一个状态，如果智能体**在该状态开始行动**，并且在之后一直遵从该策略（只要你想，可以是未来的所有步），状态价值函数会输出与之相对应的期望回报。

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/state-value-function-2.jpg" alt="State value function"/>
  <figcaption>如果我们在值为-7的状态开始：那么-7则是从该状态开始并且根据策略（贪心策略）采取行动的预期汇报，所以之后依据该策略，我们将采取的动作为：右，右，右，下，下，右，右。</figcaption>
</figure>



## 动作价值函数

在动作价值函数中，对于每一个状态和动作对，如果智能体在该状态开始采取行动并在之后一直遵循该策略，动作价值函数将会**输出相对应的预期汇报**。

基于策略 (π) 在状态 (s) 下采取动作 (a) 的价值是：



<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/action-state-value-function-1.jpg" alt="Action State value function"/>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/action-state-value-function-2.jpg" alt="Action State value function"/>

我们可以注意到两种函数之间的差别是：

- 在状态价值函数中，我们计算**状态 \(S_t）的值**。
- 在动作价值函数中，我们计算**状态价值对 (S_t, A_t)，因此计算在该状态下采取该动作的值。**

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-types.jpg" alt="Two types of value function"/>
  <figcaption>
注意: 我们在动作价值函数的例子中没有填满所有的状态价值对</figcaption>
</figure>


在任何情况下，无论我们选择什么价值函数（状态价值或者是动作价值函数），**回报函数都是预期回报。**

不过，问题是**为了计算一个状态或者状态价值对的每个值，我们都需要对一个智能体在该状态开始能获取奖励进行加和。**

### 这是一个比较昂贵的计算过程，但贝尔曼方程可以较为有效的应对这个问题。