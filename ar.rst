Augmented Reality
=================

While Glass has a relatively small display and won't be able to create full immersive overlays, there are several interesting applications of AR that have been under-explored including short term "micro-interactions".  We have put together the building blocks to get you started, this will require understanding how they work to use them properly and this module is highly experimental.

.. figure:: ardimensions.png
   :alt: Glass Prism, Photo, Preview (640x360) relationships

   This shows the relationship between the Prism, Photo, and Preview (640x360) images.

hPhotoToGlass: Slightly different for each Glass and good results require calibation (see the wearscript-ar repo).
hPreviewToPhoto: Each preview image has a different area (not all sizes are supported, see below) however they are constant across devices; however, changes in the underlying Glass camera code have caused changes in the past.


displayWarpView([Array hPhotoToGlass]) : void
  Warps each preview image to the display such that it overlaps with what the user sees (works for objects > 7ft away, currently supported resolutions are 640x360 and 1280x720).  If the hPhotoToGlass homography is not provided a default is used; however, it won't match perfectly, each Glass is slightly different and they only need to be calibrated once.

.. raw:: html

    <script src="https://gist.github.com/9302540.js?file=glass.html"></script>

warpPreviewSampleGlass([Function callback]) : void
  Publishes the next preview image it gets, it is used to match subsequent images to, a local copy is stored as the overlay and can be replaced using WS.warpSetOverlay.

  * Callback has parameters of the form function `callback(Homography array)`

warpSetOverlay(String imageb64) : void
  Sets the overlay being warped that corresponds with the last sample selected.

.. raw:: html

    <script src="https://gist.github.com/9876016.js?file=glass.html"></script>
