Arduino
=======

Using an Arduino with WearScript you can integrate all varieties of sensors, input devices, LEDs, etc.  In the wearscript-arduino repo we have an example manager.py that connects to the Arduino over a serial connection.  There are folders of examples (described below) that contain both a wearscript and an arduino file (you flash the arduino yourself).

To start the manager in client mode (i.e., it connects to the Go server, see PubSub for details) use the following.

.. code-block:: guess

    python manager.py --serialport /dev/ttyACM0 client <client endpoint here>


To start the manager in server mode (e.g., Glass connect directly to this script, bypassing the Go server) use the following.  See PubSub for details on how to get Glass to connect locally and caveats of doing so.

.. code-block:: guess

    python manager.py --serialport /dev/ttyACM0 server <server port here>

Basic
-----

Connected to an Arduino (tested with Uno and Due), the following uses the scroll gesture to control a servo on Pin 9 and an LED on pin 13 (both specified in the .ino file).  We access them by sending data of the form ["arduinobasic", device, value] where devices index contiguously into servo pins then LEDs (i.e., binary pin, need not control an LED) specified in the .ino file.  For Servos the value is written directly to the servo and for LEDs any non-zero value is "on" and zero is "off".

.. code-block:: guess

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0" bgcolor="#000000"><script>
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', function () {
	    WS.gestureCallback('onScroll', function (v, v2, v3) {
		var v = Math.min(180, Math.max(0, v / 6));
		WS.publish('arduinobasic', 0, Math.round(v));
	    });
	    WS.gestureCallback('onGesture', function (v) {
		if (v == 'TAP')
		    WS.publish('arduinobasic', 3, 0);
		else if (v == 'TWO_TAP')
		    WS.publish('arduinobasic', 3, 1);
	    });
	});
    }
    window.onload = main;
    </script></body></html>

Neopixel
---------


