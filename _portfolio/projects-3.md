---
title: "Multi-Vehicle LMPC Racing"
excerpt: "Learning Model Predictive Control for Multi-Agent Autonomous Racing <br/><img src='/images/LMPC_fpic.jpg'>"
collection: portfolio
---

Recent work in the [Model Predictive Control Lab](mpc.berkeley.edu) at UC Berkeley has focused on data driven control strategies for iterative tasks. Such algorithms, refered to as Iterative Learning Controllers (ILC), are designed to learn from previous experience as a control task is executed over multiple iterations. An example of an ILC strategy, called Learning Model Predictive Control (LMPC) [^fn1] involves solving a traditional Constrained Finite Time Optimal Control (CFTOC) problem over a finite (rolling) horizon at a fixed sampling time and applying the first control input to the system at each timestep. The difference with regular MPC strategies is that in LMPC the cost-to-go, often referred to as the value function, is approximated based on data from previous task iterations. An inductive proof can be done to show that the total cost over a task iteration must be non-increasing with an LMPC strategy, and therefore LMPC strategies converge to optimal solutions not just over the CFTOC horizon, but over the entire control task. 

![Green vehicle executing the LMPC strategy avoids and overtakes the blue car](/images/overtake.jpg)   

<!-- $$  
\underset{x}{\text{minimize}} f_0(x) \\
\text{subject to}
 f_i(x) \leq b_i, \; i = 1, \ldots, m. $$ -->

[^fn1]: U. Rosolia and F. Borrelli: "Learning Model Predictive Control for Iterative Task. A Data-Driven Control Framework", in IEEE Transaction on Automatic Control (2018).

[^fn2]: Brunner M., Rosolia U., Gonzales J. and Borrelli F. (2017), "Repetitive Learning Model Predictive Control: An Autonomous Racing Example", In Proceedings of 56th Conference on Decision and Control, 12, 2017.