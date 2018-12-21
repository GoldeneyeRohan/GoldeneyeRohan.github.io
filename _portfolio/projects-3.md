---
title: "Multi-Vehicle LMPC Racing"
excerpt: "Learning Model Predictive Control for Multi-Agent Autonomous Racing <br/><img src='/images/LMPC_fpic.jpg'>"
collection: portfolio
---

Recent work in the [Model Predictive Control Lab](mpc.berkeley.edu) at UC Berkeley has focused on data driven control strategies for iterative tasks. Such algorithms, referred to as Iterative Learning Controllers (ILC), are designed to learn from previous experience as a control task is executed over multiple iterations. An example of an ILC strategy, called Learning Model Predictive Control (LMPC) [^fn1] involves repetitively solving a more traditional Constrained Finite Time Optimal Control (CFTOC) problem over a finite (rolling) horizon at a fixed sampling time and applying the first control input to the system at each timestep. The difference with regular MPC strategies is that in LMPC the cost-to-go, often referred to as the value function, is approximated based on data from previous trials. An inductive proof can be done to show that the total cost over a task iteration must be non-increasing with an LMPC strategy, and therefore LMPC strategies converge to optimal solutions not just over the CFTOC horizon, but over the entire control task.[^fn1]

LMPC strategies have been shown to rapidly learn from previous experience and exhibit the provably safe behavior associated with Model Predictive Controllers. One notable example of LMPC controllers effectively learning to execute a complex task in an optimal fashion is in autonomous racing [^fn2]. In the MPC Lab, cars have been trained to race around racetracks near vehicle performance and handling limits with LMPC strategies. My research work focuses on extending these approaches to enable multiple vehicles to race each other in a safe fashion. The goal is for the cars:

* to still converge to globally optimal racing policies
* to learn the behavior of opponents and predict their strategy
* to avoid and overtake other vehicles strategically in a safe fashion

![racing pic](/images/LMPC_fpic.jpg)

To do this, a LMPC style CFTOC is formulated at each timestep: 

$$  J^{LMPC, j}_{t \rightarrow t + N}(x_t^j) = \underset{\mathbf{U}^j_t}{\text{minimize}} \sum_{k = t}^{t + N -1} h(x_{k | t}, u_{k | t}) + Q^{j-1}(x_{t + N | t}) $$

					![Green vehicle executing the LMPC strategy avoids and overtakes the blue car](/images/overtake.jpg)   

<!-- $$  
\underset{x}{\text{minimize}} f_0(x) \\
\text{subject to}
 f_i(x) \leq b_i, \; i = 1, \ldots, m. $$ -->

[^fn1]: U. Rosolia and F. Borrelli: "Learning Model Predictive Control for Iterative Task. A Data-Driven Control Framework", in IEEE Transaction on Automatic Control (2018).

[^fn2]: Brunner M., Rosolia U., Gonzales J. and Borrelli F. (2017), "Repetitive Learning Model Predictive Control: An Autonomous Racing Example", In Proceedings of 56th Conference on Decision and Control, 12, 2017.