---
title: "6, Towards higher-order accurate mass lumping in explicit isogeometric analysis for structural dynamics"
collection: publications
permalink: /publications/2023cmame_mass_lumping
excerpt: 'Submitted to Computer Methods in Applied Mechanics and Engineering, 2023 (under review). Read more.'
---


<div class="small">
   Computer Methods in Applied Mechanics and Engineering, 2023.
</div>

<div class="small">
   Authors: <u><strong>T.-H. Nguyen</strong></u>, R.R. Hiemstra, S. Eisenträger, D. Schillinger. 
</div><br/>
[[arXiv](https://arxiv.org/abs/2305.12916)] 

We present a mass lumping approach based on an isogeometric Petrov-Galerkin method that preserves higher-order spatial accuracy in explicit dynamics calculations irrespective of the polynomial degree of the spline approximation. To discretize the test function space, our method uses an approximate dual basis, whose functions are smooth, have local support and satisfy approximate bi-orthogonality with respect to a trial space of B-splines. The resulting mass matrix is ''close'' to the identity matrix. Specifically, a lumped version of this mass matrix preserves all relevant polynomials when utilized in a Galerkin projection. Consequently, the mass matrix can be lumped (via row-sum lumping) without compromising spatial accuracy in explicit dynamics calculations. We address the imposition of Dirichlet boundary conditions and the preservation of approximate bi-orthogonality under geometric mappings. In addition, we establish a link between the exact dual and approximate dual basis functions via an iterative algorithm that improves the approximate dual basis towards exact bi-orthogonality. We demonstrate the performance of our higher-order accurate mass lumping approach via convergence studies and spectral analyses of discretized beam, plate and shell models. 

