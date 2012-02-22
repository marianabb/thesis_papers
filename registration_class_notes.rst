=======================================================
 Registration Class by Anders Brun --- 8 February 2012
=======================================================

To Read
=======

- Image Alignment and Stitching: A Tutorial -> http://research.microsoft.com/pubs/70092/tr-2004-92.pdf 

- Image Registration Methods: A Survey -> http://library.utia.cas.cz/prace/20030125.pdf [DONE]


Algorithms
==========
* Demons algorithm
  - Original paper: Thirion 1998 -> http://webdocs.cs.ualberta.ca/~dana/readingMedIm/papers/Thirion98.pdf
  - In the further readings of the paper 'Evaluation of 14 nonlinear...' I think is a more advanced version -> http://hal.inria.fr/docs/00/16/61/23/PDF/DiffeoDemons-MICCAI07-Vercauteren.pdf

* Morphon algorithm (available in ITK)
  - Code downloadable and everything -> http://www.insight-journal.org/browse/publication/320
  - More basic papers in the references.

* Iterative Closest Points (ICP)
  - Wikipedia has everything -> http://en.wikipedia.org/wiki/Iterative_closest_point
  - Check out PCL! -> http://pointclouds.org

* Scale-invariant feature transform (SIFT) 1999
  - Patented in the US by the University of British Columbia
  - Main page (demo available) -> http://www.cs.ubc.ca/~lowe/keypoints/
  - Wikipedia has a lot -> http://en.wikipedia.org/wiki/Scale-invariant_feature_transform

  - [EXTRA] Creator David Lowe has an interesting course: Image Understanding II, with a part about registration:
    - Book (sections 4.1, 6.1): Computer Vision: Algorithms and Applications by Richard Szeliski -> http://szeliski.org/Book/
    - Interesting papers:
      - http://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf (Main SIFT paper)
      - http://www.cs.ubc.ca/~lowe/525/papers/calonder_eccv10.pdf (BRIEF: Binary Robust Independent Elementary Features)
      - http://www.cs.ubc.ca/~lowe/525/papers/seitz06.pdf (Exploring Photo Collections in 3D)
      - Multi-View Stereo for Community Photo Collections
        - Paper -> http://www.cs.ubc.ca/~lowe/525/papers/goesele-2007.pdf     
        - Web page -> http://grail.cs.washington.edu/projects/mvscpc/

* Speeded Up Robust Feature (SURF) 2006
  - Partly inspired by SIFT. Faster than SIFT.
  - There are many implementations, most of the open source.
  - Wikipedia has all implementations -> http://en.wikipedia.org/wiki/SURF
  - Web page -> http://www.vision.ee.ethz.ch/~surf/index.html

* Other projects that do 3D registration of photos
  - Photosynth (Microsoft Live Labs) -> http://en.wikipedia.org/wiki/Photosynth
  - Bundler
  - PixelStruct


------------------------------------------------

=========================================
 Meeting with Anders --- 9 February 2012
=========================================

- I showed him the time plan. He agrees with it.
- We will not focus on implementing a registration method, we will use already available methods. Possibly letting the user choose which method to use, after we have already filtered the worse ones.
- We will focus on the part about detecting small changes (possibly showing them in the interface) and quantifying them.
- Anders believes that it would be good if the final product is a 3DSlicer module instead of an independent application. Because a module can be reused and we would also be able to make it public and receive comments and reviews from people who work in the area. It depends on whether we have all the tools that we need available in 3DSlicer.

- Specifically about 3D Slicer:
  - "View" -> "Python interactor" in 3DSlicer allows us to use the python command line directly inside 3DSlicer and access all variables and methods. This is awesome!
  - This means that we'll probably be able to substract volumes and do most of the things we can do in MatLab in 3DSlicer (hopefully, it remains to be proven).
- OpenCV could also be useful, can be used from Python but not necessarily from 3DSlicer.
- Probably useful book: Programming Computer Vision with Python -> http://www.maths.lth.se/matematiklth/personal/solem/book.html
