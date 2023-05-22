# DQN

## experience replay

### harmful correlations

in q-learning, due to the adjacent state representation, it shows harmful correlations, because they are so similar.

so neural network distinguish between them is very hard.

### solution

**use a experience replay buffer** to store a certain amount of experiences, and when the experience quantity reach to set value, we use replay buffer to sample the experience to train the agent

which resolve the harmful correlations

> We can prevent action values **from oscillating or diverging catastrophically** using a **large buffer of our past experience and sample training data from it**, instead of using our latest experience. 
>
> This technique is called **replay buffer** or **experience buffer**. 

## target network

### background

单独使用q-learning的结果就是，用一个猜测的结果去预测另一个猜测的结果

因为q-learning的计算是根据bellman方程计算得来的

### the reason of unstable

once we update the parameters to make q(s,a) to the desired result in one state, that also will indirectly alter the subsequent q(s',a') and other states nearby, which means one change many

so that's very unstable





https://zhuanlan.zhihu.com/p/260703124

https://github.com/sweetice/Deep-reinforcement-learning-with-pytorch/blob/master/Char01%20DQN/DQN.py

https://blog.csdn.net/qq_37395293/article/details/114154996  	
