---
title: "5, Variationally consistent mass scaling for explicit time-integration schemes of lower- and higher-order finite element methods"
collection: publications
permalink: /publications/2022cmame_mass_scaling_stein
excerpt: 'Computer Methods in Applied Mechanics and Engineering, 2022 (under review). Read more.'
---


<div class="small">
   Computer Methods in Applied Mechanics and Engineering, 2022 (under review).
</div>

<div class="small">
   Authors: Stein K. F. Stoter, <u><strong>T.-H. Nguyen</strong></u>, R.R. Hiemstra, D. Schillinger. 
</div><br/>
[[arXiv](https://arxiv.org/abs/2201.10475)] [[doi](https://doi.org/10.1016/j.cma.2022.115310)]

In this paper, we propose a variationally consistent technique for decreasing the maximum eigenfrequencies of structural dynamics related finite element formulations. Our approach is based on adding a symmetric positive-definite term to the consistent mass matrix that follows from the integral of the traction jump across element boundaries. The added term is weighted by a small factor, for which we derive a suitable, and simple, element-local parameter choice. We perform numerical experiments for the linear wave equation in one and two dimensions, for quadrilateral elements and triangular elements, and for up to fourth order polynomial basis functions. Despite the increase in critical time-step size, we do not observe adverse effects in terms of spatial accuracy and orders of convergence. To extend the method to non-linear problems, we introduce a linear approximation. Our three-dimensional experiments with tetrahedral and hexahedral elements show that a sizeable increase in critical time-step size can be achieved while only causing minor (even beneficial) influences on the dynamic response.
