Eye Tracking
============

We developed a custom eye tracker for Glass so that we can experiment with using it as an input device.  It is for research and development purposes, there is a lot of potential for this sub-project but it is still early so bare with us.  See the video below for details of this project.  Code is available at https://github.com/wearscript/wearscript-eyetracking

.. raw:: html

  <div style="position: relative;margin-bottom: 30px;padding-bottom: 56.25%;padding-top: 30px; height: 0; overflow: hidden;">
    <iframe style="position: absolute;top: 0;left: 0;width: 100%;height: 100%;" src="http://www.youtube.com/embed/QSn6s3DPTSg" frameborder="0"></iframe>
  </div>


.. raw:: html

https://github.com/wearscript/wearscript-eyetracking/blob/master/hardware/eye_tracker4.stl
  <script src="https://embed.github.com/view/3d/wearscript/wearscript-eyetracking/master/hardware/eye_tracker4.stl"></script>

Getting Started
---------------

* The only OS recommended is Ubuntu Linux, it would be possible (though not easy because of opencv's lacking camera support on osx) to get it working on OSX and if you only have Windows consider using an Ubuntu virtual machine.
* Install the dependencies: gevent, wearscript-python, numpy, opencv (need a recent version! if you have a brand ubuntu 13.10+ you can use apt-get, else build from source).
* Acquire/build an eye tracker (best to contact brandyn in IRC)
* Run python track_debug.py debug, that will show you the eye camera and it should show a ring around your pupil
* We have several things you can do with it after this, but we are tidying things up (more info will be posted here after)


Building an Eye Tracker
------------------------

* http://www.amazon.com/Microsoft-LifeCam-HD-6000-Webcam-Notebooks/dp/B00372567A
* 2 LEDs (recommend you get 4+ as you may break/lose some while soldering): http://www.digikey.com/product-detail/en/SFH%204050-Z/475-2864-1-ND/2207282 (shipping was $2.50 and each LED is ~$.75-$1 depending on amount)
* Access to a 3d printer (print the .stl file, currently #2 is the best design)
* Skills: Need to teardown a camera (requires patience), surface mount (de)solder LEDs (is doable with normal soldering equipment), need to take off/break the IR filter on the optics (requires the care)
* Tools (see build video): Side cutter, thin phillips screwdriver, spudger/x-acto knife (remove IR filter), soldering iron, solder, prybar, wrench (for cracking open case)

.. raw:: html

  <div style="position: relative;margin-bottom: 30px;padding-bottom: 56.25%;padding-top: 30px; height: 0; overflow: hidden;">
    <iframe style="position: absolute;top: 0;left: 0;width: 100%;height: 100%;" src="http://www.youtube.com/embed/uoeUJYn5C-g" frameborder="0"></iframe>
  </div>

Tips
-----

* The webcam must be manually focused, if it appears blurry twist the lens until the image is in focus.  It may help to take the webcam out of the plastic mount and hold it roughly where it would be while doing this.
