# unit 2

[TOC]



# Q-Learning简介

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/thumbnail.jpg" alt="Unit 2 thumbnail" width="100%">


In the first unit of this class, we learned about Reinforcement Learning (RL), the RL process, and the different methods to solve an RL problem. We also **trained our first agents and uploaded them to the Hugging Face Hub.**

In this unit, we're going to **dive deeper into one of the Reinforcement Learning methods: value-based methods** and study our first RL algorithm: **Q-Learning.**

We'll also **implement our first RL agent from scratch**, a Q-Learning agent, and will train it in two environments:

1. Frozen-Lake-v1 (non-slippery version): where our agent will need to **go from the starting state (S) to the goal state (G)** by walking only on frozen tiles (F) and avoiding holes (H).
2. An autonomous taxi: where our agent will need **to learn to navigate** a city to **transport its passengers from point A to point B.**

在本课程的第一单元中，我们了解了强化学习 (RL)、RL 过程以及解决 RL 问题的不同方法。 我们还**训练了我们的第一批代理并将它们上传到 Hugging Face Hub。**

在本单元中，我们将**深入研究其中一种强化学习方法：基于价值的方法**并研究我们的第一个 RL 算法：**Q-Learning。**

我们还将**从头开始实现我们的第一个 RL 智能体**，一个 Q-Learning 智能体，并将在两个环境中训练它：

1. Frozen-Lake-v1（防滑版本）：我们的智能体需要**从起始状态（S）到目标状态（G）**，只在冻结的瓷砖（F）上行走并避免孔 (H)。
2. 自动驾驶出租车：我们的智能体需要**学习导航**城市以**将乘客从 A 点运送到 B 点。**


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/envs.gif" alt="Environments"/>

Concretely, we will:

- Learn about **value-based methods**.
- Learn about the **differences between Monte Carlo and Temporal Difference Learning**.
- Study and implement **our first RL algorithm**: Q-Learning.

This unit is **fundamental if you want to be able to work on Deep Q-Learning**: the first Deep RL algorithm that played Atari games and beat the human level on some of them (breakout, space invaders, etc).

So let's get started! 🚀

具体来说，我们将：

- 了解**基于价值的方法**。
- 了解**蒙特卡洛和时间差异学习之间的差异**。
- 研究并实施**我们的第一个 RL 算法**：Q-Learning。

如果你想从事深度 Q 学习，这个单元是**基础**：第一个玩 Atari 游戏并在其中一些游戏（突破、太空入侵者等）上击败人类水平的深度 RL 算法。

让我们开始吧！ 🚀

# 【简短回顾】什么是强化学习？ 

In RL, we build an agent that can ***\*****make smart decisions*****\***. For instance, an agent that ***\*****learns to play a video game.*****\*** Or a trading agent that ***\*****learns to maximize its benefits*****\*** by deciding on ***\*****what stocks to buy and when to sell.*****\***

在 RL 中，我们构建了一个可以 ***\***** 做出明智决策*****\*** 的代理。 例如，***\***** 学习玩视频游戏的代理人。*****\*** 或 ***\***** 学习最大化其利益的交易代理人 *****\*** 通过决定 ***\***** 买入什么股票以及何时卖出。*****\***

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/rl-process.jpg" alt="RL process"/>





But, to make intelligent decisions, our agent will learn from the environment by ***\*****interacting with it through trial and error*****\*** and receiving rewards (positive or negative) ***\*****as unique feedback.*****\***



Its goal ***\*****is to maximize its expected cumulative reward*****\*** (because of the reward hypothesis).



***\*****The agent's decision-making process is called the policy π:*****\*** given a state, a policy will output an action or a probability distribution over actions. That is, given an observation of the environment, a policy will provide an action (or multiple probabilities for each action) that the agent should take.



<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/policy.jpg" alt="Policy"/>



*****Our goal is to find an optimal policy π**** **, aka., a policy that leads to the best expected cumulative reward.



And to find this optimal policy (hence solving the RL problem), there ***\*****are two main types of RL methods*****\***:



\- ****Policy-based methods****: ***\*****Train the policy directly*****\*** to learn which action to take given a state.

\- ****Value-based methods****: ***\*****Train a value function*****\*** to learn ***\*****which state is more valuable*****\*** and use this value function ***\*****to take the action that leads to it.*****\***



<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches.jpg" alt="Two RL approaches"/>

And in this unit, ***\*****we'll dive deeper into the value-based methods.*****\***

# Two types of value-based methods [[two-types-value-based-methods]]

In value-based methods, **we learn a value function** that **maps a state to the expected value of being at that state.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/vbm-1.jpg" alt="Value Based Methods"/>

The value of a state is the **expected discounted return** the agent can get if it **starts at that state and then acts according to our policy.**

<Tip>
But what does it mean to act according to our policy? After all, we don't have a policy in value-based methods since we train a value function and not a policy.
</Tip>

Remember that the goal of an **RL agent is to have an optimal policy π\*.**

To find the optimal policy, we learned about two different methods:

- *Policy-based methods:* **Directly train the policy** to select what action to take given a state (or a probability distribution over actions at that state). In this case, we **don't have a value function.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches-2.jpg" alt="Two RL approaches"/>

The policy takes a state as input and outputs what action to take at that state (deterministic policy: a policy that output one action given a state, contrary to stochastic policy that output a probability distribution over actions).

And consequently, **we don't define by hand the behavior of our policy; it's the training that will define it.**

- *Value-based methods:* **Indirectly, by training a value function** that outputs the value of a state or a state-action pair. Given this value function, our policy **will take an action.**

Since the policy is not trained/learned, **we need to specify its behavior.** For instance, if we want a policy that, given the value function, will take actions that always lead to the biggest reward, **we'll create a Greedy Policy.**

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-approaches-3.jpg" alt="Two RL approaches"/>
  <figcaption>Given a state, our action-value function (that we train) outputs the value of each action at that state. Then, our pre-defined Greedy Policy selects the action that will yield the highest value given a state or a state action pair.</figcaption>
</figure>

Consequently, whatever method you use to solve your problem, **you will have a policy**. In the case of value-based methods, you don't train the policy: your policy **is just a simple pre-specified function** (for instance, Greedy Policy) that uses the values given by the value-function to select its actions.

So the difference is:

- In policy-based, **the optimal policy (denoted π\*) is found by training the policy directly.**
- In value-based, **finding an optimal value function (denoted Q\* or V\*, we'll study the difference after) leads to having an optimal policy.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="Link between value and policy"/>

In fact, most of the time, in value-based methods, you'll use **an Epsilon-Greedy Policy** that handles the exploration/exploitation trade-off; we'll talk about it when we talk about Q-Learning in the second part of this unit.


So, we have two types of value-based functions:

## The state-value function [[state-value-function]]

We write the state value function under a policy π like this:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/state-value-function-1.jpg" alt="State value function"/>

For each state, the state-value function outputs the expected return if the agent **starts at that state** and then follows the policy forever afterward (for all future timesteps, if you prefer).

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/state-value-function-2.jpg" alt="State value function"/>
  <figcaption>If we take the state with value -7: it's the expected return starting at that state and taking actions according to our policy (greedy policy), so right, right, right, down, down, right, right.</figcaption>
</figure>

# The action-value function [[action-value-function]]

In the action-value function, for each state and action pair, the action-value function **outputs the expected return** if the agent starts in that state and takes action, and then follows the policy forever after.

The value of taking action \\(a\\) in state \\(s\\) under a policy \\(π\\) is:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/action-state-value-function-1.jpg" alt="Action State value function"/>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/action-state-value-function-2.jpg" alt="Action State value function"/>


We see that the difference is:

- In state-value function, we calculate **the value of a state \\(S_t\\)**
- In action-value function, we calculate **the value of the state-action pair ( \\(S_t, A_t\\) ) hence the value of taking that action at that state.**

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/two-types.jpg" alt="Two types of value function"/>
  <figcaption>
Note: We didn't fill all the state-action pairs for the example of Action-value function</figcaption>
</figure>

In either case, whatever value function we choose (state-value or action-value function), **the returned value is the expected return.**

However, the problem is that it implies that **to calculate EACH value of a state or a state-action pair, we need to sum all the rewards an agent can get if it starts at that state.**

This can be a computationally expensive process, and that's **where the Bellman equation comes to help us.**

# The Bellman Equation: simplify our value estimation [[bellman-equation]]

The Bellman equation **simplifies our state value or state-action value calculation.**


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman.jpg" alt="Bellman equation"/>

With what we have learned so far, we know that if we calculate the \\(V(S_t)\\) (value of a state), we need to calculate the return starting at that state and then follow the policy forever after. **(The policy we defined in the following example is a Greedy Policy; for simplification, we don't discount the reward).**

So to calculate \\(V(S_t)\\), we need to calculate the sum of the expected rewards. Hence:

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman2.jpg" alt="Bellman equation"/>
  <figcaption>To calculate the value of State 1: the sum of rewards if the agent started in that state and then followed the greedy policy (taking actions that leads to the best states values) for all the time steps.</figcaption>
</figure>

Then, to calculate the \\(V(S_{t+1})\\), we need to calculate the return starting at that state \\(S_{t+1}\\).

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman3.jpg" alt="Bellman equation"/>
  <figcaption>To calculate the value of State 2: the sum of rewards <b>if the agent started in that state</b>, and then followed the <b>policy for all the time steps.</b></figcaption>
</figure>

So you may have noticed, we're repeating the computation of the value of different states, which can be tedious if you need to do it for each state value or state-action value.

Instead of calculating the expected return for each state or each state-action pair, **we can use the Bellman equation.** (hint: if you know what Dynamic Programming is, this is very similar! if you don't know what it is, no worries!)

The Bellman equation is a recursive equation that works like this: instead of starting for each state from the beginning and calculating the return, we can consider the value of any state as:

**The immediate reward  \\(R_{t+1}\\)  + the discounted value of the state that follows ( \\(gamma * V(S_{t+1}) \\) ) .**

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman4.jpg" alt="Bellman equation"/>
</figure>


If we go back to our example, we can say that the value of State 1 is equal to the expected cumulative return if we start at that state.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman2.jpg" alt="Bellman equation"/>


To calculate the value of State 1: the sum of rewards **if the agent started in that state 1** and then followed the **policy for all the time steps.**

This is equivalent to  \\(V(S_{t})\\)  = Immediate reward  \\(R_{t+1}\\)  + Discounted value of the next state  \\(\gamma * V(S_{t+1})\\)

<figure>
  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman6.jpg" alt="Bellman equation"/>
  <figcaption>For simplification, here we don’t discount so gamma = 1.</figcaption>
</figure>

In the interest of simplicity, here we don't discount, so gamma = 1.
But you'll study an example with gamma = 0.99 in the Q-Learning section of this unit.

- The value of  \\(V(S_{t+1}) \\)  = Immediate reward  \\(R_{t+2}\\)  + Discounted value of the next state ( \\(gamma * V(S_{t+2})\\) ).
- And so on.





To recap, the idea of the Bellman equation is that instead of calculating each value as the sum of the expected return, **which is a long process**, we calculate the value as **the sum of immediate reward + the discounted value of the state that follows.**

Before going to the next section, think about the role of gamma in the Bellman equation. What happens if the value of gamma is very low (e.g. 0.1 or even 0)? What happens if the value is 1? What happens if the value is very high, such as a million?

# Monte Carlo vs Temporal Difference Learning [[mc-vs-td]]

The last thing we need to discuss before diving into Q-Learning is the two learning strategies.

Remember that an RL agent **learns by interacting with its environment.** The idea is that **given the experience and the received reward, the agent will update its value function or policy.**

Monte Carlo and Temporal Difference Learning are two different **strategies on how to train our value function or our policy function.** Both of them **use experience to solve the RL problem.**

On one hand, Monte Carlo uses **an entire episode of experience before learning.** On the other hand, Temporal Difference uses **only a step ( \\(S_t, A_t, R_{t+1}, S_{t+1}\\) ) to learn.**

We'll explain both of them **using a value-based method example.**

## Monte Carlo: learning at the end of the episode [[monte-carlo]]

Monte Carlo waits until the end of the episode, calculates  \\(G_t\\) (return) and uses it as **a target for updating  \\(V(S_t)\\).**

So it requires a **complete episode of interaction before updating our value function.**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/monte-carlo-approach.jpg" alt="Monte Carlo"/>


If we take an example:

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-2.jpg" alt="Monte Carlo"/>


- We always start the episode **at the same starting point.**
- **The agent takes actions using the policy**. For instance, using an Epsilon Greedy Strategy, a policy that alternates between exploration (random actions) and exploitation.
- We get **the reward and the next state.**
- We terminate the episode if the cat eats the mouse or if the mouse moves > 10 steps.

- At the end of the episode, **we have a list of State, Actions, Rewards, and Next States tuples**
For instance [[State tile 3 bottom, Go Left, +1, State tile 2 bottom], [State tile 2 bottom, Go Left, +0, State tile 1 bottom]...]

- **The agent will sum the total rewards \\(G_t\\)** (to see how well it did).
- It will then **update \\(V(s_t)\\) based on the formula**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-3.jpg" alt="Monte Carlo"/>

- Then **start a new game with this new knowledge**

By running more and more episodes, **the agent will learn to play better and better.**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-3p.jpg" alt="Monte Carlo"/>

For instance, if we train a state-value function using Monte Carlo:

- We just started to train our value function, **so it returns 0 value for each state**
- Our learning rate (lr) is 0.1 and our discount rate is 1 (= no discount)
- Our mouse **explores the environment and takes random actions**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-4.jpg" alt="Monte Carlo"/>


- The mouse made more than 10 steps, so the episode ends .

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-4p.jpg" alt="Monte Carlo"/>


- We have a list of state, action, rewards, next_state, **we need to calculate the return \\(G{t}\\)**
- \\(G_t = R_{t+1} + R_{t+2} + R_{t+3} ...\\)
- \\(G_t = R_{t+1} + R_{t+2} + R_{t+3}…\\) (for simplicity we don’t discount the rewards).
- \\(G_t = 1 + 0 + 0 + 0+ 0 + 0 + 1 + 1 + 0 + 0\\)
- \\(G_t= 3\\)
- We can now update \\(V(S_0)\\):

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-5.jpg" alt="Monte Carlo"/>

- New \\(V(S_0) = V(S_0) + lr * [G_t — V(S_0)]\\)
- New \\(V(S_0) = 0 + 0.1 * [3 – 0]\\)
- New \\(V(S_0) = 0.3\\)


  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/MC-5p.jpg" alt="Monte Carlo"/>


## Temporal Difference Learning: learning at each step [[td-learning]]

**Temporal Difference, on the other hand, waits for only one interaction (one step) \\(S_{t+1}\\)** to form a TD target and update \\(V(S_t)\\) using \\(R_{t+1}\\) and \\( \gamma * V(S_{t+1})\\).

The idea with **TD is to update the \\(V(S_t)\\) at each step.**

But because we didn't experience an entire episode, we don't have \\(G_t\\) (expected return). Instead, **we estimate \\(G_t\\) by adding \\(R_{t+1}\\) and the discounted value of the next state.**

This is called bootstrapping. It's called this **because TD bases its update part on an existing estimate \\(V(S_{t+1})\\) and not a complete sample \\(G_t\\).**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-1.jpg" alt="Temporal Difference"/>


This method is called TD(0) or **one-step TD (update the value function after any individual step).**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-1p.jpg" alt="Temporal Difference"/>

If we take the same example,

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-2.jpg" alt="Temporal Difference"/>

- We just started to train our value function, so it returns 0 value for each state.
- Our learning rate (lr) is 0.1, and our discount rate is 1 (no discount).
- Our mouse explore the environment and take a random action: **going to the left**
- It gets a reward  \\(R_{t+1} = 1\\) since **it eats a piece of cheese**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-2p.jpg" alt="Temporal Difference"/>


  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-3.jpg" alt="Temporal Difference"/>

We can now update  \\(V(S_0)\\):

New  \\(V(S_0) = V(S_0) + lr * [R_1 + \gamma * V(S_1) - V(S_0)]\\)

New \\(V(S_0) = 0 + 0.1 * [1 + 1 * 0–0]\\)

New \\(V(S_0) = 0.1\\)

So we just updated our value function for State 0.

Now we **continue to interact with this environment with our updated value function.**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-3p.jpg" alt="Temporal Difference"/>

  If we summarize:

  - With *Monte Carlo*, we update the value function from a complete episode, and so we **use the actual accurate discounted return of this episode.**
  - With *TD Learning*, we update the value function from a step, so we replace \\(G_t\\) that we don't have with **an estimated return called TD target.**

  <img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Summary.jpg" alt="Summary"/>

# Mid-way Recap [[mid-way-recap]]

Before diving into Q-Learning, let's summarize what we just learned.

We have two types of value-based functions:

- State-value function: outputs the expected return if **the agent starts at a given state and acts accordingly to the policy forever after.**
- Action-value function: outputs the expected return if **the agent starts in a given state, takes a given action at that state** and then acts accordingly to the policy forever after.
- In value-based methods, rather than learning the policy, **we define the policy by hand** and we learn a value function. If we have an optimal value function, we **will have an optimal policy.**

There are two types of methods to learn a policy for a value function:

- With *the Monte Carlo method*, we update the value function from a complete episode, and so we **use the actual accurate discounted return of this episode.**
- With *the TD Learning method,* we update the value function from a step, so we replace \\(G_t\\) that we don't have with **an estimated return called TD target.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/summary-learning-mtds.jpg" alt="Summary"/>

# Mid-way Quiz [[mid-way-quiz]]

The best way to learn and [to avoid the illusion of competence](https://www.coursera.org/lecture/learning-how-to-learn/illusions-of-competence-BuFzf) **is to test yourself.** This will help you to find **where you need to reinforce your knowledge**.


## Q1: What are the two main approaches to find optimal policy?

<Question
    choices={[
        {
            text: "Policy-based methods",
            explain: "With Policy-Based methods, we train the policy directly to learn which action to take given a state.",
      correct: true
        },
        {
            text: "Random-based methods",
            explain: ""
        },
    {
            text: "Value-based methods",
            explain: "With value-based methods, we train a value function to learn which state is more valuable and use this value function to take the action that leads to it.",
      correct: true
        },
        {
            text: "Evolution-strategies methods",
      explain: ""
        }
    ]}
/>


## Q2: What is the Bellman Equation?

<details>
<summary>Solution</summary>

**The Bellman equation is a recursive equation** that works like this: instead of starting for each state from the beginning and calculating the return, we can consider the value of any state as:

Rt+1 + gamma * V(St+1)

The immediate reward + the discounted value of the state that follows

</details>

## Q3: Define each part of the Bellman Equation

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman4-quiz.jpg" alt="Bellman equation quiz"/>


<details>
<summary>Solution</summary>

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/bellman4.jpg" alt="Bellman equation solution"/>

</details>

## Q4: What is the difference between Monte Carlo and Temporal Difference learning methods?

<Question
    choices={[
        {
            text: "With Monte Carlo methods, we update the value function from a complete episode",
            explain: "",
      correct: true
        },
    {
            text: "With Monte Carlo methods, we update the value function from a step",
            explain: ""
        },
    {
            text: "With TD learning methods, we update the value function from a complete episode",
            explain: ""
        },
    {
            text: "With TD learning methods, we update the value function from a step",
            explain: "",
      correct: true
        },
    ]}
/>

## Q5: Define each part of Temporal Difference learning formula

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/td-ex.jpg" alt="TD Learning exercise"/>

<details>
<summary>Solution</summary>

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/TD-1.jpg" alt="TD Exercise"/>

</details>


## Q6: Define each part of Monte Carlo learning formula

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/mc-ex.jpg" alt="MC Learning exercise"/>

<details>
<summary>Solution</summary>

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/monte-carlo-approach.jpg" alt="MC Exercise"/>

</details>

Congrats on finishing this Quiz 🥳, if you missed some elements, take time to read again the previous sections to reinforce (😏) your knowledge.

# Introducing Q-Learning [[q-learning]]
## What is Q-Learning? [[what-is-q-learning]]

Q-Learning is an **off-policy value-based method that uses a TD approach to train its action-value function:**

- *Off-policy*: we'll talk about that at the end of this unit.
- *Value-based method*: finds the optimal policy indirectly by training a value or action-value function that will tell us **the value of each state or each state-action pair.**
- *Uses a TD approach:* **updates its action-value function at each step instead of at the end of the episode.**

**Q-Learning is the algorithm we use to train our Q-function**, an **action-value function** that determines the value of being at a particular state and taking a specific action at that state.

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-function.jpg" alt="Q-function"/>
  <figcaption>Given a state and action, our Q Function outputs a state-action value (also called Q-value)</figcaption>
</figure>

The **Q comes from "the Quality" (the value) of that action at that state.**

Let's recap the difference between value and reward:

- The *value of a state*, or a *state-action pair* is the expected cumulative reward our agent gets if it starts at this state (or state-action pair) and then acts accordingly to its policy.
- The *reward* is the **feedback I get from the environment** after performing an action at a state.

Internally, our Q-function has **a Q-table, a table where each cell corresponds to a state-action pair value.** Think of this Q-table as **the memory or cheat sheet of our Q-function.**

Let's go through an example of a maze.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Maze-1.jpg" alt="Maze example"/>

The Q-table is initialized. That's why all values are = 0. This table **contains, for each state, the four state-action values.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Maze-2.jpg" alt="Maze example"/>

Here we see that the **state-action value of the initial state and going up is 0:**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Maze-3.jpg" alt="Maze example"/>

Therefore, Q-function contains a Q-table **that has the value of each-state action pair.** And given a state and action, **our Q-function will search inside its Q-table to output the value.**

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-function-2.jpg" alt="Q-function"/>
</figure>

If we recap, *Q-Learning* **is the RL algorithm that:**

- Trains a *Q-function* (an **action-value function**), which internally is a **Q-table that contains all the state-action pair values.**
- Given a state and action, our Q-function **will search into its Q-table the corresponding value.**
- When the training is done, **we have an optimal Q-function, which means we have optimal Q-table.**
- And if we **have an optimal Q-function**, we **have an optimal policy** since we **know for each state what is the best action to take.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="Link value policy"/>


But, in the beginning, **our Q-table is useless since it gives arbitrary values for each state-action pair** (most of the time, we initialize the Q-table to 0). As the agent **explores the environment and we update the Q-table, it will give us better and better approximations** to the optimal policy.

<figure class="image table text-center m-0 w-full">
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-1.jpg" alt="Q-learning"/>
  <figcaption>We see here that with the training, our Q-table is better since, thanks to it, we can know the value of each state-action pair.</figcaption>
</figure>

Now that we understand what Q-Learning, Q-function, and Q-table are, **let's dive deeper into the Q-Learning algorithm**.

## The Q-Learning algorithm [[q-learning-algo]]

This is the Q-Learning pseudocode; let's study each part and **see how it works with a simple example before implementing it.** Don't be intimidated by it, it's simpler than it looks! We'll go over each step.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-2.jpg" alt="Q-learning"/>

### Step 1: We initialize the Q-table [[step1]]

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-3.jpg" alt="Q-learning"/>


We need to initialize the Q-table for each state-action pair. **Most of the time, we initialize with values of 0.**

### Step 2: Choose action using epsilon-greedy strategy [[step2]]

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-4.jpg" alt="Q-learning"/>


Epsilon greedy strategy is a policy that handles the exploration/exploitation trade-off.

The idea is that we define the initial epsilon ɛ = 1.0:

- *With probability 1 — ɛ* : we do **exploitation** (aka our agent selects the action with the highest state-action pair value).
- With probability ɛ: **we do exploration** (trying random action).

At the beginning of the training, **the probability of doing exploration will be huge since ɛ is very high, so most of the time, we'll explore.** But as the training goes on, and consequently our **Q-table gets better and better in its estimations, we progressively reduce the epsilon value** since we will need less and less exploration and more exploitation.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-5.jpg" alt="Q-learning"/>


### Step 3: Perform action At, gets reward Rt+1 and next state St+1 [[step3]]

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-6.jpg" alt="Q-learning"/>

## Step 4: Update Q(St, At) [[step4]]

Remember that in TD Learning, we update our policy or value function (depending on the RL method we choose) **after one step of the interaction.**

To produce our TD target, **we used the immediate reward \\(R_{t+1}\\) plus the discounted value of the next state best state-action pair** (we call that bootstrap).

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-7.jpg" alt="Q-learning"/>

Therefore, our \\(Q(S_t, A_t)\\) **update formula goes like this:**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-8.jpg" alt="Q-learning"/>


It means that to update our \\(Q(S_t, A_t)\\):

- We need \\(S_t, A_t, R_{t+1}, S_{t+1}\\).
- To update our Q-value at a given state-action pair, we use the TD target.

How do we form the TD target?
1. We obtain the reward after taking the action \\(R_{t+1}\\).
2. To get the **best next-state-action pair value**, we use a greedy policy to select the next best action. Note that this is not an epsilon-greedy policy, this will always take the action with the highest state-action value.

Then when the update of this Q-value is done, we start in a new state and select our action **using a epsilon-greedy policy again.**

**This is why we say that Q Learning is an off-policy algorithm.**

## Off-policy vs On-policy [[off-vs-on]]

The difference is subtle:

- *Off-policy*: using **a different policy for acting (inference) and updating (training).**

For instance, with Q-Learning, the epsilon-greedy policy (acting policy), is different from the greedy policy that is **used to select the best next-state action value to update our Q-value (updating policy).**


<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-1.jpg" alt="Off-on policy"/>
  <figcaption>Acting Policy</figcaption>
</figure>

Is different from the policy we use during the training part:


<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-2.jpg" alt="Off-on policy"/>
  <figcaption>Updating policy</figcaption>
</figure>

- *On-policy:* using the **same policy for acting and updating.**

For instance, with Sarsa, another value-based algorithm, **the epsilon-greedy policy selects the next state-action pair, not a greedy policy.**


<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-3.jpg" alt="Off-on policy"/>
    <figcaption>Sarsa</figcaption>
</figure>

<figure>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-4.jpg" alt="Off-on policy"/>
</figure>
# A Q-Learning example [[q-learning-example]]

To better understand Q-Learning, let's take a simple example:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Maze-Example-2.jpg" alt="Maze-Example"/>

- You're a mouse in this tiny maze. You always **start at the same starting point.**
- The goal is **to eat the big pile of cheese at the bottom right-hand corner** and avoid the poison. After all, who doesn't like cheese?
- The episode ends if we eat the poison, **eat the big pile of cheese or if we spent more than five steps.**
- The learning rate is 0.1
- The gamma (discount rate) is 0.99

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-1.jpg" alt="Maze-Example"/>


The reward function goes like this:

- **+0:** Going to a state with no cheese in it.
- **+1:** Going to a state with a small cheese in it.
- **+10:** Going to the state with the big pile of cheese.
- **-10:** Going to the state with the poison and thus die.
- **+0** If we spend more than five steps.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-2.jpg" alt="Maze-Example"/>

To train our agent to have an optimal policy (so a policy that goes right, right, down), **we will use the Q-Learning algorithm**.

## Step 1: We initialize the Q-table [[step1]]

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Example-1.jpg" alt="Maze-Example"/>

So, for now, **our Q-table is useless**; we need **to train our Q-function using the Q-Learning algorithm.**

Let's do it for 2 training timesteps:

Training timestep 1:

## Step 2: Choose action using Epsilon Greedy Strategy [[step2]]

Because epsilon is big = 1.0, I take a random action, in this case, I go right.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-3.jpg" alt="Maze-Example"/>


## Step 3: Perform action At, gets Rt+1 and St+1 [[step3]]

By going right, I've got a small cheese, so \\(R_{t+1} = 1\\), and I'm in a new state.


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-4.jpg" alt="Maze-Example"/>


## Step 4: Update Q(St, At) [[step4]]

We can now update \\(Q(S_t, A_t)\\) using our formula.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-5.jpg" alt="Maze-Example"/>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Example-4.jpg" alt="Maze-Example"/>

Training timestep 2:

## Step 2: Choose action using Epsilon Greedy Strategy [[step2-2]]

**I take a random action again, since epsilon is big 0.99** (since we decay it a little bit because as the training progress, we want less and less exploration).

I took action down. **Not a good action since it leads me to the poison.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-6.jpg" alt="Maze-Example"/>


## Step 3: Perform action At, gets Rt+1 and St+1 [[step3-3]]

Because I go to the poison state, **I get \\(R_{t+1} = -10\\), and I die.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-7.jpg" alt="Maze-Example"/>

## Step 4: Update Q(St, At) [[step4-4]]

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-ex-8.jpg" alt="Maze-Example"/>

Because we're dead, we start a new episode. But what we see here is that **with two explorations steps, my agent became smarter.**

As we continue exploring and exploiting the environment and updating Q-values using TD target, **Q-table will give us better and better approximations. And thus, at the end of the training, we'll get an estimate of the optimal Q-function.**

# Q-Learning Recap [[q-learning-recap]]


The *Q-Learning* **is the RL algorithm that** :

- Trains *Q-function*, an **action-value function** that contains, as internal memory, a *Q-table* **that contains all the state-action pair values.**

- Given a state and action, our Q-function **will search into its Q-table the corresponding value.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-function-2.jpg" alt="Q function"  width="100%"/>

- When the training is done,**we have an optimal Q-function, so an optimal Q-table.**

- And if we **have an optimal Q-function**, we
have an optimal policy,since we **know for each state, what is the best action to take.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="Link value policy"  width="100%"/>

But, in the beginning, our **Q-table is useless since it gives arbitrary value for each state-action pair (most of the time we initialize the Q-table to 0 values)**. But, as we’ll explore the environment and update our Q-table it will give us better and better approximations

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/q-learning.jpeg" alt="q-learning.jpeg" width="100%"/>

This is the Q-Learning pseudocode:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-2.jpg" alt="Q-Learning" width="100%"/>

# Glossary [[glossary]]

This is a community-created glossary. Contributions are welcomed!

### Strategies to find the optimal policy

- **Policy-based methods.** The policy is usually trained with a neural network to select what action to take given a state. In this case is the neural network which outputs the action that the agent should take instead of using a value function. Depending on the experience received by the environment, the neural network will be re-adjusted and will provide better actions.
- **Value-based methods.** In this case, a value function is trained to output the value of a state or a state-action pair that will represent our policy. However, this value doesn't define what action the agent should take. In contrast, we need to specify the behavior of the agent given the output of the value function. For example, we could decide to adopt a policy to take the action that always leads to the biggest reward (Greedy Policy). In summary, the policy is a Greedy Policy (or whatever decision the user takes) that uses the values of the value-function to decide the actions to take.

### Among the value-based methods, we can find two main strategies

- **The state-value function.** For each state, the state-value function is the expected return if the agent starts in that state and follows the policy until the end.
- **The action-value function.** In contrast to the state-value function, the action-value calculates for each state and action pair the expected return if the agent starts in that state and takes an action. Then it follows the policy forever after.

### Epsilon-greedy strategy:

- Common exploration strategy used in reinforcement learning that involves balancing exploration and exploitation.
- Chooses the action with the highest expected reward with a probability of 1-epsilon.
- Chooses a random action with a probability of epsilon.
- Epsilon is typically decreased over time to shift focus towards exploitation.

### Greedy strategy:

- Involves always choosing the action that is expected to lead to the highest reward, based on the current knowledge of the environment. (only exploitation)
- Always chooses the action with the highest expected reward.
- Does not include any exploration.
- Can be disadvantageous in environments with uncertainty or unknown optimal actions.


If you want to improve the course, you can [open a Pull Request.](https://github.com/huggingface/deep-rl-class/pulls)

This glossary was made possible thanks to:

- [Ramón Rueda](https://github.com/ramon-rd)
- [Hasarindu Perera](https://github.com/hasarinduperera/)

# Hands-on [[hands-on]]

      <CourseFloatingBanner classNames="absolute z-10 right-0 top-0"
      notebooks={[
        {label: "Google Colab", value: "https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/unit2/unit2.ipynb"}
        ]}
        askForHelpUrl="http://hf.co/join/discord" />



Now that we studied the Q-Learning algorithm, let's implement it from scratch and train our Q-Learning agent in two environments:

1. [Frozen-Lake-v1  (non-slippery and slippery version)](https://www.gymlibrary.dev/environments/toy_text/frozen_lake/) ☃️ : where our agent will need to **go from the starting state (S) to the goal state (G)** by walking only on frozen tiles (F) and avoiding holes (H).
2. [An autonomous taxi](https://www.gymlibrary.dev/environments/toy_text/taxi/) 🚖 will need **to learn to navigate** a city to **transport its passengers from point A to point B.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/envs.gif" alt="Environments"/>

Thanks to a [leaderboard](https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard), you'll be able to compare your results with other classmates and exchange the best practices to improve your agent's scores. Who will win the challenge for Unit 2?

To validate this hands-on for the [certification process](https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process), you need to push your trained Taxi model to the Hub and **get a result of >= 4.5**.

To find your result, go to the [leaderboard](https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard) and find your model, **the result = mean_reward - std of reward**

For more information about the certification process, check this section 👉 https://huggingface.co/deep-rl-course/en/unit0/introduction#certification-process

And you can check your progress here 👉 https://huggingface.co/spaces/ThomasSimonini/Check-my-progress-Deep-RL-Course


**To start the hands-on click on Open In Colab button** 👇 :

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/huggingface/deep-rl-class/blob/master/notebooks/unit2/unit2.ipynb)


# Unit 2: Q-Learning with FrozenLake-v1 ⛄ and Taxi-v3 🚕

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/thumbnail.jpg" alt="Unit 2 Thumbnail">

In this notebook, **you'll code from scratch your first Reinforcement Learning agent** playing FrozenLake ❄️ using Q-Learning, share it to the community, and experiment with different configurations.


⬇️ Here is an example of what **you will achieve in just a couple of minutes.** ⬇️


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/envs.gif" alt="Environments"/>

### 🎮 Environments:

- [FrozenLake-v1](https://www.gymlibrary.dev/environments/toy_text/frozen_lake/)
- [Taxi-v3](https://www.gymlibrary.dev/environments/toy_text/taxi/)

### 📚 RL-Library:

- Python and NumPy
- [Gym](https://www.gymlibrary.dev/)

We're constantly trying to improve our tutorials, so **if you find some issues in this notebook**, please [open an issue on the GitHub Repo](https://github.com/huggingface/deep-rl-class/issues).

## Objectives of this notebook 🏆

At the end of the notebook, you will:

- Be able to use **Gym**, the environment library.
- Be able to code from scratch a Q-Learning agent.
- Be able to **push your trained agent and the code to the Hub** with a nice video replay and an evaluation score 🔥.


## Prerequisites 🏗️

Before diving into the notebook, you need to:

🔲 📚 **Study [Q-Learning by reading Unit 2](https://huggingface.co/deep-rl-course/unit2/introduction)**  🤗

## A small recap of Q-Learning

- The *Q-Learning* **is the RL algorithm that**

  - Trains *Q-Function*, an **action-value function** that contains, as internal memory, a *Q-table* **that contains all the state-action pair values.**

  - Given a state and action, our Q-Function **will search into its Q-table the corresponding value.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-function-2.jpg" alt="Q function"  width="100%"/>

- When the training is done,**we have an optimal Q-Function, so an optimal Q-Table.**

- And if we **have an optimal Q-function**, we
  have an optimal policy,since we **know for each state, what is the best action to take.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="Link value policy"  width="100%"/>


But, in the beginning, our **Q-Table is useless since it gives arbitrary value for each state-action pair (most of the time we initialize the Q-Table to 0 values)**. But, as we’ll explore the environment and update our Q-Table it will give us better and better approximations

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/q-learning.jpeg" alt="q-learning.jpeg" width="100%"/>

This is the Q-Learning pseudocode:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-2.jpg" alt="Q-Learning" width="100%"/>


# Let's code our first Reinforcement Learning algorithm 🚀

## Install dependencies and create a virtual display 🔽

In the notebook, we'll need to generate a replay video. To do so, with Colab, **we need to have a virtual screen to render the environment** (and thus record the frames).

Hence the following cell will install the libraries and create and run a virtual screen 🖥

We’ll install multiple ones:

- `gym`: Contains the FrozenLake-v1 ⛄ and Taxi-v3 🚕 environments. We use `gym==0.24` since it contains a nice Taxi-v3 UI version.
- `pygame`: Used for the FrozenLake-v1 and Taxi-v3 UI.
- `numpy`: Used for handling our Q-table.

The Hugging Face Hub 🤗 works as a central place where anyone can share and explore models and datasets. It has versioning, metrics, visualizations and other features that will allow you to easily collaborate with others.

You can see here all the Deep RL models available (if they use Q Learning) 👉 https://huggingface.co/models?other=q-learning

```bash
pip install -r https://raw.githubusercontent.com/huggingface/deep-rl-class/main/notebooks/unit2/requirements-unit2.txt
```

```bash
sudo apt-get update
apt install python-opengl ffmpeg xvfb
pip3 install pyvirtualdisplay
```

To make sure the new installed libraries are used, **sometimes it's required to restart the notebook runtime**. The next cell will force the **runtime to crash, so you'll need to connect again and run the code starting from here**. Thanks for this trick, **we will be able to run our virtual screen.**

```python
import os

os.kill(os.getpid(), 9)
```

```python
# Virtual display
from pyvirtualdisplay import Display

virtual_display = Display(visible=0, size=(1400, 900))
virtual_display.start()
```

## Import the packages 📦

In addition to the installed libraries, we also use:

- `random`: To generate random numbers (that will be useful for epsilon-greedy policy).
- `imageio`: To generate a replay video.

```python
import numpy as np
import gym
import random
import imageio
import os

import pickle5 as pickle
from tqdm.notebook import tqdm
```

We're now ready to code our Q-Learning algorithm 🔥

# Part 1: Frozen Lake ⛄ (non slippery version)

## Create and understand [FrozenLake environment ⛄](https://www.gymlibrary.dev/environments/toy_text/frozen_lake/)

---

💡 A good habit when you start to use an environment is to check its documentation

👉 https://www.gymlibrary.dev/environments/toy_text/frozen_lake/

---

We're going to train our Q-Learning agent **to navigate from the starting state (S) to the goal state (G) by walking only on frozen tiles (F) and avoid holes (H)**.

We can have two sizes of environment:

- `map_name="4x4"`: a 4x4 grid version
- `map_name="8x8"`: a 8x8 grid version


The environment has two modes:

- `is_slippery=False`: The agent always moves **in the intended direction** due to the non-slippery nature of the frozen lake (deterministic).
- `is_slippery=True`: The agent **may not always move in the intended direction** due to the slippery nature of the frozen lake (stochastic).

For now let's keep it simple with the 4x4 map and non-slippery

```python
# Create the FrozenLake-v1 environment using 4x4 map and non-slippery version
env = gym.make()  # TODO use the correct parameters
```

### Solution

```python
env = gym.make("FrozenLake-v1", map_name="4x4", is_slippery=False)
```

You can create your own custom grid like this:

```python
desc=["SFFF", "FHFH", "FFFH", "HFFG"]
gym.make('FrozenLake-v1', desc=desc, is_slippery=True)
```

but we'll use the default environment for now.

### Let's see what the Environment looks like:


```python
# We create our environment with gym.make("<name_of_the_environment>")- `is_slippery=False`: The agent always moves in the intended direction due to the non-slippery nature of the frozen lake (deterministic).
print("_____OBSERVATION SPACE_____ \n")
print("Observation Space", env.observation_space)
print("Sample observation", env.observation_space.sample())  # Get a random observation
```

We see with `Observation Space Shape Discrete(16)` that the observation is an integer representing the **agent’s current position as current_row * nrows + current_col (where both the row and col start at 0)**.

For example, the goal position in the 4x4 map can be calculated as follows: 3 * 4 + 3 = 15. The number of possible observations is dependent on the size of the map. **For example, the 4x4 map has 16 possible observations.**


For instance, this is what state = 0 looks like:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/frozenlake.png" alt="FrozenLake">

```python
print("\n _____ACTION SPACE_____ \n")
print("Action Space Shape", env.action_space.n)
print("Action Space Sample", env.action_space.sample())  # Take a random action
```

The action space (the set of possible actions the agent can take) is discrete with 4 actions available 🎮:

- 0: GO LEFT
- 1: GO DOWN
- 2: GO RIGHT
- 3: GO UP

Reward function 💰:

- Reach goal: +1
- Reach hole: 0
- Reach frozen: 0

## Create and Initialize the Q-table 🗄️

(👀 Step 1 of the pseudocode)

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-2.jpg" alt="Q-Learning" width="100%"/>


It's time to initialize our Q-table! To know how many rows (states) and columns (actions) to use, we need to know the action and observation space. We already know their values from before, but we'll want to obtain them programmatically so that our algorithm generalizes for different environments. Gym provides us a way to do that: `env.action_space.n` and `env.observation_space.n`


```python
state_space =
print("There are ", state_space, " possible states")

action_space =
print("There are ", action_space, " possible actions")
```

```python
# Let's create our Qtable of size (state_space, action_space) and initialized each values at 0 using np.zeros
def initialize_q_table(state_space, action_space):
  Qtable =
  return Qtable
```

```python
Qtable_frozenlake = initialize_q_table(state_space, action_space)
```

### Solution

```python
state_space = env.observation_space.n
print("There are ", state_space, " possible states")

action_space = env.action_space.n
print("There are ", action_space, " possible actions")
```

```python
# Let's create our Qtable of size (state_space, action_space) and initialized each values at 0 using np.zeros
def initialize_q_table(state_space, action_space):
    Qtable = np.zeros((state_space, action_space))
    return Qtable
```

```python
Qtable_frozenlake = initialize_q_table(state_space, action_space)
```

## Define the greedy policy 🤖

Remember we have two policies since Q-Learning is an **off-policy** algorithm. This means we're using a **different policy for acting and updating the value function**.

- Epsilon-greedy policy (acting policy)
- Greedy-policy (updating policy)

Greedy policy will also be the final policy we'll have when the Q-learning agent will be trained. The greedy policy is used to select an action from the Q-table.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-4.jpg" alt="Q-Learning" width="100%"/>


```python
def greedy_policy(Qtable, state):
  # Exploitation: take the action with the highest state, action value
  action =

  return action
```

#### Solution

```python
def greedy_policy(Qtable, state):
    # Exploitation: take the action with the highest state, action value
    action = np.argmax(Qtable[state][:])

    return action
```

##Define the epsilon-greedy policy 🤖

Epsilon-greedy is the training policy that handles the exploration/exploitation trade-off.

The idea with epsilon-greedy:

- With *probability 1 - ɛ* : **we do exploitation** (i.e. our agent selects the action with the highest state-action pair value).

- With *probability ɛ*: we do **exploration** (trying random action).

And as the training goes, we progressively **reduce the epsilon value since we will need less and less exploration and more exploitation.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-4.jpg" alt="Q-Learning" width="100%"/>


```python
def epsilon_greedy_policy(Qtable, state, epsilon):
  # Randomly generate a number between 0 and 1
  random_num =
  # if random_num > greater than epsilon --> exploitation
  if random_num > epsilon:
    # Take the action with the highest value given a state
    # np.argmax can be useful here
    action =
  # else --> exploration
  else:
    action = # Take a random action

  return action
```

#### Solution

```python
def epsilon_greedy_policy(Qtable, state, epsilon):
    # Randomly generate a number between 0 and 1
    random_int = random.uniform(0, 1)
    # if random_int > greater than epsilon --> exploitation
    if random_int > epsilon:
        # Take the action with the highest value given a state
        # np.argmax can be useful here
        action = greedy_policy(Qtable, state)
    # else --> exploration
    else:
        action = env.action_space.sample()

    return action
```

## Define the hyperparameters ⚙️

The exploration related hyperparamters are some of the most important ones.

- We need to make sure that our agent **explores enough of the state space** to learn a good value approximation. To do that, we need to have progressive decay of the epsilon.
- If you decrease epsilon too fast (too high decay_rate), **you take the risk that your agent will be stuck**, since your agent didn't explore enough of the state space and hence can't solve the problem.

```python
# Training parameters
n_training_episodes = 10000  # Total training episodes
learning_rate = 0.7  # Learning rate

# Evaluation parameters
n_eval_episodes = 100  # Total number of test episodes

# Environment parameters
env_id = "FrozenLake-v1"  # Name of the environment
max_steps = 99  # Max steps per episode
gamma = 0.95  # Discounting rate
eval_seed = []  # The evaluation seed of the environment

# Exploration parameters
max_epsilon = 1.0  # Exploration probability at start
min_epsilon = 0.05  # Minimum exploration probability
decay_rate = 0.0005  # Exponential decay rate for exploration prob
```

## Create the training loop method

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-2.jpg" alt="Q-Learning" width="100%"/>

The training loop goes like this:

```
For episode in the total of training episodes:

Reduce epsilon (since we need less and less exploration)
Reset the environment

  For step in max timesteps:
    Choose the action At using epsilon greedy policy
    Take the action (a) and observe the outcome state(s') and reward (r)
    Update the Q-value Q(s,a) using Bellman equation Q(s,a) + lr [R(s,a) + gamma * max Q(s',a') - Q(s,a)]
    If done, finish the episode
    Our next state is the new state
```

```python
def train(n_training_episodes, min_epsilon, max_epsilon, decay_rate, env, max_steps, Qtable):
  for episode in range(n_training_episodes):
    # Reduce epsilon (because we need less and less exploration)
    epsilon = min_epsilon + (max_epsilon - min_epsilon)*np.exp(-decay_rate*episode)
    # Reset the environment
    state = env.reset()
    step = 0
    done = False

    # repeat
    for step in range(max_steps):
      # Choose the action At using epsilon greedy policy
      action =

      # Take action At and observe Rt+1 and St+1
      # Take the action (a) and observe the outcome state(s') and reward (r)
      new_state, reward, done, info =

      # Update Q(s,a):= Q(s,a) + lr [R(s,a) + gamma * max Q(s',a') - Q(s,a)]
      Qtable[state][action] =

      # If done, finish the episode
      if done:
        break

      # Our next state is the new state
      state = new_state
  return Qtable
```

#### Solution

```python
def train(n_training_episodes, min_epsilon, max_epsilon, decay_rate, env, max_steps, Qtable):
    for episode in tqdm(range(n_training_episodes)):
        # Reduce epsilon (because we need less and less exploration)
        epsilon = min_epsilon + (max_epsilon - min_epsilon) * np.exp(-decay_rate * episode)
        # Reset the environment
        state = env.reset()
        step = 0
        done = False

        # repeat
        for step in range(max_steps):
            # Choose the action At using epsilon greedy policy
            action = epsilon_greedy_policy(Qtable, state, epsilon)

            # Take action At and observe Rt+1 and St+1
            # Take the action (a) and observe the outcome state(s') and reward (r)
            new_state, reward, done, info = env.step(action)

            # Update Q(s,a):= Q(s,a) + lr [R(s,a) + gamma * max Q(s',a') - Q(s,a)]
            Qtable[state][action] = Qtable[state][action] + learning_rate * (
                reward + gamma * np.max(Qtable[new_state]) - Qtable[state][action]
            )

            # If done, finish the episode
            if done:
                break

            # Our next state is the new state
            state = new_state
    return Qtable
```

## Train the Q-Learning agent 🏃

```python
Qtable_frozenlake = train(n_training_episodes, min_epsilon, max_epsilon, decay_rate, env, max_steps, Qtable_frozenlake)
```

## Let's see what our Q-Learning table looks like now 👀

```python
Qtable_frozenlake
```

## The evaluation method 📝

- We defined the evaluation method that we're going to use to test our Q-Learning agent.

```python
def evaluate_agent(env, max_steps, n_eval_episodes, Q, seed):
    """
    Evaluate the agent for ``n_eval_episodes`` episodes and returns average reward and std of reward.
    :param env: The evaluation environment
    :param n_eval_episodes: Number of episode to evaluate the agent
    :param Q: The Q-table
    :param seed: The evaluation seed array (for taxi-v3)
    """
    episode_rewards = []
    for episode in tqdm(range(n_eval_episodes)):
        if seed:
            state = env.reset(seed=seed[episode])
        else:
            state = env.reset()
        step = 0
        done = False
        total_rewards_ep = 0

        for step in range(max_steps):
            # Take the action (index) that have the maximum expected future reward given that state
            action = greedy_policy(Q, state)
            new_state, reward, done, info = env.step(action)
            total_rewards_ep += reward

            if done:
                break
            state = new_state
        episode_rewards.append(total_rewards_ep)
    mean_reward = np.mean(episode_rewards)
    std_reward = np.std(episode_rewards)

    return mean_reward, std_reward
```

## Evaluate our Q-Learning agent 📈

- Usually, you should have a mean reward of 1.0
- The **environment is relatively easy** since the state space is really small (16). What you can try to do is [to replace it with the slippery version](https://www.gymlibrary.dev/environments/toy_text/frozen_lake/), which introduces stochasticity, making the environment more complex.

```python
# Evaluate our Agent
mean_reward, std_reward = evaluate_agent(env, max_steps, n_eval_episodes, Qtable_frozenlake, eval_seed)
print(f"Mean_reward={mean_reward:.2f} +/- {std_reward:.2f}")
```

## Publish our trained model to the Hub 🔥

Now that we saw good results after the training, **we can publish our trained model to the Hub 🤗 with one line of code**.

Here's an example of a Model Card:

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/modelcard.png" alt="Model card" width="100%"/>


Under the hood, the Hub uses git-based repositories (don't worry if you don't know what git is), which means you can update the model with new versions as you experiment and improve your agent.

#### Do not modify this code

```python
from huggingface_hub import HfApi, snapshot_download
from huggingface_hub.repocard import metadata_eval_result, metadata_save

from pathlib import Path
import datetime
import json
```

```python
def record_video(env, Qtable, out_directory, fps=1):
    """
    Generate a replay video of the agent
    :param env
    :param Qtable: Qtable of our agent
    :param out_directory
    :param fps: how many frame per seconds (with taxi-v3 and frozenlake-v1 we use 1)
    """
    images = []
    done = False
    state = env.reset(seed=random.randint(0, 500))
    img = env.render(mode="rgb_array")
    images.append(img)
    while not done:
        # Take the action (index) that have the maximum expected future reward given that state
        action = np.argmax(Qtable[state][:])
        state, reward, done, info = env.step(action)  # We directly put next_state = state for recording logic
        img = env.render(mode="rgb_array")
        images.append(img)
    imageio.mimsave(out_directory, [np.array(img) for i, img in enumerate(images)], fps=fps)
```

```python
def push_to_hub(repo_id, model, env, video_fps=1, local_repo_path="hub"):
    """
    Evaluate, Generate a video and Upload a model to Hugging Face Hub.
    This method does the complete pipeline:
    - It evaluates the model
    - It generates the model card
    - It generates a replay video of the agent
    - It pushes everything to the Hub

    :param repo_id: repo_id: id of the model repository from the Hugging Face Hub
    :param env
    :param video_fps: how many frame per seconds to record our video replay
    (with taxi-v3 and frozenlake-v1 we use 1)
    :param local_repo_path: where the local repository is
    """
    _, repo_name = repo_id.split("/")

    eval_env = env
    api = HfApi()

    # Step 1: Create the repo
    repo_url = api.create_repo(
        repo_id=repo_id,
        exist_ok=True,
    )

    # Step 2: Download files
    repo_local_path = Path(snapshot_download(repo_id=repo_id))

    # Step 3: Save the model
    if env.spec.kwargs.get("map_name"):
        model["map_name"] = env.spec.kwargs.get("map_name")
        if env.spec.kwargs.get("is_slippery", "") == False:
            model["slippery"] = False

    # Pickle the model
    with open((repo_local_path) / "q-learning.pkl", "wb") as f:
        pickle.dump(model, f)

    # Step 4: Evaluate the model and build JSON with evaluation metrics
    mean_reward, std_reward = evaluate_agent(
        eval_env, model["max_steps"], model["n_eval_episodes"], model["qtable"], model["eval_seed"]
    )

    evaluate_data = {
        "env_id": model["env_id"],
        "mean_reward": mean_reward,
        "n_eval_episodes": model["n_eval_episodes"],
        "eval_datetime": datetime.datetime.now().isoformat(),
    }

    # Write a JSON file called "results.json" that will contain the
    # evaluation results
    with open(repo_local_path / "results.json", "w") as outfile:
        json.dump(evaluate_data, outfile)

    # Step 5: Create the model card
    env_name = model["env_id"]
    if env.spec.kwargs.get("map_name"):
        env_name += "-" + env.spec.kwargs.get("map_name")

    if env.spec.kwargs.get("is_slippery", "") == False:
        env_name += "-" + "no_slippery"

    metadata = {}
    metadata["tags"] = [env_name, "q-learning", "reinforcement-learning", "custom-implementation"]

    # Add metrics
    eval = metadata_eval_result(
        model_pretty_name=repo_name,
        task_pretty_name="reinforcement-learning",
        task_id="reinforcement-learning",
        metrics_pretty_name="mean_reward",
        metrics_id="mean_reward",
        metrics_value=f"{mean_reward:.2f} +/- {std_reward:.2f}",
        dataset_pretty_name=env_name,
        dataset_id=env_name,
    )

    # Merges both dictionaries
    metadata = {**metadata, **eval}

    model_card = f"""
    # **Q-Learning** Agent playing1 **{env_id}**
    This is a trained model of a **Q-Learning** agent playing **{env_id}** .

    ## Usage

    ```python

    model = load_from_hub(repo_id="{repo_id}", filename="q-learning.pkl")

    # Don't forget to check if you need to add additional attributes (is_slippery=False etc)
    env = gym.make(model["env_id"])
```

    """
    
    evaluate_agent(env, model["max_steps"], model["n_eval_episodes"], model["qtable"], model["eval_seed"])
    
    readme_path = repo_local_path / "README.md"
    readme = ""
    print(readme_path.exists())
    if readme_path.exists():
        with readme_path.open("r", encoding="utf8") as f:
            readme = f.read()
    else:
        readme = model_card
    
    with readme_path.open("w", encoding="utf-8") as f:
        f.write(readme)
    
    # Save our metrics to Readme metadata
    metadata_save(readme_path, metadata)
    
    # Step 6: Record a video
    video_path = repo_local_path / "replay.mp4"
    record_video(env, model["qtable"], video_path, video_fps)
    
    # Step 7. Push everything to the Hub
    api.upload_folder(
        repo_id=repo_id,
        folder_path=repo_local_path,
        path_in_repo=".",
    )
    
    print("Your model is pushed to the Hub. You can view your model here: ", repo_url)

```
### .

By using `push_to_hub` **you evaluate, record a replay, generate a model card of your agent and push it to the Hub**.

This way:
- You can **showcase our work** 🔥
- You can **visualize your agent playing** 👀
- You can **share with the community an agent that others can use** 💾
- You can **access a leaderboard 🏆 to see how well your agent is performing compared to your classmates** 👉 https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard


To be able to share your model with the community there are three more steps to follow:

1️⃣ (If it's not already done) create an account to HF ➡ https://huggingface.co/join

2️⃣ Sign in and then, you need to store your authentication token from the Hugging Face website.
- Create a new token (https://huggingface.co/settings/tokens) **with write role**


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/create-token.jpg" alt="Create HF Token">


```python
from huggingface_hub import notebook_login

notebook_login()
```

If you don't want to use a Google Colab or a Jupyter Notebook, you need to use this command instead: `huggingface-cli login` (or `login`)

3️⃣ We're now ready to push our trained agent to the 🤗 Hub 🔥 using `push_to_hub()` function

- Let's create **the model dictionary that contains the hyperparameters and the Q_table**.

```python
model = {
    "env_id": env_id,
    "max_steps": max_steps,
    "n_training_episodes": n_training_episodes,
    "n_eval_episodes": n_eval_episodes,
    "eval_seed": eval_seed,
    "learning_rate": learning_rate,
    "gamma": gamma,
    "max_epsilon": max_epsilon,
    "min_epsilon": min_epsilon,
    "decay_rate": decay_rate,
    "qtable": Qtable_frozenlake,
}
```

Let's fill the `push_to_hub` function:

- `repo_id`: the name of the Hugging Face Hub Repository that will be created/updated `
  (repo_id = {username}/{repo_name})`
  💡 A good `repo_id` is `{username}/q-{env_id}`
- `model`: our model dictionary containing the hyperparameters and the Qtable.
- `env`: the environment.
- `commit_message`: message of the commit

```python
model
```

```python
username = ""  # FILL THIS
repo_name = "q-FrozenLake-v1-4x4-noSlippery"
push_to_hub(repo_id=f"{username}/{repo_name}", model=model, env=env)
```

Congrats 🥳 you've just implemented from scratch, trained and uploaded your first Reinforcement Learning agent.
FrozenLake-v1 no_slippery is very simple environment, let's try an harder one 🔥.

# Part 2: Taxi-v3 🚖

## Create and understand [Taxi-v3 🚕](https://www.gymlibrary.dev/environments/toy_text/taxi/)

---

💡 A good habit when you start to use an environment is to check its documentation

👉 https://www.gymlibrary.dev/environments/toy_text/taxi/

---

In `Taxi-v3` 🚕, there are four designated locations in the grid world indicated by R(ed), G(reen), Y(ellow), and B(lue).

When the episode starts, **the taxi starts off at a random square** and the passenger is at a random location. The taxi drives to the passenger’s location, **picks up the passenger**, drives to the passenger’s destination (another one of the four specified locations), and then **drops off the passenger**. Once the passenger is dropped off, the episode ends.


<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/taxi.png" alt="Taxi">


```python
env = gym.make("Taxi-v3")
```

There are **500 discrete states since there are 25 taxi positions, 5 possible locations of the passenger** (including the case when the passenger is in the taxi), and **4 destination locations.**


```python
state_space = env.observation_space.n
print("There are ", state_space, " possible states")
```

```python
action_space = env.action_space.n
print("There are ", action_space, " possible actions")
```

The action space (the set of possible actions the agent can take) is discrete with **6 actions available 🎮**:

- 0: move south
- 1: move north
- 2: move east
- 3: move west
- 4: pickup passenger
- 5: drop off passenger

Reward function 💰:

- -1 per step unless other reward is triggered.
- +20 delivering passenger.
- -10 executing “pickup” and “drop-off” actions illegally.

```python
# Create our Q table with state_size rows and action_size columns (500x6)
Qtable_taxi = initialize_q_table(state_space, action_space)
print(Qtable_taxi)
print("Q-table shape: ", Qtable_taxi.shape)
```

## Define the hyperparameters ⚙️

⚠ DO NOT MODIFY EVAL_SEED: the eval_seed array **allows us to evaluate your agent with the same taxi starting positions for every classmate**

```python
# Training parameters
n_training_episodes = 25000  # Total training episodes
learning_rate = 0.7  # Learning rate

# Evaluation parameters
n_eval_episodes = 100  # Total number of test episodes

# DO NOT MODIFY EVAL_SEED
eval_seed = [
    16,
    54,
    165,
    177,
    191,
    191,
    120,
    80,
    149,
    178,
    48,
    38,
    6,
    125,
    174,
    73,
    50,
    172,
    100,
    148,
    146,
    6,
    25,
    40,
    68,
    148,
    49,
    167,
    9,
    97,
    164,
    176,
    61,
    7,
    54,
    55,
    161,
    131,
    184,
    51,
    170,
    12,
    120,
    113,
    95,
    126,
    51,
    98,
    36,
    135,
    54,
    82,
    45,
    95,
    89,
    59,
    95,
    124,
    9,
    113,
    58,
    85,
    51,
    134,
    121,
    169,
    105,
    21,
    30,
    11,
    50,
    65,
    12,
    43,
    82,
    145,
    152,
    97,
    106,
    55,
    31,
    85,
    38,
    112,
    102,
    168,
    123,
    97,
    21,
    83,
    158,
    26,
    80,
    63,
    5,
    81,
    32,
    11,
    28,
    148,
]  # Evaluation seed, this ensures that all classmates agents are trained on the same taxi starting position
# Each seed has a specific starting state

# Environment parameters
env_id = "Taxi-v3"  # Name of the environment
max_steps = 99  # Max steps per episode
gamma = 0.95  # Discounting rate

# Exploration parameters
max_epsilon = 1.0  # Exploration probability at start
min_epsilon = 0.05  # Minimum exploration probability
decay_rate = 0.005  # Exponential decay rate for exploration prob
```

## Train our Q-Learning agent 🏃

```python
Qtable_taxi = train(n_training_episodes, min_epsilon, max_epsilon, decay_rate, env, max_steps, Qtable_taxi)
Qtable_taxi
```

## Create a model dictionary 💾 and publish our trained model to the Hub 🔥

- We create a model dictionary that will contain all the training hyperparameters for reproducibility and the Q-Table.


```python
model = {
    "env_id": env_id,
    "max_steps": max_steps,
    "n_training_episodes": n_training_episodes,
    "n_eval_episodes": n_eval_episodes,
    "eval_seed": eval_seed,
    "learning_rate": learning_rate,
    "gamma": gamma,
    "max_epsilon": max_epsilon,
    "min_epsilon": min_epsilon,
    "decay_rate": decay_rate,
    "qtable": Qtable_taxi,
}
```

```python
username = ""  # FILL THIS
repo_name = ""
push_to_hub(repo_id=f"{username}/{repo_name}", model=model, env=env)
```

Now that's on the Hub, you can compare the results of your Taxi-v3 with your classmates using the leaderboard 🏆 👉 https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard

⚠ To see your entry, you need to go to the bottom of the leaderboard page and **click on refresh** ⚠

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/taxi-leaderboard.png" alt="Taxi Leaderboard">

# Part 3: Load from Hub 🔽

What's amazing with Hugging Face Hub 🤗 is that you can easily load powerful models from the community.

Loading a saved model from the Hub is really easy:

1. You go https://huggingface.co/models?other=q-learning to see the list of all the q-learning saved models.
2. You select one and copy its repo_id

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/notebooks/unit2/copy-id.png" alt="Copy id">

3. Then we just need to use `load_from_hub` with:

- The repo_id
- The filename: the saved model inside the repo.

#### Do not modify this code

```python
from urllib.error import HTTPError

from huggingface_hub import hf_hub_download


def load_from_hub(repo_id: str, filename: str) -> str:
    """
    Download a model from Hugging Face Hub.
    :param repo_id: id of the model repository from the Hugging Face Hub
    :param filename: name of the model zip file from the repository
    """
    # Get the model from the Hub, download and cache the model on your local disk
    pickle_model = hf_hub_download(repo_id=repo_id, filename=filename)

    with open(pickle_model, "rb") as f:
        downloaded_model_file = pickle.load(f)

    return downloaded_model_file
```

### .

```python
model = load_from_hub(repo_id="ThomasSimonini/q-Taxi-v3", filename="q-learning.pkl")  # Try to use another model

print(model)
env = gym.make(model["env_id"])

evaluate_agent(env, model["max_steps"], model["n_eval_episodes"], model["qtable"], model["eval_seed"])
```

```python
model = load_from_hub(
    repo_id="ThomasSimonini/q-FrozenLake-v1-no-slippery", filename="q-learning.pkl"
)  # Try to use another model

env = gym.make(model["env_id"], is_slippery=False)

evaluate_agent(env, model["max_steps"], model["n_eval_episodes"], model["qtable"], model["eval_seed"])
```

## Some additional challenges 🏆

The best way to learn **is to try things by your own**! As you saw, the current agent is not doing great. As a first suggestion, you can train for more steps. With 1,000,000 steps, we saw some great results!

In the [Leaderboard](https://huggingface.co/spaces/huggingface-projects/Deep-Reinforcement-Learning-Leaderboard) you will find your agents. Can you get to the top?

Here are some ideas to achieve so:

* Train more steps
* Try different hyperparameters by looking at what your classmates have done.
* **Push your new trained model** on the Hub 🔥

Are walking on ice and driving taxis too boring to you? Try to **change the environment**, why not using FrozenLake-v1 slippery version? Check how they work [using the gym documentation](https://www.gymlibrary.dev/) and have fun 🎉.

_____________________________________________________________________

Congrats 🥳, you've just implemented, trained, and uploaded your first Reinforcement Learning agent.

Understanding Q-Learning is an **important step to understanding value-based methods.**

In the next Unit with Deep Q-Learning, we'll see that creating and updating a Q-table was a good strategy — **however, this is not scalable.**

For instance, imagine you create an agent that learns to play Doom.

<img src="https://vizdoom.cs.put.edu.pl/user/pages/01.tutorial/basic.png" alt="Doom"/>

Doom is a large environment with a huge state space (millions of different states). Creating and updating a Q-table for that environment would not be efficient.

That's why we'll study, in the next unit, Deep Q-Learning, an algorithm **where we use a neural network that approximates, given a state, the different Q-values for each action.**

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit4/atari-envs.gif" alt="Environments"/>


See you on Unit 3! 🔥

## Keep learning, stay awesome 🤗

# Second Quiz [[quiz2]]

The best way to learn and [to avoid the illusion of competence](https://www.coursera.org/lecture/learning-how-to-learn/illusions-of-competence-BuFzf) **is to test yourself.** This will help you to find **where you need to reinforce your knowledge**.


### Q1: What is Q-Learning?


<Question
    choices={[
        {
            text: "The algorithm we use to train our Q-function",
            explain: "",
      correct: true
        },
        {
            text: "A value function",
            explain: "It's an action-value function since it determines the value of being at a particular state and taking a specific action at that state",
        },
    {
            text: "An algorithm that determines the value of being at a particular state and taking a specific action at that state",
            explain: "",
      correct: true
        },
        {
            text: "A table",
      explain: "Q-function is not a Q-table. The Q-function is the algorithm that will feed the Q-table."
        }
    ]}
/>

### Q2: What is a Q-table?

<Question
    choices={[
        {
            text: "An algorithm we use in Q-Learning",
            explain: "",
        },
        {
            text: "Q-table is the internal memory of our agent",
            explain: "",
      correct: true
        },
    {
            text: "In Q-table each cell corresponds a state value",
            explain: "Each cell corresponds to a state-action value pair value. Not a state value.",
        }
    ]}
/>

### Q3: Why if we have an optimal Q-function Q* we have an optimal policy?

<details>
<summary>Solution</summary>

Because if we have an optimal Q-function, we have an optimal policy since we know for each state what is the best action to take.

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/link-value-policy.jpg" alt="link value policy"/>

</details>

### Q4: Can you explain what is Epsilon-Greedy Strategy?

<details>
<summary>Solution</summary>
Epsilon Greedy Strategy is a policy that handles the exploration/exploitation trade-off.

The idea is that we define epsilon ɛ = 1.0:

- With *probability 1 — ɛ* : we do exploitation (aka our agent selects the action with the highest state-action pair value).
- With *probability ɛ* : we do exploration (trying random action).

<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/Q-learning-4.jpg" alt="Epsilon Greedy"/>


</details>

### Q5: How do we update the Q value of a state, action pair?
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-update-ex.jpg" alt="Q Update exercise"/>

<details>
<summary>Solution</summary>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/q-update-solution.jpg" alt="Q Update exercise"/>

</details>



### Q6: What's the difference between on-policy and off-policy

<details>
<summary>Solution</summary>
<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit3/off-on-4.jpg" alt="On/off policy"/>
</details>

Congrats on finishing this Quiz 🥳, if you missed some elements, take time to read again the chapter to reinforce (😏) your knowledge.

**#** **Conclusion** **[****[conclusion****]****]**



Congrats on finishing this chapter! There was a lot of information. And congrats on finishing the tutorials. You’ve just implemented your first RL agent from scratch and shared it on the Hub 🥳.



Implementing from scratch when you study a new architecture ***\*****is important to understand how it works.*****\***



That’s ***\*****normal if you still feel confused*****\*** with all these elements. ***\*****This was the same for me and for all people who studied RL.*****\***



Take time to really grasp the material before continuing.





In the next chapter, we’re going to dive deeper by studying our first Deep Reinforcement Learning algorithm based on Q-Learning: Deep Q-Learning. And you'll train a ***\*****DQN agent with** **<****a** **href****=****"****https://github.com/DLR-RM/rl-baselines3-zoo****"****>****RL-Baselines3 Zoo****</****a****>** **to play Atari Games*****\***.





<img src="https://huggingface.co/datasets/huggingface-deep-rl-course/course-images/resolve/main/en/unit4/atari-envs.gif" alt="Atari environments"/>





Finally, we would love ***\*****to hear what you think of the course and how we can improve it*****\***. If you have some feedback then, please 👉  [fill this form](https://forms.gle/BzKXWzLAGZESGNaE9)



**###** **Keep Learning, stay awesome 🤗**

# Additional Readings [[additional-readings]]

These are **optional readings** if you want to go deeper.

## Monte Carlo and TD Learning [[mc-td]]

To dive deeper on Monte Carlo and Temporal Difference Learning:

- <a href="https://stats.stackexchange.com/questions/355820/why-do-temporal-difference-td-methods-have-lower-variance-than-monte-carlo-met">Why do temporal difference (TD) methods have lower variance than Monte Carlo methods?</a>
- <a href="https://stats.stackexchange.com/questions/336974/when-are-monte-carlo-methods-preferred-over-temporal-difference-ones"> When are Monte Carlo methods preferred over temporal difference ones?</a>

## Q-Learning [[q-learning]]

- <a href="http://incompleteideas.net/book/RLbook2020.pdf">Reinforcement Learning: An Introduction, Richard Sutton and Andrew G. Barto Chapter 5, 6 and 7</a>
- <a href="https://youtu.be/Psrhxy88zww">Foundations of Deep RL Series, L2 Deep Q-Learning by Pieter Abbeel</a>