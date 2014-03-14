Camera
======


cameraOn(double imagePeriod, [int maxHeight, int maxWidth, Function callback]) : void
  Continuously capture camera frames. Grab output via cameraCallback.

cameraPhoto(Function callback) : void
  Take a picture and callback to `callback(String imageb64)` with a base64 encoded jpeg.

cameraPhotoPath(Function calback) : void
  Take a picture and save it to the SD card and callback to `callback(String path)`. Can be used as src attribute for an image.

cameraVideo() : void
  Record a video and save to the SD card.

cameraOff() : void
  Turns off camera


TODO: Figure out the syntax for this
.. code-block:: javascript

    WS.cameraPhoto(function () {
    });


.. code-block:: javascript

    WS.cameraVideo();

.. code-block:: javascript

    WS.cameraOn(2., 360, 640, function (data) {
        // TODO: Set data in image
    });
