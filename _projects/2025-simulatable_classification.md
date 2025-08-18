---
title: "Human-Simulatable Additive Classification Models"
collections: projects
date: 2025-08-18
mathjax: true
---

Generalised additive classification models provide probabilistic predictions by adding the output of individual predictors to form a raw score $a=a_1+\dots+a_k$ and then by mapping this raw score to a probability $p(a)$. This process is considered human-interpretable because of the relative ease with which humans can carry out addition, and this is further enhanced by model fitting methods that restrict the individual predictor output to a small set of round values, i.e., $a_i \in \{-5, \dots, 5\}$. However, even when the resulting raw sum is round and easy to compute, the standard classification machinery based around the logistic transformation $p(a)=e^a/(1+e^a)$ renders it hard for humans to convert raw outputs to probabilities. For instance, a raw model output of $a=1$ corresponds to a probability $e/(1+e) \approx$ 0.7311 and $a=2$ corresponds to $e^2/(1+e^2)\approx 0.8808$.

In this project, we aim to develop truly human-interpretable additive classification models, centred around the idea to re-calibrate raw classification scores to be easily mappable to probabilities and to integrate these scores into an efficient and consistent model fitting approach that produces round predictor outputs. Specifically, we consider the generalised transformation $p_b(a)=b^a/(1+b^a)$ resulting e.g., in $p_3(1) = 3/4 = 0.75$ and $p_3(2) = 9/10=0.9$. This results in a number of research questions, including how the resulting classification loss function can be integrated into existing optimisation approaches for finding predictor weights and, more fundamentally, by how much the resulting re-calibrated score helps humans to simulate the model to reach better decisions under time pressure.
