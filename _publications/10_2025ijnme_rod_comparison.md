---
title: "10, A study on nodal and isogeometric formulations for nonlinear dynamics of shear- and torsion-free rods"
collection: publications
permalink: /publications/10_2025ijnme_rod_comparison
excerpt: ' International Journal for Numerical Methods in Engineering (under review), 2025. Read more.'
---


<div class="small">
    International Journal for Numerical Methods in Engineering (under review), 2025.
</div>

<div class="small">
   Authors: <u><strong>T.-H. Nguyen</strong></u>, B.A. Roccia, D. Schillinger, C.C. Gebhardt. 
</div><br/>
[[arXiv](https://doi.org/10.48550/arXiv.2412.20132)]

In this work, we compare the nodal and isogeometric spatial discretization schemes for the nonlinear formulation of shear- and torsion-free rods introduced in [1]. We investigate the resulting discrete solution space, the accuracy, and the computational cost of these spatial discretization schemes. To fulfill the required C1 continuity of the rod formulation, the nodal scheme discretizes the rod in terms of its nodal positions and directors using cubic Hermite splines. Isogeometric discretizations naturally fulfill this with smoothspline basis functions and discretize the rod only in terms of the positions of the control points [2], which leads to a discrete solution in multiple copies of the Euclidean space R3. They enable the employment of basis functions of one degree lower, i.e. quadratic C1 splines, and possibly reduce the number of degrees of freedom. When using the nodal scheme, since the defined director field is in the unit sphere S2, preserving this for the nodal director variable field requires an additional constraint of unit nodal directors. This leads to a discrete solution in multiple copies of the manifold R3xS2, however, results in zero nodal axial stress values. Allowing arbitrary length for the nodal directors, i.e. a nodal director field in R3 instead of S2 as within discrete rod elements, eliminates the constrained nodal axial stresses and leads to a discrete solution in multiple copies of R3. We discuss a strong and weak approach using the Lagrange multiplier method and penalty method, respectively, to enforce the unit nodal director constraint. We compare the resulting semi-discrete formulations and the computational cost of these discretization variants. We numerically demonstrate our findings via examples of a planar roll-up, a catenary, and a mooring line.  