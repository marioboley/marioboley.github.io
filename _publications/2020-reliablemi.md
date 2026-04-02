---
title: "Discovering dependencies with reliable mutual information"
collection: publications
category: manuscripts
permalink: '/publication/2020-reliablemi'
excerpt: 'Discover statistical dependencies between variables using an estimate of mutual information that accounts for finite-sample bias.'
date: 2020-07-24
venue: 'Knowledge and Information Systems'
slidesurl: 
paperurl: 'https://link.springer.com/article/10.1007/s10115-020-01494-9'
citation: 'P Mandros, M Boley, J Vreeken. (2020). &quot;Discovering dependencies with reliable mutual information.&quot; <i>Knowledge and Information Systems</i>. 62, 4223–4253.'
bibtex: |-
    @article{mandrosDiscoveringDependenciesReliable2020,
        title = {Discovering Dependencies with Reliable Mutual Information},
        author = {Mandros, Panagiotis and Boley, Mario and Vreeken, Jilles},
        year = 2020,
        month = jul,
        journal = {Knowledge and Information Systems},
        volume = {62},
        pages = {4223--4253},
        publisher = {Springer},
        issn = {0219-3116},
        doi = {10.1007/s10115-020-01494-9},
    }
---

**Abstract:** We consider the task of discovering functional dependencies in data for target attributes of interest. To solve it, we have to answer two questions: How do we quantify the dependency in a model-agnostic and interpretable way as well as reliably against sample size and dimensionality biases? How can we efficiently discover the exact or $\alpha$-approximate top-k dependencies? We address the first question by adopting information-theoretic notions. Specifically, we consider the mutual information score, for which we propose a reliable estimator that enables robust optimization in high-dimensional data. To address the second question, we then systematically explore the algorithmic implications of using this measure for optimization. We show the problem is NP-hard and justify worst-case exponential-time as well as heuristic search methods. We propose two bounding functions for the estimator, which we use as pruning criteria in branch-and-bound search to efficiently mine dependencies with approximation guarantees. Empirical evaluation shows that the derived estimator has desirable statistical properties, the bounding functions lead to effective exact and greedy search algorithms, and when combined, qualitative experiments show the framework indeed discovers highly informative dependencies.
