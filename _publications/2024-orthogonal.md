---
title: "Orthogonal Gradient Boosting for Simpler Additive Rule Ensembles"
collection: publications
category: conferences
permalink: '/publication/2024-orthogonal' 
excerpt: 'Improving the accuracy / comprehensibility trade-off of rule ensembles and other additive models via proper adaption of boosting with weight correction.'
date: 2024-04-18
venue: 'International Conference on Artificial Intelligence and Statistics'
slidesurl: 
paperurl: 'https://proceedings.mlr.press/v238/yang24b/yang24b.pdf'
citation: 'F Yang, P Le Bodic, M Kamp, M Boley. (2024). &quot;Orthogonal Gradient Boosting for Simpler Additive Rule Ensembles.&quot; <i>AISTATS</i>.'
---

**Abstract:** Gradient boosting of prediction rules is an efficient approach to learn potentially interpretable yet accurate probabilistic models. However, actual interpretability requires to limit the number and size of the generated rules, and existing boosting variants are not designed for this purpose. Though corrective boosting refits all rule weights in each iteration to minimise prediction risk, the included rule conditions tend to be sub-optimal, because commonly used objective functions fail to anticipate this refitting. Here, we address this issue by a new objective function that measures the angle between the risk gradient vector and the projection of the condition output vector onto the orthogonal complement of the already selected conditions. This approach correctly approximates the ideal update of adding the risk gradient itself to the model and favours the inclusion of more general and thus shorter rules. As we demonstrate using a wide range of prediction tasks, this significantly improves the comprehensibility/accuracy trade-off of the fitted ensemble. Additionally, we show how objective values for related rule conditions can be computed incrementally to avoid any substantial computational overhead of the new method.