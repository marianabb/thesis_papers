===========================================================================
 Image registration methods: a survey - Barbara Zitova, Jan Flusser [2003] --- February 6&7, 2012
===========================================================================
-> The paper does not cover 3D registration methods.


1. Introduction
===============
- Registration: Process of overlaying two or more images of the same scene taken at different times, from different viewpoints, and/or by different sensors.
- Change detection
- Base image is the reference or source, the second image is referred to as the target or sensed.


2. Methodology
==============
Four steps for registration methods:
  1. Feature detection: finding Control Points
  2. Feature matching: correspondence between features in both images
  3. Transform model estimation: the mapping functions to take one image to the other
  4. Image resampling and transformation: Transform the image using the mapping functions

- Some typical problems


3. Feature detection
====================
- Can also be done by an expert, but in this case we want it automatically.

3.1. Area-based methods
-----------------------
- No Feature detection, directly feature matching.
- They work directly with image intensity values, no structural analysis -> sensitive to illumination changes or different sensor types. Ex: Cross-correlation.
- Medical images are usually not so rich in easy detectable features and thus area-based methods are usually employed. The features can be selected by an expert or by introducing marks when taking the image.
- Only shift and small rotation between the images are allowed.

3.2. Feature-based methods
--------------------------
- Features are expected to be stable in time to stay at fixed positions.
- The number of common elements should be sufficiently high, regardless of the change of image geometry, noise or changes in the scanned scene.
- Do not work directly with image intensity values -> suitable for illumination changes and images of different nature.
  
  - Region Features: Detected using segmentation.
  - Line Features
  - Point Features: Line intersections, centroids of regions, high variance points, corners, ... [lots about this]


4. Feature matching
===================

4.1. Area-based methods
-----------------------
- Limitation: they use a window of predefined size (or the whole image) to compare.
  - The shape of the window limits the type of differences between images that can be detected.
  - If the window has nothing remarkable it might match with any other unremarkable part of the image. We want distinctive parts of the image in the windows.

4.1.1 Correlation-like methods:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  - Usually use Normalized CC [Measure of similarity]: Computed for window pairs and we search for the maximum -> maximums are corresponding pairs.
  - CC can only align translated imaged, but there are versions for more deformed images -> computational cost grows very fast.

  - Sequential Similarity detection Algorithm (SSDA): Accumulates the sum of absolute differences of the image intensity values and applies threshold (threshold is the max).
  - It's computationally simpler.

4.1.2 Fourier methods
~~~~~~~~~~~~~~~~~~~~~
  - Faster than correlation methods!
  - Robust against noise and illumination disturbances. 

4.1.3 Mutual information methods
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  - Leading technique in multimodal registration.
  - Measure of statistical dependency between two datasets. We want to maximize MI.

4.1.4 Optimization methods
~~~~~~~~~~~~~~~~~~~~~~~~~~
  - The problem can be viewed as an optimization problem where we try to maximize the similarity measure or minimize the dissimilarity measure. The number of dimensions corresponds to the degrees of freedom of the expected geometrical transformation.
  - Only way of finding a global extreme -> exhaustive search -> we need advanced optimization algorithms.



4.2 Feature-based methods
-------------------------

4.2.1 Methods using spatial relations
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Find pairwise correspondence between CPs. Use the information about the distance between the CPs and their spatial distribution.
- Clustering techniques, Chamfer matching, Iterative Closest Point (ICP) [for 3D shapes]

4.2.2 Methods using invariant descriptors
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Estimates correspondence of features using their description, which should be invariant to the expected deformation.
- The description should be: [HARD, we must agree on a trade-off]
  + Invariant
  + Unique
  + Stable
  + Independent
- The matching pairs are defined with the minimum distance rule with thresholding, or matching likelihood coefficients [more robust].
- Some examples of descriptors: image intensity function, correlation coefficients, MI, moment-based invariants, complex moments, ...
- Can also be used when no features have been detected previously.

4.2.3 Relaxation methods
~~~~~~~~~~~~~~~~~~~~~~~~
- One solution to the consistent labeling problem (CLP): To label each feature on each image that corresponds with the right label.
- Considers the 'match quality' of feature pairs and their neighbours and repeats iteratively until stability.
- Another solution to CLP uses backtracking.


Pyramids and wavelets
~~~~~~~~~~~~~~~~~~~~~
- Goal: reduce computational cost due to large image size.
- Coarse-to-fine hierarchical strategy: starts with both images at a coarse resolution and gradually improve the estimates of correspondence while going up to finer resolutions.
- Registration with respect to large-scale features comes first. This is good as long as there are no errors -> include a backtracking or consistency check.



5. Transform model estimation
=============================
- We must construct the mapping function to transform the sensed image to overlay it over the reference image.
- The type of mapping function corresponds to: geometric deformation of sensed image, method of image acquisition and required accuracy of the registration.


5.1 Global mapping models
-------------------------
- Use ALL CPs! for estimating one set of the mapping function parameters valid for the entire image.
- Can not properly handle images deformed locally -> Not good for Medical imaging!

- Various formulas depending on the transformations required: bivariate polynomials of low degrees, affine transform, perspective projection model.
- In general, the number of CPs is usually higher that the number required to determine the mapping function.
- The parameters of the function are computed using least-square fit, so that the polynomials minimize the sum of squared errors at the CPs.


5.2 Local mapping models
------------------------
- Tessellate the image and define the parameters of the mapping function for each patch separately.
- Many methods: weighted least square, weighted mean, piecewise linear mapping, piecewise cubic mapping.


5.3 Mapping by radial basis functions
-------------------------------------
- Representatives of the group of Global mapping methods but can handle locally varying geometric distortions.
- Originally developed for interpolation of irregular surfaces.
- These function have very small global influence and even significant local deformations can be well registered. -> Good for Medical imaging!
- Most often used representatives: Thin-plate splines (TPS). Gives good results but can be time consuming of the number of CPs is high.
- Other representatives of the spline family for this:
  - A linear combination of translated cubic B-splines 
  - Elastic body spline (EBS) [paper on breasts [39]]

5.4 Elastic registration
-----------------------
- Technique that does NOT use any parametric mapping functions where the estimation of deformation is reduced to look for the best parameters.
- Instead: the image is viewed as pieces of a rubber sheet, on which external forces defined by constraints are applied to bring them into alignment with the minimal amount of bending and stretching. 
- Registration is achieved by locating the minimum energy state in an iterative fashion. Usually uses a pyramidal approach.
- Disadvantage: when deformations are very localized -> solution: Fluid registration.

- Fluid registration:
  - Image is modelled as a thick fluid that flows to match the sensed image.
  - Mainly used in Medical Imaging! [25 quite old] [212 comparison of costs]
  - Weakness: blurring introduced during registration.

- Diffusion registration: handles object contours and features as membranes.
- Optical flow approach: motivated by estimation of relative motion between images. Many methods -> beyond the scope of this paper.

- Large deformation models (diffeomorphisms)[*]



6. Image resamplig and transformation
=====================================
- Use the mapping functions to transform the sensed image and register the images.
- Two approaches:
  * Forward 
    - Transform each pixel from the sensed image using the mapping functions.
    - Complicated to implement and can produce holes or overlaps (caused by discretization and rounding).
  * Backward:
    - The data for the output image is determined using the coordinates of the target pixel (reference image's coordinate system) and the inverse of the estimated mapping function.
    - The interpolation is done via convolution with an interpolation kernel. Preferably separable interpolants.
      - Bilinear interpolation offers a good trade-off -> most commonly used.



7. Evaluation of the image registration accuracy
================================================
- Estimate objectively how accurate is the registration.
- Hard to distinguish between the registration inaccuracies and actual physical differences in the image contents.
- Without quantitative evaluation, no registration method can be accepted for practical utilization.

- Errors depend on the stage:
  * Localization error
    - Displacement of CP coordinates due to inaccurate detection.
    - Can't be measured directly on the given image.
    - Solution: select an 'optimal' feature detection algorithm.
    - Sometimes we prefer more CPs with localization errors than too few.
  * Matching error (BAD PROBLEM)
    - Number of false matches when establishing correspondence between CP candidates.
    - Solution: Do a consistency check by applying two different matching methods to the same set of CP candidates.
                Or, cross validate the CP pairs.
  * Alignment error
    - Difference between the mapping model used for the registration and the actual between-image geometric distortion.
    - Always present in practice.
    - To evaluate it:
      - Mean Square Error at the CPs (CPE), not a good error measure but commonly used.
      - Test Point Error (TPE), more meaningful than CPE.
      - Consistency check using multiple cues. Compare to another registration with a 'gold standard method'.  
      - Visual assessment by a domain expert.

8. Current trends and outlook for the future
============================================
- Automatic registration is key for many areas but still remains an open problem.
- If we want to register images with non-linear, locally dependent geometric distortions, we are faced with some problems:
  1. How to match the CPs
     - Generally unsolvable
  2. What mapping functions to use
     - Can be solved, in theory, by using appropriate radial basis functions.
  3. How to distinguish between image deformations and real changes of the scene.



-----

[*] Obtained from Wikipedia.
