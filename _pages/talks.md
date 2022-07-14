---
layout: archive
title: "Talks and presentations"
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
I also present my works at international conferences. Below is an overview of my talks. 
</div> 


<!-- WCCM 2022 -->
<h6>A Variational approach based on perturbed eigenvalue analysis for improving spectral properties of isogeometric multipatch discretizations</h6>
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
