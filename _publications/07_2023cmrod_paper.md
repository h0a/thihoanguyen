---
title: "7, Nonlinear dynamic analysis of shear- and torsion-free rods using isogeometric discretization and outlier removal"
collection: publications
permalink: /publications/07_2023cmrod_paper
excerpt: 'Computational Mechanics, 2024. Read more.'
---


<div class="small">
   Computational Mechanics, 2024.
</div>

<div class="small">
   Authors: <u><strong>T.-H. Nguyen</strong></u>, B.A. Roccia, R.R. Hiemstra, C.G. Gebhardt, D. Schillinger. 
</div><br/>
[[arXiv](https://arxiv.org/abs/2309.10652)], [[doi](https://doi.org/10.1007/s00466-024-02527-8)]

In this paper, we present a discrete formulation of nonlinear shear- and torsion-free rods based on [20] that uses isogeometric discretization and robust time integration. Omitting the director as an independent variable field, we reduce the number of degrees of freedom and obtain discrete solutions in multiple copies of the Euclidean space $\left(\mathbb{R}^3\right)$, which is larger than the corresponding multiple copies of the manifold $\left(\mathbb{R}^3 \times S^2\right)$ obtained with standard Hermite finite elements. For implicit time integration, we choose a hybrid form of the mid-point rule and the trapezoidal rule that  preserves the linear angular momentum exactly and approximates the energy accurately. In addition, we apply a recently introduced approach for outlier removal [26] that reduces high-frequency content in the response without affecting the accuracy, ensuring robustness of our nonlinear discrete formulation. We illustrate the efficiency of our nonlinear discrete formulation for static and transient rods under different loading conditions, demonstrating good accuracy in space, time and the frequency domain. Our numerical example coincides with a relevant application case, the simulation of mooring lines.