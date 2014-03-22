User Input
==========

gestureCallback(String event, Function callback) : void
  Register to get gesture events using the string of one of the events below (following GDK names, see below).

  Event Types
          :onGesture(String gesture): The gestures that can be returned are `listed here <https://developers.google.com/glass/develop/gdk/reference/com/google/android/glass/touchpad/Gesture>`_: LONG_PRESS, SWIPE_DOWN, SWIPE_LEFT, SWIPE_RIGHT, TAP, THREE_LONG_PRESS, THREE_TAP, TWO_LONG_PRESS, TWO_SWIPE_RIGHT, TWO_SWIPE_UP, TWO_TAP
          :onGesture<GESTURE>(): Shorthand for a specific gesture (e.g., onGestureTAP).
          :onFingerCountChanged(int previousCount, int currentCount): see `FingerListener <https://developers.google.com/glass/develop/gdk/reference/com/google/android/glass/touchpad/GestureDetector.FingerListener#onFingerCountChanged(int, int)>`_ in GDK
          :onScroll(float displacement, float delta, float velocity): see `ScrollListener <https://developers.google.com/glass/develop/gdk/reference/com/google/android/glass/touchpad/GestureDetector.ScrollListener#onScroll(float, float, float)>`_ in GDK
          :onTwoFingerScroll(float displacement, float delta, float velocity): see `TwoFingerScrollListener <https://developers.google.com/glass/develop/gdk/reference/com/google/android/glass/touchpad/GestureDetector.TwoFingerScrollListener#onTwoFingerScroll(float, float, float)>`_ in GDK
          :onEyeGesture(String gesture): One of WINK, DOUBLE_WINK, DOUBLE_BLINK, DON, DOFF
          :onEyeGesture<GESTURE>: Shorthand for a specific gesture (e.g., onEyeGestureWINK)

.. code-block:: javascript

    WS.gestureCallback('onGesture', function (gesture) {
        WS.say(gesture);
    });
    WS.gestureCallback('onFingerCountChanged', function (i, i2) {
	WS.log('onFingerCountChanged: ' + i + ', ' + i2);
    });
    WS.gestureCallback('onScroll', function (v, v2, v3) {
	WS.log('onScroll: ' + v + ', ' + v2 + ', ' + v3);
    });
    WS.gestureCallback('onTwoFingerScroll', function (v, v2, v3) {
	WS.log('onTwoFingerScroll: ' + v + ', ' + v2 + ', ' + v3);
    });

.. code-block:: javascript

    WS.gestureCallback('onGestureTAP', function () {
        WS.say('tapped');
    });

.. code-block:: javascript

    WS.gestureCallback('onEyeGesture', function (gesture) {
        WS.say(gesture);
    });


.. code-block:: javascript

    WS.gestureCallback('onEyeGestureDOUBLE_BLINK', function () {
        WS.say('double blink');
    });


speechRecognize(String prompt, Function callback) : void
  Displays the prompt and calls your callback with the recognized speech as a string

  Callback of the form `function mycallback(String recognizedText)`

.. code-block:: javascript

    WS.speechRecognize('Say Something', function speech(data) {
        WS.say('you said ' + data);
    });

qr(Function callback) : void
   Open a QR scanner, return scan results via a callback from zxing

   Callback of the form `function mycallback(data, format)`
     :String data: The scanned data (e.g., http://wearscript.com) is returned
     :String format: The format of the data (e.g., QR_CODE)

.. code-block:: javascript

    WS.qr(function (data) {
        WS.say(data);
    });
