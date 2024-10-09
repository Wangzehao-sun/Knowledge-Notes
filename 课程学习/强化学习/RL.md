## 强化学习笔记

https://datawhalechina.github.io/easy-rl/#/

**强化学习（Reinforcement Learning，RL）**

讨论的问题是智能体（agent）怎么在复杂、不确定的环境（environment）中最大化它能获得的奖励。

强化学习由两部分组成：智能体和环境。在强化学习过程中，智能体与环境一直在交互。智能体在环境中获取某个状态后，它会利用该状态输出一个动作 （action），这个动作也称为决策（decision）。然后这个动作会在环境中被执行，环境会根据智能体采取的动作，输出下一个状态以及当前这个动作带来的奖励。智能体的目的就是尽可能多地从环境中获取奖励。

<img src=".\attachments\image-20240921105831258.png" alt="image-20240921105831258" style="zoom:50%;" />