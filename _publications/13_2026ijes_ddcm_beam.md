---
title: "13, A data-driven solving strategy based on a greedy optimization algorithm for the analysis of nonlinear beam structures"
collection: publications
permalink: /publications/13_2026ijes_ddcm_beam
excerpt: '(Under review) International Journal of Engineering Science, 2026. Read more.'
---


<div class="small">
   (Under review) International Journal of Engineering Science, 2026.
</div>

<div class="small">
   Authors: <u><strong>T.-H. Nguyen</strong></u>, B. A. Roccia, C. G. Gebhardt. 
</div><br/>
[[arXiv](https://arxiv.org/abs/2607.10401)]

In the last decade, data-driven computational mechanics (DDCM) has emerged as a novel paradigm in computational mechanics, enabling the direct use of constitutive data - such as stress-strain pairs obtained from experiments, without relying on ad-hoc material models and thereby avoiding information loss. In this work, we extend our data-driven solving strategy GO-ADM, which combines a greedy optimization algorithm with the alternating direction method (ADM), to the structural analysis of geometrically exact beams formulated using director-based kinematics. We discuss a data initialization strategy for nonlinear systems based on a conventional finite element analysis of the same structure using a prescribed constitutive model. The resulting discrete stress and strain fields, possibly obtained under multiple loading scenarios, may also be employed as artificial datasets for the subsequent data-driven computations. Furthermore, we investigate the thermomechanical consistency of both the dataset and the discrete solution, and propose a weak enforcement of this consistency in the latter via a penalty approach. Numerical examples involving single- and multi-member structures demonstrate that the proposed penalty term leads to thermomechanically consistent discrete stress and strain fields. Moreover, for the studied examples, the solving strategy GO-ADM yields a generally improved approximation of the globally optimal solution compared to the standard ADM-based direct solver.
