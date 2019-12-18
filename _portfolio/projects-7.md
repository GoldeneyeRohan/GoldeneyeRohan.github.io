---
title: "Data-Poisoning of Linear Models"
excerpt: "Algorithms for constructing data-poisoning attacks on Linear Models using Semi-Definite Programming <br/><img src='/images/dpois_fpic.jpg'>"
collection: portfolio
---

Problem - Make train set hard to fit
If attacker goes second, we have the standard _adversarial robustness_ problem:

$$
    p^* = \min_{f\in\F} \max_{X\in\mathcal X}\; L(f, X, y)
$$
If the attacker goes first, we have the \textit{data poisoning} problem:
$$    d^* = \max_{X\in\mathcal X} \min_{f\in\F}\; L(f, X, y) $$
Weak duality holds: $$d^*\leq p^* $$. \\
$$p^*$$ often corresponds to a convex problem (if the $$L$$ is convex in $$X$$ and $$f$$), but $$d^*$$ does not.

