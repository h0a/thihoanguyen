---
title: "9, A locking-free isogeometric thin shell formulation based on higher order accurate local strain projection via approximate dual splines"
collection: publications
permalink: /publications/09_2024ijnme_strain_projection
excerpt: ' International Journal for Numerical Methods in Engineering (under review), 2024. Read more.'
---


<div class="small">
    International Journal for Numerical Methods in Engineering (under review), 2024.
</div>

<div class="small">
   Authors: <u><strong>T.-H. Nguyen</strong></u>, R.R. Hiemstra, D. Schillinger. 
</div><br/>
[[arXiv](https://doi.org/10.48550/arXiv.2406.16685)]

We present a novel isogeometric discretization approach for the Kirchhoff-Love shell formulation based on the Hellinger-Reissner variational principle. For mitigating membrane locking, we discretize the independent strains with spline basis functions that are one degree lower than those used for the displacements. To enable computationally efficient condensation of the independent strains, we first discretize the variations of the independent strains with approximate dual splines to obtain a projection matrix that is close to a diagonal matrix. We then diagonalize this strain projection matrix via row-sum lumping. The combination of approximate dual test functions with row-sum lumping enables the direct condensation of the independent strain fields at the quadrature point level, while maintaining higher-order accuracy at optimal rates of convergence. We illustrate the numerical properties and the performance of our approach through numerical benchmarks, including a curved Euler-Bernoulli beam and the examples of the shell obstacle course. 