---
title: 'Gradient Boosting versus Mixed Integer Programming for Sparse Additive Modeling'
collection: publications
category: conferences
permalink: /publication/2025-gbvsmip
excerpt: 'How accurate is gradient boosting as an approximation algorithm for the best additive model with a limited number of terms? Theoretically, we show that the approximation gap can be as wide as $1/2$, but how representative is that for the practical performance? An empirical comparison with mixed integer programming paints a differentiated picture.'
date: 2025-11-25
venue: 'Joint European Conference on Machine Learning and Knowledge Discovery in Databases (ECMLPKDD 2025)'
paperurl: 'https://ecmlpkdd-storage.s3.eu-central-1.amazonaws.com/preprints/2025/research/preprint_ecml_pkdd_2025_research_522.pdf'
slidesurl: 
citation: 'Yang, Fan, Pierre Le Bodic, and Mario Boley. "Gradient Boosting Versus Mixed Integer Programming for Sparse Additive Modeling." In Joint European Conference on Machine Learning and Knowledge Discovery in Databases, pp. 453-470. Cham: Springer Nature Switzerland, 2025.'
---

**Abstract:** Gradient boosting is a widely used algorithm for fitting sparse additive models over flexible classes of basis functions. Despite its popularity, the performance of gradient boosting as an approximation algorithm to the empirical risk minimizing model with a specific number k of selected basis functions is poorly understood. We provide a theoretical lower bound of $1/2-1/(4k-2)$ on the worst-case approximation ratio for the risk reduction that gradient boosting achieves relative to the optimal model when both are limited to $k$ terms. This result reveals an inherent limitation in boosting’s ability to approximate the best possible sparse additive model, raising the question of how tight and representative this bound is in practice. To empirically answer this question, we employ mixed integer programming (MIP) to approximate the optimal additive models on 21 real datasets. The experimental results do not show larger gaps than the theoretical analysis, indicating that the theoretical lower bound is tight. Moreover, for twelve datasets, the approximation gaps are of the same order of magnitude as the theoretical lower bound, which shows the representativeness of the theoretical bound. To that end, the study also has the practical implication that the presented MIP approach frequently offers notable improvements over gradient boosting.