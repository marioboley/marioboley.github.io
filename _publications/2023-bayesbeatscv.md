---
title: "Bayes beats cross validation: fast and accurate ridge regression via expectation maximization"
collection: publications
category: conferences
permalink: /publication/2023-bayesbeatscv
excerpt: 'Ridge regression is one of the most widely used methods to fit linear regression models. Surprisingly, the established method for estimating its optimal shrinkage hyper-parameter value can be rather inaccurate. We show how a Bayesian formulation can address this issue.'
excerpt_old: 'Presents a novel method for tuning the regularization hyper-parameter, λ, of a ridge regression that is faster to compute than leave-one-out cross-validation (LOOCV) while yielding estimates of the regression parameters of equal, or particularly in the setting of sparse covariates, superior quality to those obtained by minimising the LOOCV risk'
date: 2023-12-10
venue: 'Proceedings of the 36th International Conference on Neural Information Processing Systems (NeurIPS)'
paperurl: 'https://papers.neurips.cc/paper_files/paper/2023/file/3eec5006051d9544e717067de3220198-Paper-Conference.pdf'
slidesurl: 
citation: 'SY Tew, M Boley, DF Schmidt. (2023). &quot;Bayes beats cross validation: fast and accurate ridge regression via expectation maximization.&quot; <i>NeurIPS</i>. 36'
---

**Abstract:**
We present a novel method for tuning the regularization hyper-parameter, λ, of a ridge regression that is faster to compute than leave-one-out cross-validation (LOOCV) while yielding estimates of the regression parameters of equal, or particularly in the setting of sparse covariates, superior quality to those obtained by minimising the LOOCV risk. The LOOCV risk can suffer from multiple and bad local minima for finite n and thus requires the specification of a set of candidate λ, which can fail to provide good solutions. In contrast, we show that the proposed method is guaranteed to find a unique optimal solution for large enough n, under relatively mild conditions, without requiring the specification of any difficult to determine hyper-parameters. This is based on a Bayesian formulation of ridge regression that we prove to have a unimodal posterior for large enough n, allowing for both the optimal λ and the regression coefficients to be jointly learned within an iterative expectation maximization (EM) procedure. Importantly, we show that by utilizing an appropriate preprocessing step, a single iteration of the main EM loop can be implemented in O(min(n,p)) operations, for input data with n rows and p columns. In contrast, evaluating a single value of λ using fast LOOCV costs $O(n\min(n,p))$ operations when using the same preprocessing. This advantage amounts to an asymptotic improvement of a factor of $l$ for $l$ candidate values for λ (in the regime $q, p \in O(n\sqrt{q})$ where $q$ is the number of regression targets).
