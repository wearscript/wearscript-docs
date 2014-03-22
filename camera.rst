Camera
======

cameraOn(double imagePeriod, [int maxHeight, int maxWidth, Function callback]) : void
  Continuously capture camera frames. If a callback is specified it is given the data base64 encoded.

.. code-block:: javascript

	WS.cameraOn(.5, 180, 320, function (x) {
	    document.getElementById('image').setAttribute('src', 'data:image/jpg;base64,' + x);
	});

cameraOff() : void
  Turns off camera

.. code-block:: javascript

	WS.cameraOn(.5, 180, 320, function (x) {
	    document.getElementById('image').setAttribute('src', 'data:image/jpg;base64,' + x);
	});

  setTimeout(function () {
	    WS.cameraOff();
	}, 5000);

cameraPhoto([Function callback]) : void
  Take a picture and save it to the SD card and callback to `callback(String path)`. Can be used as src attribute for an image.


.. code-block:: javascript

	WS.cameraPhoto(function (x) {
	    document.getElementById('image').setAttribute('src', 'file://' + x);
	});

cameraPhotoData([Function callback]) : void
  Take a picture and callback to `callback(String imageb64)` with a base64 encoded jpeg.

.. code-block:: javascript

	WS.cameraPhotoData(function (x) {
	    document.getElementById('image').setAttribute('src', 'data:image/jpg;base64,' + x);
	});

cameraVideo([Function callback]) : void
  Record a video and save to the SD card.

.. code-block:: javascript

  WS.cameraVideo(function (x) {
      WS.log(x);
  });
