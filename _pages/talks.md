---
layout: archive
title: "Talks, presentations, and posters"
permalink: /talks/
author_profile: true
redirect_from: 
  - /md/
  - /talks.html
---

<!-- If you have talk posts in /_talks/, used layout class defined in /_includes/archive-single.html

{% for post in site.talks reversed %}
  {% include archive-single.html %}
{% endfor %} -->
<div class="small">
I also present my works at international conferences. Below is an overview of my talks and posters.  
</div> 


<h4>
<u>Presentations</u>
</h4>

<!-- APCOM-ACCM 2025 -->
<h6>Data-driven geometrically exact beam elements for structural analysis of mooring lines in offshore wind applications: Solving strategies – Accuracy and computational cost</h6>
<div class="small">
   The 9th Asian Pacific Congress on Computational Mechanics/The 7th Australasian Conference on Computational Mechanics (APCOM-ACCM), December 7-10, 2025, Brisbane, Australia.
</div> 

<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  Recently, data-driven computational mechanics has emerged as a promising and powerful approach that enables the direct employment of raw experimental data in structural analysis, for instance data on the constitutive relations. In this work, we extend our solving strategy, introduced in our previous work [1], for structural analysis using data-driven geometrically exact beam elements (see e.g. [2]). In particular, we combine a greedy optimization algorithm and the alternating direction method (ADM), i.e. the standard direct data-driven solver for nonlinear systems [3], and refer to this as the GO-ADM solving strategy [1,4]. We apply our GO-ADM solving strategy to the structural analysis of two- and three-dimensional structures, considering different constitutive datasets. We show that for these examples, our solving strategy generally achieves a better approximation of the globally optimal solution. This, however, comes at the expense of higher computational cost which is scaled by the number of “greedy” searches.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] V. H. Gjerde, A data-driven model for the analysis of geometrically nonlinear one-dimensional structures, Master’s thesis, University of Bergen (2025).<br/>

  [2] C. G. Gebhardt, D. Schillinger , M. C. Steinbach, R. Rolfes, A framework for Data-Driven Structural Analysis in general elasticity based on nonlinear optimization: The static case, CMAME 365 (2020) 112993.<br/>

  [3] T. Kirchdoerfer, M. Ortiz, Data-driven computational mechanics, CMAME 304 (2016) 81–101.<br/>

  [4] T.-H. Nguyen, V. H. Gjerde, B. A. Roccia, C. G. Gebhardt, Solving strategies for data-driven one-dimensional elasticity exhibiting nonlinear strains, arXiv: 2512.19912, 2025.

</details>
</div> 



<!-- ECCOMAS 2024 -->
<h6>An isogeometric nonlinear formulation for shear- and torsion-free rods using outlier removal and robust time integration</h6>
<div class="small">
   9th European Congress on Computational Methods in Applied Sciences and Engineering (ECCOMAS), June 3-7, 2024, Lisbon, Portugal.
</div> 

<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  In this work, we present an isogeometric nonlinear formulation based on the nonlinear shear- and torsion-free rods introduced by Gebhardt and Romero in [1] employing robust time integration and outlier removal approach. Omitting the director as an independent variable field, we reduce the number of degrees of freedom and obtain discrete solutions in multiple copies of the Euclidean space (R3), which is larger than the corresponding multiple copies of the manifold (R3 ×S2) obtained with standard Hermite finite elements. For implicit time integration, we choose a hybrid form of the mid-point rule and the trapezoidal rule that preserves the linear and angular momentum exactly and approximates the energy accurately. In addition, we apply a recently introduced approach for outlier removal by Hiemstra et al. [2] that reduces high-frequency content in the response without affecting the accuracy, ensuring robustness of our nonlinear discrete formulation. We illustrate the efficiency of our nonlinear discrete formulation for static and transient rods under different loading conditions, demonstrating good accuracy in space, time and the frequency domain. Our numerical example coincides with a relevant application case, the simulation of mooring lines.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] C. G. Gebhardt, I. Romero, On a nonlinear rod exhibiting only axial and bending deformations: mathematical modeling and numerical implementation. Acta Mechanica 232 (10) (2021) 3825–3847.<br/>
  
  [2] R. R. Hiemstra, T. J. R. Hughes, A. Reali, D. Schillinger, Removal of spurious outlier frequencies and modes from isogeometric discretizations of second- and fourth-order problems in one, two, and three dimensions. Computer Methods in Applied Mechanics and Engineering 387 (2021) 114115.

</details>
</div> 


<!-- IGA 2023 -->
<h6>An isogeometric Petrov-Galerkin formulation with approximate dual spline functions and mass lumping for higher-order accurate explicit 
dynamics of shells</h6>
<div class="small">
   11th International Conference on Isogeometric Analysis (IGA2023), June 18-21, Lyon, France.
</div> 


<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  In structural dynamics, particularly in crash and metal forming simulations, explicit methods have broad applications. Commercial codes of these computations, such as LS-DYNA, PAM-CRASH, and RADIOSS, rely on three key ingredients to achieve highly efﬁcient transient calculations: (1) low memory requirements; (2) an efﬁcient solve; and (3) relatively large critical time-step values. These ingredients are present in contemporary linear finite element codes based on mass lumping [1]. In this talk, we present a higher order accurate mass lumping technique within the context of isogeometric analysis. Our method uses compactly supported test functions that are ''approximate'' dual functionals of B-splines [2]. Because these dual functionals are linear combinations of the same B-splines, the spanning test space remains unaltered. Lumping the Galerkin mass matrix yields an identity matrix, eliminating the need for matrix inversion. We discuss two approaches that weakly and strongly enforce the essential boundary conditions without losing variational consistency and negatively affecting the accuracy. We demonstrate via numerical examples of thin-walled structures in explicit dynamics settings that using our approach retains high order accuracy. This is further supported by good spectrum properties and high efficiency of the explicit scheme.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] Hughes, T. J. R., The Finite Element Method: Linear Static and Dynamic Finite Element Analysis. Dover Publications, 2003.<br/>

  [2] Chui, C. K., He, W., and Stöckler, J., Nonstationary tight wavelet frames, I: Bounded intervals. Applied and Computational Harmonic Analysis (2004) 17 (2): 141–197.
</details>
</div> 




<!-- GAMM 2023 -->
<h6>Nonlinear dynamic analysis of rods precluding shear and torsion with isogeometric discretizations</h6>
<div class="small">
   93rd Annual Meeting of the International Association of Applied Mathematics and Mechanics (GAMM), May 30-June 2, 2023, Dresden, Germany.
</div> 

<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  In this work, we investigate, in the context of isogeometric analysis (IGA) [1], the recently developed formulation of nonlinear rods exhibiting only axial and bending deformations introduced in [2]. We utilize the higher-order continuity of smooth spline functions which naturally fulfill the C1 continuity required by the nonlinear formulation of [2]. The number of discrete variable fields, compared to the standard spatial discretization scheme using cubic C1 Hermite polynomials in the same reference, thus can be reduced. The resulting discrete solution belongs to R3 that is larger than the manifold (R3 × S2) of the standard scheme, however, might not preserve the same manifold structure. Inspired by [2], we employ the implicit time integration scheme that is a hybrid combination of the midpoint and trapezoidal rules. It approximately preserves the energy and exactly preserves the linear angular momentum, and thus is efficient and robust for our investigation. We demonstrate, via two- and three-dimensional numerical examples of rods, that isogeometric discretizations of the same polynomial degree and smoothness are less robust than the standard spatial discretization scheme using Hermite polynomials. Their robustness can be improved by using, for instance, the strong approach of outlier removal, or by reducing the time step. We illustrate, via an example of a swinging rod under non- and conservative, and pulsating forces, that the improved discretization scheme thus can be employed for highly nonlinear cases. We also show that the configuration-dependent mass matrix of the studied formulation behaves irregularly and thus cannot be simplified to a conﬁguration-independent one.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] T. J. R. Hughes, J. A. Cottrell, Y. Bazilevs, Isogeometric analysis: CAD, finite elements, NURBS, exact geometry and mesh refinement. Computer Methods in Applied Mechanics and Engineering 194 (39) (2005) 4135–4195.<br/>

  [2] C. G. Gebhardt, I. Romero, On a nonlinear rod exhibiting only axial and bending deformations: mathematical modeling and numerical implementation. Acta Mechanica 232 (10) (2021) 3825–3847.
</details>
</div> 



<!-- GACM 2022 + IGA 2022 -->
<h6>Spectral analysis for assessing membrane locking and unlocking in isogeometric finite element formulations of the curved Euler-Bernoulli beam</h6>
<div class="small">
   10th International Conference on Isogeometric Analysis (IGA2022), November 6-9, Banff, Canada.
   <!-- 9th GACM Colloquium on Computational Mechanics, September 21-23, 2022, Essen, Germany. -->
</div> 


<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  In finite element discretizations of curved beam and shell models, membrane locking refers to the phenomenon of artificial bending stiffness due to the coupling of the bending response and membrane response caused by the local curvature [1,2], which negatively affects accuracy and convergence. The development of locking-preventing discretization technology has a history of more than 40 years, first within classical finite elements and then in isogeometric analysis. Given the multitude of formulations addressing membrane locking, the question arises how to best compare and assess their accuracy and effectivity. In this talk, we present spectral analysis as a tool to assess locking phenomena in finite element formulations and the effectiveness of locking-free formulations [3]. Via comparison the difference between eigenvalue and mode error curves computed on coarse meshes with "asymptotic" error curves computed on "overkill" meshes, locking can be identified and "measured". To demonstrate the intimate relation between membrane locking and spectral accuracy, we focus on the example of a circular ring discretized with isogeometric curved Euler-Bernoulli beam elements. We show that the transverse-displacement-dominating modes are locking-prone, while the circumferential-displacement-dominating modes are naturally locking-free. We use eigenvalue and mode errors to assess five isogeometric finite element formulations in terms of their locking-related efficiency: the displacement-based formulation with full and reduced integration and three locking-free formulations based on the B-bar, discrete strain gap, and Hellinger-Reissner methods. Our study shows that spectral analysis uncovers locking-related effects across the spectrum of eigenvalues and eigenmodes, rigorously characterizing membrane locking in the displacement-based formulation and unlocking in the locking-free formulations.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] Stolarski, H. and Belytschko, T., Membrane locking and reduced integration for curved elements. Journal of Applied Mechanics, Transactions ASME (1982) 49 (1): 172–176.<br/>

  [2] Bischoff, M., Ramm, E. and Irslinger, J., Models and finite elements for thin-walled structures. Encyclopedia of Computational Mechanics Second Edition (2018) 1–86.<br/>

  [3] Nguyen, T.-H., Hiemstra, R. R. and Schillinger, D., Leveraging spectral analysis to elucidate membrane locking and unlocking in isogeometric finite element formulations of the curved Euler-Bernoulli beam. Computer Methods in Applied Mechanics and Engineering (2021) 388: 114240.
</details>
</div> 






<!-- WCCM 2022 -->
<h6>A variational approach based on perturbed eigenvalue analysis for improving spectral properties of isogeometric multipatch discretizations</h6>
<div class="small">
   15th World Congress on Computational Mechanics (WCCM XV - APCOM-VIII), July 31-August 5, 2022, Yokohama, Japan.
</div> 

<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  A key advantage of isogeometric discretizations is their accurate and well-behaved eigenfrequencies and eigenmodes. For degree two and higher, however, the so-called optical branches formed by spurious outlier frequencies and modes may appear due to boundaries or reduced continuity at patch interfaces [1, 2]. The outlier frequencies are signiﬁcantly overestimated, which unnecessarily reduce the stable critical time-step size in explicit dynamics calculations. Moreover, the outlier modes behave in a spurious manner and may have a negative impact on the solution accuracy and robustness, particularly in hyperbolic problems [3]. In this talk, we present (a) a variational approach based on perturbed eigenvalue analysis to improve the spectral properties of isogeometric multipatch discretizations; and (b) a scheme for estimating optimal scaling parameters of the added perturbation term such that the outlier frequencies are effectively reduced and the accuracy in the remainder of the spectrum and modes is not negatively affected; and (c) how to cast this scheme into a pragmatic iterative procedure that can be readily implemented in any isogeometric analysis framework. We verify numerically via spectral analysis of second-and fourth-order problems that the proposed approach improves spectral properties of isogeometric multipatch discretizations in the one- and multidimensional setting. For exemplary membrane and plate structures, we confirm that our approach maintains spatial accuracy and enables a larger critical time-step size. We also demonstrate that it does not depend on the polynomial degree of spline basis functions.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] Cottrell, J. A., Reali, A., Bazilevs, Y. and Hughes, T. J. R., Isogeometric analysis of structural
vibrations. Computer Methods in Applied Mechanics and Engineering (2006) 195 (41): 5257–5296.<br/>

  [2] Puzyrev V., Deng, Q. and Calo, V. Spectral approximation properties of isogeometric analysis with variable continuity. Computer Methods in Applied Mechanics and Engineering (2018) 334: 22–39.<br/>

  [3] Hughes, T. J. R., Evans, J. A. and Reali, A., Finite element and NURBS approximations of eigenvalue, boundary-value, and initial-value problems. Computer Methods in Applied Mechanics and Engineering (2014) 272: 290–320.
</details>
</div> 




<!-- GAMM 2021 -->
<h6>Variationally consistent framework for higher-order imperfect interface models of thin layers</h6>
<div class="small">
   International Association of Applied Mathematics and Mechanics (GAMM2020@21), March 15-19, 2021, Kassel, Germany.
</div> 

<div class="small">
<details>
  <summary>Read more.</summary>
  <br/>
  In composite structures, thin films and coatings are typically used to prevent damage or to increase structure durability. Direct numerical simulation of their mechanical response requires extreme fine mesh sizes and is thus computationally expensive. Therefore, a finite-thickness interphase model is often approximated by an interface model of zero thickness, based on the reformulation of its mechanical effects as jump conditions in the relevant fields [1]. In this talk, we present a) an extension of an existing first-order into a new higher-order accurate interface model which involves higher-order differential operators in the jump formulation; and b) a variationally consistent framework combining higher-order smooth spline basis functions and cut finite element methods for its numerical approximation [2]. We demonstrate robustness and accuracy of this framework via a two-dimensional Laplace problem with a thin circular
  interphase.<br/>
  <br/>
  <b>References</b>:<br/>

  [1] Y. Benveniste and T. Miloh (2001): Imperfect soft and stiff interfaces in two-dimensional elasticity. Mechanics of Materials, 33:309-323, 2001.<br/>

  [2] Z. Han, S.K.F. Stoter, C.T. Wu, C. Cheng, A. Mantzaflaris, S. Mogilevskaya, D. Schillinger (2019): Consistent discretization of higher-order interface models for thin layers and elastic material surfaces, enabled by isogeometric cut-cell methods. Computer Methods in Applied Mechanics and Engineering, 350:245-267, 2019.
</details>
</div> 




<h4>
<u>Posters</u>
</h4>

<!-- DeepWind 2026 -->
<h6>A novel disruptive data-driven approach for mooring line modeling and design in floating offshore wind</h6>
<div class="small">
   EERA DEEPWIND 2026, January 14-16, 2026, Trondheim, Norway.
</div>

<embed src="https://https://github.com/h0a/thihoanguyen/blob/master/assets/posters/DeepWind2025_Poster.pdf" type="application/pdf" />


<!-- DeepWind 2025 -->
<h6>FEM formulations for nonlinear dynamics of shear- and torsion-free rods in mooring line applications</h6>
<div class="small">
   [Poster (PDF)](/assets/posters/DeepWind2025_Poster.pdf). EERA DEEPWIND 2025, January 15-17, 2025, Trondheim, Norway.
</div> 