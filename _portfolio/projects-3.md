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

$$  J^{LMPC, j}_{t \rightarrow t + N}(x_t^j) = \underset{\mathbf{U}^j_t}{\text{minimize}} \sum_{k = t}^{t + N -1} h(x_{k | t}, u_{k | t}) + Q^{j-1}(x_{t + N | t}) \\ 
\text{subject to} \ \ \ \ x_{k+1 | t} = f(x_{k|t}, u_{k | t}) \ \ \ \  \forall k \in [t , \dots, t + N - 1 ] \\
x_{k | t} \in \mathcal{X}_k  \ \ \ \ \forall k \in [t , \dots, t + N] \\
u_{k | t} \in \mathcal{U}_k \ \ \ \  \forall k \in [t , \dots, t + N] \\
x_{t+N | t} \in \mathcal{SS}^{j-1} \\
x_{k | t } = x[t], \ \ \ \ k = t 
$$

Here h(x,u) is the stage cost at each timestep, x[k+1] = f(x[k], u[k]) is the discrete time difference equation governing the system, X<sub>k</sub> is the feasible state space at time step k, and U<sub>k</sub> is the feasible input space. To make this an LMPC approach, the terminal state along the prediction horizon is constrained to lie in the Safe Set (SS), defined as the union of all states measured during previous trials where the control task was executed successfully. The Q function is defined as the minimum cost-to-go for a point in the Safe Set over all iterations where that datapoint was recorded: 

$$ \mathcal{SS}^{j} = \bigcup_{i = 0}^{j} \bigcup_{k = 0}^{T_i} x_k^i $$ 

Where j is the number of completed trials and T<sub>i</sub> is the Time at which trial i was completed. Therefore: 

$$ Q: \mathcal{SS} \subseteq \mathbb{R}^n \mapsto \mathbb{R} $$

This approach, documented in literature [^fn1], allows us to approximate the value function based on previous data. As more data is collected, the estimate of the cost-to-go improves and the agent converges to globally optimal behavior over the entire control task. We can extend this approach to achieve the desired performance in the presence of multiple adversarial agents. 

First off, we can collect similar or reduced state data from the other agents. Since all the vehicles are racing on the same track, it is a fair assumption that the position of the other vehicles on the track can be observed by the other cars. Based on the data of the other agent's behavior, predictions can be formed of what the other agent is most likely to do next. Other than game-theoretic approaches to a competitive problem, such predictions are agnostic to what the other agent's strategy is. They can be generated simply, such as inference based on the convex hull of the recorded trajectories (e.g. the other agent is most likely to act similarly to its past behavior), or via more complicated statistical approaches. 

Once a prediction has been formed, two adjustements to the LMPC problem statement need to be made to prevent collisions. Firstly, the Safe-Set needs to be adjusted such that no points in the Safe-Set overlap with the predicted terminal state of the other agent(s), since the terminal constraint in the LMPC problem requires the terminal state along the prediction horizon to lie inside the Safe-Set:

$$ \mathcal{SS_{adj}} = \mathcal{SS} \setminus \mathcal{A_j^{t+N}} $$

Here $$ \mathcal{A_j^{k}} = \{x \in \mathbb{R}^n | A^k_j x \leq b\} $$ is the predicted polyhedron defining the state space occupied by the j'th agent on the k'th timestep. Theoretically it could be possible for the control problem to become infeasible if too many points in the Safe-Set have to be removed, as dynamic or input constraints might make it impossible to reach the remaining points in the Safe-Set. However, in practice this is not much of in issue, primarily because a single lap around a track already provides enough data for at least one feasible point to remain in the adjusted Safe-Set. Moreover, predictive control problems are almost always (also in this case) implemented using slack variables on most constraints. This is because in the case that a collision cannot be avoided, one would still want the cars to crash as softly as possible. This opposes a hard implementation where the problem becomes infeasible, no new control action is computed, and the cars crash at full speed. 

The second adjustement to the problem that needs to be made is that the feasible state space at each timestep in the control problem needs to be asjusted to  

![Green vehicle executing the LMPC strategy avoids and overtakes the blue car](/images/overtake.jpg)  

[^fn1]: U. Rosolia and F. Borrelli: "Learning Model Predictive Control for Iterative Task. A Data-Driven Control Framework", in IEEE Transaction on Automatic Control (2018).

[^fn2]: Brunner M., Rosolia U., Gonzales J. and Borrelli F. (2017), "Repetitive Learning Model Predictive Control: An Autonomous Racing Example", In Proceedings of 56th Conference on Decision and Control, 12, 2017.
