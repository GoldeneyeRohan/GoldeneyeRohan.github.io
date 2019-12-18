---
title: "Data-Poisoning of Linear Models"
excerpt: "Algorithms for constructing data-poisoning attacks on Linear Models using Semi-Definite Programming <br/><img src='/images/dpois_fpic.jpg'>"
collection: portfolio
---
Data-Poisoning is a general term to refer to training-adversarial attacks, where an adversary has the ability to alter some subset of the training-data before a model is learned. Through this perturbation, the adversary coerces the learner to learn a corrupted model according to some goal for the adversary. These forms of attacks have been understudied when compared to the traditional robustness problem, even though they are have the potential to severely impact the performance of learned models. In application, large datasets are generated from external sources by scraping the internet, and an adversary could easily insert a small number of corrupted samples into training data to alter a learned model. Adhyyan Narang, Anand Siththaranjan, Forest Yang, and I studied the data-poisoning problem on simple linear models such as ridge and logistic regression as a course project for EECS227B under Prof. Laurent El Ghaoui and Prof. Somayeh Sojoudi. We proposed algorithms to create near-optimal (and in some cases optimal) data-poisoning attacks realiant on semi-definite programming, and are planning to release a preprint in December 2019. 

Intuitively, we can think of an adversarial attack as a game between the adversary (who can modify the data), and the learner (who needs to pick the model). If the learner has to pick first, we have the standard _adversarial robustness_ problem:

$$
    p^* = \min_{f\in\mathcal{F}} \max_{X\in\mathcal X}\; L(f, X, y)
$$
If the attacker goes first, we have the _data-poisoning}_ problem:

$$    d^* = \max_{X\in\mathcal X} \min_{f\in\mathcal{F}}\; L(f, X, y) $$

Weak duality holds: $$d^*\leq p^* $$.
However, $$p^*$$ often corresponds to a convex problem (if the $$L$$ is convex in $$X$$ and $$f$$), but $$d^*$$ does not. This motivates the use of convex relaxation techniques to generate data-poisoning attacks.


 
