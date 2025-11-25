---
title: "Relative Flatness and Generalization"
collection: publications
category: conferences
permalink: '/publication/2021-flatness' 
excerpt: 'Why do over-parameterized machine learning models like deep neural networks generalize to unseen data? Under certain conditions, the relative flatness of the loss surface around the parameter values found during training is an explanation.'
date: 2021-12-06
venue: 'Proceedings of the 34th International Conference on Neural Information Processing Systems (NeurIPS)'
slidesurl: 
paperurl: 'https://papers.nips.cc/paper_files/paper/2021/file/995f5e03890b029865f402e83a81c29d-Paper.pdf'
citation: 'Petzka, Henning, Michael Kamp, Linara Adilova, Cristian Sminchisescu, and Mario Boley. "Relative flatness and generalization." <i>Advances in neural information processing systems</i>34 (NeurIPS 2021): 18420-18432.'
bibtex: |-
    @inproceedings{petzka2021flatness,
        title = {Relative Flatness and Generalization},
        author = {Petzka, Henning and Kamp, Michael and Adilova, Linara and Sminchisescu, Cristian and Boley, Mario},
        booktitle = {Advances in Neural Information Processing Systems (NeurIPS)},
        volume    = {34},
        year      = {2021},
        pages     = {18420--18432}
    }
---

**Abstract:** Flatness of the loss curve is conjectured to be connected to the generalization
ability of machine learning models, in particular neural networks. While it has
been empirically observed that flatness measures consistently correlate strongly
with generalization, it is still an open theoretical problem why and under which
circumstances flatness is connected to generalization, in particular in light of
reparameterizations that change certain flatness measures but leave generalization
unchanged. We investigate the connection between flatness and generalization
by relating it to the interpolation from representative data, deriving notions of
representativeness, and feature robustness. The notions allow us to rigorously
connect flatness and generalization and to identify conditions under which the
connection holds. Moreover, they give rise to a novel, but natural relative flatness
measure that correlates strongly with generalization, simplifies to ridge regression
for ordinary least squares, and solves the reparameterization issue.