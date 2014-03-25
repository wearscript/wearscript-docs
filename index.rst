WearScript: JS with Batteries Included for Glass
================================================
WearScript combines the power of Android development on Glass with the learning curve of a website.  Go from concept to demo in a fraction of the time. For an overview check out the intro video and sample script below.  Visit https://github.com/wearscript for the goods.

One-Line Installer(Linux/OSX): Execute the following in a shell to install WearScript on your Glass and authenticate with our default server.

.. code-block:: bash

  curl -L http://goo.gl/nRjW6y > install.py && python install.py

.. raw:: html

  <div style="position: relative;margin-bottom: 30px;padding-bottom: 56.25%;padding-top: 30px; height: 0; overflow: hidden;">
    <iframe style="position: absolute;top: 0;left: 0;width: 100%;height: 100%;" src="http://www.youtube.com/embed/0kqn4Q0qe0Q" frameborder="0"></iframe>
  </div>

.. code-block:: html

  <!-- Sample WearScript -->
  <html style="width:100%; height:100%; overflow:hidden">
  <body style="width:100%; height:100%; overflow:hidden; margin:0">
  <canvas id="canvas" width="640" height="360" style="display:block"></canvas>
  <script>
  function server() {
      WS.log('Welcome to WearScript');  // Write to Android Log and Playground console
      WS.say('Welcome to WearScript');  // Text-to-Speech
      WS.sound('SUCCESS')

      // Changes canvas color with head rotation
      WS.sensorOn('orientation', .15, function (data) {
	  ctx.fillStyle = 'hsl(' + data['values'][0] + ', 90%, 50%)'
	  ctx.fillRect(0, 0, 640, 360);
      });

      // Stream several sensors (can view in the Sensors tab)
      var sensors = ['gps', 'accelerometer', 'magneticField', 'gyroscope',
		     'light', 'gravity', 'linearAcceleration', 'rotationVector'];
      for (var i = 0; i < sensors.length; i++)
	  WS.sensorOn(sensors[i], .15);

      // Stream camera frames (can view in the Images tab)
      WS.cameraOn(.25);
      WS.dataLog(false, true, .15);

      // Hookup touch and eye gesture callbacks
      WS.gestureCallback('onTwoFingerScroll', function (v, v2, v3) {
	  WS.log('onTwoFingerScroll: ' + v + ', ' + v2 + ', ' + v3);
      });
      WS.gestureCallback('onEyeGesture', function (name) {
	  WS.log('onEyeGesture: ' + name);
      });
      WS.gestureCallback('onGesture', function (name) {
	  WS.log('onGesture: ' + name);
      });
      WS.gestureCallback('onFingerCountChanged', function (i, i2) {
	  WS.log('onFingerCountChanged: ' + i + ', ' + i2);
      });
      WS.gestureCallback('onScroll', function (v, v2, v3) {
	  WS.log('onScroll: ' + v + ', ' + v2 + ', ' + v3);
      });
  }
  function main() {
      if (WS.scriptVersion(1)) return;
      ctx = document.getElementById('canvas').getContext("2d");
      WS.serverConnect('{{WSUrl}}', server);
  }
  window.onload = main;
  </script></body></html>

..  toctree::
    :hidden:
    :maxdepth: 1

    basics
    gist
    cards
    input
    sensors
    camera
    pubsub
    arduino
    bluetooth
    ar
    eyetracking
    myo
    pebble
    tips
    wire
    contributing

About
-----

* `OpenShades <http://openshades.com>`_ (the new OpenGlass) is our community
* IRC freenode #wearscript and #openshades (if you want to collaborate or chat that's the place to be)
* Project Lead: Brandyn White (bwhite dappervision com)
* `G+ Community <https://plus.google.com/communities/101102785351379725742>`_ (we post work in progress here)
* `Youtube <https://www.youtube.com/channel/UCGy1Zo81X2cRRQ5GQYz8eEQ>`_ (all demo videos)
* `Dapper Vision, Inc. <http://www.dappervision.com>`_ (by Brandyn and Andrew) is the sponsor of this project
* Code is licensed under `Apache 2.0 <http://www.apache.org/licenses/LICENSE-2.0.html>`_ unless otherwise specified

Contributors
------------
See `contributors <https://github.com/bwhite/wearscript/graphs/contributors>`_ for details.  Name (irc nick)

* `Brandyn White (brandyn) <https://plus.google.com/109113122718379096525?rel=author>`_
* Andrew Miller (amiller)
* Scott Greenwald (scottgwald)
* Kurtis Nelson (kurtisnelson)
* Conner Brooks (connerb)
* Lance Vick (lrvick)
* Daniel Grove (iShortBus)
* Justin Chase (jujuman)
* Alexander Conroy (geilt)
