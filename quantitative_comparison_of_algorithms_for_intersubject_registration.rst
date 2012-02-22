=========================================================================================================================
 Quantitative comparison of algorithms for inter-subject registration of 3D volumetric brain MRI scans - Ardekani et al. --- 10-13 February, 2012
=========================================================================================================================

1. Introduction
===============
- Inter-subject registration or spatial normalization of images acquired from DIFFERENT individuals.
- The main objective is to find a displacement field 'w' such that the warped image I_w(r) is as similar as possible to a template image I_t(r).
- Algorithms compared:
  * SPM99 [Friston et al. 1995]: w is modeled by a finite orthogonal series with trigonometric basis functions. The algorithm computes a displacement field by estimating the coefficients of the series using an iterative linearized least-squares approach.
  * AIR [Wood et al. 1998]: The displacement field is modeled as a polynomial. The algorithm estimates the polynomial coefficients using non-linear least-squares optimization.
  * ART (Automatic Registration Toolbox): non-parametric method. Has many more degrees of freedom than the other two methods. Uses cross-correlation as a measure of accuracy.


2. Materials and methods
========================

2.1. Implementation of ART inter-subject registration
-----------------------------------------------------
- Implementation details for the ART algorithm...


2.2. MRI scans
--------------
- 17 MRI scans


2.3. Preprocessing
------------------
- Deleted extra cranial regions using the brain extraction tool (BET) [Smith, 2002].


2.4. Registration
-----------------
- The MRI scan of the subject with the median intra-cranial volume was selected as the template image set.


2.5. Dispersion of homologous landmarks post-registration
---------------------------------------------------------
- 20 landmarks on each of the 16 original unregistered image volumes were manually located by an operator trained in neuroanatomy.
- After landmark selection, the 16 original volumes were warped using the deformation fields obtained from AIR, APM99 and ART.
- The new locations of the original 20 landmarks were located and the dispersion calculated.


3. Results and discussion
=========================
- The average registered volume corresponding to ART has higher resolution.
- ART has lower variance in general.

- Quantitative comparisons were conducted using the Standard Deviation maps.
- The images registered using ART are significantly less variable than those obtained using SPM99. However, AIR automatically scales the images in a way that tends to reduce the variance. Therefore we can not rely on this method for quantitative comparison of AIR.

- SPM99 was most noticeable the fastest [but they used archaic computers...]
- The AIR package has difficulty dealing with situations where the template and subject images have a significant initial misalignment.
- The removal of non-brain regions before starting the registration process was critical to the success of AIR.

- The greater ability of ART can be attributed to its greater degrees of freedom. The particular non-parametric implementation of ART allows incorporating a much larger number of degrees of freedom while keeping the computational cost manageable.
- In SPM99 and AIR, it is possible to use a larger number of basis functions that what is used in this paper, which should improve the results. But it might excessively increase the computation cost.


4. Conclusions
==============
- Homologous landmark sets manually identified on the images before registration were significantly less dispersed in ART after registration.
- The post-registration spatial dispersion of homologous landmarks measure becomes significantly more sensitive to the differences between algorithms when the landmarks are placed after image registration, as apposed to before.
- Non-parametric inter-subject registration algorithms with high degrees of freedom are able to reduce inter-subject anatomic variability to a greater extent as compared to lower dimensional parametric methods such as AIR and SPM99, while keeping the computational cost within acceptable limits.




