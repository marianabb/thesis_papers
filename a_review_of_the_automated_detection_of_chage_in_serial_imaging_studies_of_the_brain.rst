============================================================================================
 A Review of the Automated Detection of Change in Serial Imaging Studies of the Brain [2004] --- 13&14 February, 2012
============================================================================================
======================================
 Julia Patriache and Bradley Erickson
======================================

- A system which could reduce the large quantity of primitive data to a smaller and more informative subset of data, emphasizing change, would be useful.


1. Introduction
===============
- Many acquisition-related factors make detection of pathology-related change difficult:
  - One cannot assume a one-to-one correspondence between slices from one acquisition to the next.
  - A different scanner may be used in a followup scan.
  - Scanning parameters are not the same from one acquisition to the next.
- An essential component in the process of change detection is therefore not simply the detection of change nut the separation of acquisition-related change from disease-related change.  


2. Detailed review of the literature
====================================

2.1 Manual Inspection
---------------------
- MANY problems with this approach.
- Some authors have used subtraction to aid in the process, [but still requires manual work and its not exact].


2.2 Measurement Sampling [Not useful in our case]
------------------------
- Measurement of tumor largest diameter and largest corresponding perpendicular.
- It disregards location information, which might be important to the doctor.


2.3 Volumetrics [Not sure if useful for us]
---------------
- Compute global volume of each tissue of interest at each acquisition in a series, with the intention of subtracting this metric from one scan to the next.
- Problems: 
  - Lot of uncertainty from a mathematical standpoint.
  - Computing volumes over unnecessarily large regions in an image also affects uncertainty.
  - More problems if the delineation of the volume is done manually.
  - It is also possible for a tumor to change shape without changing volume.
- Studies strongly suggest that quantifying the volume of abnormal imaging will not necessarily provide and accurate representation of the volume and extent of histologic tumor burden.
   

2.4 Warping
-----------
- Refers to nonlinear registration.
- Use an elastic image registration algorithm to match surfaces from one acquisition to the next; the resulting surface deformations were used to derive volumetric deformation fields from which local measures of 3D tissue dilation and contraction were quantified.
- Tumor, edema and necrosis inherently blend into structures of which they are part. It is therefore nor appropriate to use surface warping approaches in case of tumors.
- Problems with nonlinear registration for change detection:
  - It is underconstrained: For a given pair of acquisitions, there are an infinite number of displacement fields that will yield a match. This problem is partly addressed by specifying the constraints under which the displacement field will be derived. Ex: use of continuum mechanical models, a viscous fluid model, etc.
  - The use of a fluid model is an approximation.
  - Warping algorithms are based on the assumption that a particular region of tissue exists in both acquisitions and has simply moved (One-to-one tissue correspondence). This is a requirement which tumors frequently do not meet.

- Approach by Thirion et al. [35] to disambiguate growth from tossie character change:
  - Use a warping algorithm to develop a vector displacement field. The user then gives the coordinates of the center of a lesion.
  - The algorithm places concentric shells around this point. The divergence of the warping field in integrated over the volume within; this yields the change in volume for that shell.


2.5 Temporal Analysis
---------------------
- View the images as a 4D dataset where the 4th dimension is time.



3. Steps in practical change detection system
=============================================
- A change detection system should not be thought of as am algorithm but rather as a pipeline of algorithms and steps.
- A good implementation if some steps could ameliorate the need for others.
- Steps:
  1. Preacquisition registration (patient in exact same position in each acquisition).
  2. Near-identical image acquisition (equivalent scanner)
  3. Scalp and extradural tissue removal.
  4. Correction for scaling changes
  5. Subvoxel brain registration.
  6. Sinc interpolation to apply scaling and registration transformation.
  7. Inhomogeneity correction
  8. Change computation
  9. Significant region detection
  10. Presentation of results.


3.1 Preacquisition Registration of scans
----------------------------------------
- Variability in volume quantification with positioning between scans is of critical importance, even before any image processing has been undertaken.
- Even sinc interpolation, which theoretically should not introduce errors, is generally windowed spatially and therefore introduces artifacts.
- Two forms:
  - Mechanic: Mask molded to the shape of the patient, head holders, ear plugs, ...
  - Electronic: Acquiring the followup volumes already in registration with the baseline scan by reorienting the acquisition planes.


3.2 Acquisition considerations
------------------------------
- If one could perform true 3D volumetric acquisitions with very small voxels in all pulse sequence, and one was using sinc interpolation with an infinitely large window, this would, to a certain extent, obviate the need of preacquisition registration.


3.3 Segmentation of Skull and Brain
-----------------------------------
- Several structures are not related to the brain and are not invariant, therefore have the potential to mislead the registration algorithm.


3.4 Scaling Correction
----------------------
- Gradient amplifiers of magnetic resonance imagers change over time, resulting in a corresponding scaling distortion.
- A standard linear registration algorithm could be used to compute the scaling factors, but it would be better to prevent these changes with adjustments to the scanner.
- [Important] It is inadvisable to incorporate this scaling into the registration algorithm proper. If this scaling correction were derived as a part of a registration algorithm based upon the brain itself, this assumption would be violated since the brain may change under a pathological process. 
  - Solution: Begin by computing the scaling transformation. Then at each iteration of the registration algorithm the trial transformation matrix (containing rotation and translation components) would be multiplied by the previously computed scaling matrix. The unified transformation would be applied as one.


3.5 Subvoxel registration
-------------------------
- Linear registration algorithms, nonlinear registration step following an initial linear registration step, ...

- Overview of an algorithm:
  - If volumes are out of alignment -> value of the cost function over all voxels will be fairly uniformly poor.
  - As volumes become aligned -> values of the cost function for more and more voxels will be more and more optimal.
  - Brains perfectly aligned -> value of the cost function at all voxels will be expected to occupy a distribution about some optimal level.
  - In the case of anatomy changes, a population of voxels will approach a distribution about the optimum level when the two volumes are registered, while a population of voxels will remain outliers. These outliers may be excluded from the overall cost function computation at each trial.
  - The volume-wide cost function at each iteration could then be computed as a combination of the number of voxels included in the cost function (which should be maximized) and the traditional metric such as the sum of the square of the differences (which should be minimized).


3.6 Sinc Interpolation
----------------------
- Once the scaling and linear registration transformations have been computed, they should be combined in order that at most ONE interpolation procedure be applied to the data.
- Sinc interpolation could be used in 3D if the volumes were acquired in true 3D. Fast sinc could also be used.


3.7 Inhomogeneity Correction
----------------------------
- Refers to using information about portions of anatomy that we know will remain invariant from one scan to the next.
- Using this during registration could be useful.


3.8 Change Computation
----------------------
- Relates to conversion of corrected image volumes into measures of change. It is related to classification because we must assign each voxel to various classes, depending upon the voxel's location in feature space.
- Two things of interest in change detection:
  - What tissue a particular voxel contains (similar to classification).
  - How that voxel moves through feature space, from one scan to the next.
- Great advantages result from considering both images together as a 4D dataset.

- In classification we know that voxel locations in feature space tend to center around clusters, each of which corresponds to a tissue.
- In change detection we know that change tends to occur along lines in feature space, between particular pairs of cluster centroids.
- Knowledge about what classes of transitions are physically possible results in a reduction of the number of possible "lines" in feature space to a manageable number.
  
- [They say that] an Euclidean classifier is necessary.


3.9 Significant region detection
--------------------------------
- At this point, data has been transformed from imaging data to change data. We need a method to reduce the number of false positives (voxels that appear to have changed but actually have not).
- Many methods:
  - Thresholding
  - Take into account the fact that changed rarely occur in isolation.
  - Using intelligent filters to "smear" large magnitude changes into their neighbors.
  - Use the likelihood ratio to test whether a group of voxels is changing.
  - A threshold based upon cluster size.


3.10 Presentation of results
----------------------------
- The (significant) changes remaining could be displayed in a volume, where each change type could be color-coded. The magnitude of the change in each voxel could be represented by the intensity of the color.



4. Discussion and Summary
=========================
- It would be good if the task of acquisition to acquisition comparison and quantitative analysis could be assigned to the computer, to which these tasks come naturally.
- The process of interpretation and judgment of those quantitative results should be given to the radiologist.




