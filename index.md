# NeurIPS 2021 Workshop  Review: Cooperative AI

### Introduction

The NeurIPS 2021 Cooperative AI workshop was organized with the goal of incentivizing cooperation in AI systems, and providing theoretical and empirical insights for how best to implement effective coordination given these incentives. In the context of multi-agent and human-agent cooperation, the papers we focus on discuss two key directions: 1) how to solve multi-agent cooperation problems under challenging and realistic conditions, such as learning under complex environments, or with a large number of agents, and 2) how to improve human-agent, and agent-agent collaboration efficiently through learning conventions, growing knowledge across generations, and inferring reward through observations.

The first direction aims to solve the challenges of training cooperative multi-agent systems, from learning in a partially non-observable environment to learning with a large number of agents where the joint observation space grows exponentially. The second direction aims to solve the challenges of efficient and aligned collaboration. For example, when an agent adapts to collaborate well with a human but the human does not offer clear preferences. In such situations quick adaptation is necessary.

Connecting to decision-making for robotics, the first direction is key for multi-robotics systems, or swarm robotics, where there are real-life applications such as search and rescue operations, environmental monitoring and cleanup, and warehouse logistics. For these tasks, a large number of robots are often needed for task success, and the robots face complex environments that are dynamic and partially observable. For example, in search and rescue operations robots need to cover a large area quickly to maximize the rate of search subject survival. In addition, the robots may be traversing unfamiliar terrain that also changes under weather conditions. Papers that consider complex environment conditions, and large-scale multi-agent learning pave the way toward applications for swarm robotics.

The second direction is key for building reliable and trustworthy robotics systems. As robots are deployed to work along other robots and work with humans, ensuring coordination that is aligned with humans efficiently is crucial. We can consider the application of robots aiding people with disabilities. To be able to aid the user correctly with a few demonstrations is important, therefore learning about the user’s needs, and learning conventions or sharing knowledge is important for adapting to human preferences which are not always clearly defined.

In the following sections we discuss the specific talks followed by our concluding remarks.

---

### **Learning to Coordinate in Complex Environment**

The authors start by pointing out common challenges for solving tasks that require coordination in a complex environment. These include:

1. Open, partially observable, dynamic environment: the exact state and environment is unknown and/or keep changing, making it difficult to coordinate.
2. No real-time global reward signal
3. Competition/cooperation: agents need to learn how to compete while also learning how to cooperate with teammates. For example robot soccer
4. Communication delay: due to limited communication bandwidth

To tackle some of these problems, they present a few works. In “Efficient Multi-agent communication” they observe that limiting the message bandwidth constrains the message’s entropy. Inspired by this observation, they compress the communication messages using the information bottleneck principle.

In “RMIX: Learning Risk-Sensitive Policies for Cooperative Reinforcement Learning Agents” they observed that global Q-value decomposition used by most MARL methods is based on expectation and is risk-neutral. Optimistic actions can impede team coordination. To solve this problem they propose to learn the distribution of future returns instead of direct future returns. They propose a general framework for risk-sensitive cooperative MARL which includes:

- Learning a risk-sensitive policy by applying CVaR to represent the agent’s policies
- Using a risk level controller for temporal adaption to encourage temporal consistency

The authors propose several noteworthy technical contributions to tackle some of the common problems with multi-agent coordination in complex environments. Their proposal to constrain the message bandwidth is important to increase message speed while maintaining the information in messages. Risk sensitivity is especially important for multi-agent cooperation since one agent’s overly optimistic Q-function can cause the whole team to fail at the task. Their proposed RMIX algorithm is an interesting approach to solve this problem.

There are still a lot of open problems. It is still unclear how to scale the proposed algorithms to more complex environments. It is also unclear how robust the algorithms are in stochastic environments where the agent does not know anything about the environment.

### **Interactive Inverse Reinforcement Learning for Cooperative Games**

The authors are interested in tackling AI-human collaboration problems. The agent is not sure what the human wants or what the human preferences are. Learn from the human and the agent interacting instead of just observing the human.

The task is set up so that two agents $A_1$ and $A_2$ collaborate. In the proposed algorithm, the learned policy is used to control agent $A_1$ but not agent $A_2$. Also, $A_1$ does not know nor observe the joint reward function R. The goal is still to maximize the expected sum of discounted rewards and the expectation is taken over the policies of both agents.

The goal is to learn the reward function that agent $A_2$ is trying to optimize. In their proposed setup the $A_1$ takes an action and then $A_2$ observes the action and takes its own action. $A_1$ observes $A_2$’s policy and computes a plausible reward function $R_t$. This can be thought of as $A_1$ implicitly designing an environment $P_{\pi_t^1}$ for agent $A_2$ to act in. Since $A_1$ designs the environment in which $A_2$ acts, it can design an environment in which it can learn $A_2$’s reward function. This is based on the assumption that demonstrations in some environments are more informative, so $A_1$ can create optimal environments to learn $A_2$’s reward function.

I think their contribution is interesting and can work in settings where $A_1$ has sufficient power to change the environment that $A_2$ acts in. This is not usually the case. In many MARL setups, all agents are acting concurrently and one agent does not have the power to alter the environment significantly enough to observe the other agents.

It is not clear how their proposed method would work for MARL setups with more than two agents. It would be interesting to extend this method to more than two agents where each agent is learning the reward function that the other agent is trying to maximize.

### **Learning to solve complex tasks by growing knowledge culturally across generations**

This work is inspired by the observation that AI systems are solipsistic, only having knowledge that they’ve been trained on. To alleviate this problem the researchers propose broad AI systems that teach and learn culturally from each other and from humans, by describing their knowledge and experiences in a human-interpretable way. They asked how efficient language is for passing on knowledge (vs learning from one’s own experience) when learners contribute new knowledge, what knowledge they convey, and how effective it is.

To answer these questions, they developed a game where each player is given only two attempts, and if they fail to perform the task they can document their experience and pass it down to the next player to help them. The players in the next generation are allowed to use instructions from the previous generation and can modify the instructions for the future generation. The authors found that future generations benefited from knowledge passed down from prior generations and their performance improved.

This work proposes a system for training collaborative AI systems. The idea is interesting and inspired by human experience. The results clearly show that passing information to future generations can help them succeed, which aligns with our intuition.

It’s not clear, however, how this can be applied to AI agents and the authors do not propose a solution. There’s also the open problem of figuring out if the observed results will hold for AI systems. The authors began the talk by asking how efficient language is for passing on knowledge and how such information passing can contribute to new knowledge. They answered the question for human subjects, however, future work needs to be done to be able to answer the question for actual AI agents.

### On the Approximation of Cooperative Heterogeneous Multi-Agent Reinforcement Learning (MARL) using Mean Field Control (MFC)

This talk introduces an algorithm using Mean Field Control that offers guarantees for the large-scale MARL. They study the MARL set up with the goal of learning a policy that instructs the agents to choose actions based on the joint observation such that the sum reward is maximized. This paper focuses on the cooperative problem between agents, and how to best solve the problem for many agents that are heterogeneous, or not the same.

The key contribution for the paper was that it’s the first scalable approach to solve cooperative heterogeneous MARL for a large population. Challenge for large-scale MARL arises when as population size rises, the joint state and action spaces increase exponentially. Prior works such as independent Q learning and centralized training with decentralized execution don’t have convergence guarantees. Mean-field control has guarantees, though it rests on the premise that the agents are homogeneous and exchangeable. Mondal et al., relaxes the homogeneous condition, into a heterogeneous population of agents that can be segregated into K homogenous classes, thus showing that heterogeneous MARL can be approximated by a K-class MFC problem, a generalization of MFC. {will add equation}

This is a technically strong and clearly laid out paper that offers a principled method for large-scale MARL. As a theory paper, the assumptions and proof are clear, and the talk is very well structured and easy to follow. My main critique and uncertainty about the work is related to comparison between MFC and non-MFC methods for multi-agent learning. It seems like in application, centralized training with decentralized execution, or communication is much more often applied. I’m curious about any empirical observation of convergence for the non-principled methods, and how MFC compares empirically; this is a theory only paper.

This paper offers an important relaxation for using MFC for large-scale multi-agent problems. I’m most interested in experimentally how K-class MFC performs compared to other methods. The authors note that we assume we can group agents into K homogeneous groups, and this assumption may hold in certain multi-agent problems, and not hold in others.

### The Role of Conventions in Adaptive Human-AI Interaction

The talk focuses on how conventions, a set of representations encoding shared understanding, is crucial for human collaboration, and proves to be useful for agent-agent, and agent-human interactions. This connects to the question of how best to cooperate between humans and AI.

Sadigh’s prior paper proposes a modular way of learning representation: one for partner-based representations, and one for task-specific representation. On three different tasks, agents adaptively build new conventions with each other over repeated interactions, and this framework leads to faster adaptations. Moreover, convention as low-dimensional representation can capture interaction and can change over time. Sadigh et. al also proposes restricting the convention to be low-dimensional. They observe that people’s actions can be large and non-stationary, but can be represented by low-dimensional latent strategies, aka conventions. The conventions can be used to predict the partner’s policies, and to react to the other agent.

This is a well-motivated and well-structured talk that gives a strong argument for conventions as a key part for adaptation in human-AI collaboration.

---

### Concluding remarks

Humans do not usually learn or work towards tasks and goals in isolation. Especially when assisting someone else, they interact with the person to learn their preferences. Similarly, for robots to be able to assist humans effectively, they need to be able to make decisions that are grounded on the user’s preference. Current robot systems are closed loop with regards to human preference and/or other agents observations and actions. The workshop talks we presented propose to address existing challenges to improve robot-robot and robot-human collaboration and coordination.

Additionally, the workshop papers proposed methods to make robot-robot and robot-human coordination possible in complex environments. These methods should enable robots to make better decisions in complex environments where coordination is required.
