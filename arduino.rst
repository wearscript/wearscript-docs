Arduino
=======

Using an Arduino with WearScript you can integrate all varieties of sensors, input devices, LEDs, etc.  In the wearscript-arduino repo we have an example manager.py that connects to the Arduino over a serial connection.  There are folders of examples (described below) that contain both a wearscript and an arduino file (you flash the arduino yourself).

To start the manager in client mode (i.e., it connects to the Go server, see PubSub for details) use the following.

.. code-block:: bash

    python manager.py --serialport /dev/ttyACM0 client <client endpoint here>


To start the manager in server mode (e.g., Glass connect directly to this script, bypassing the Go server) use the following.  See PubSub for details on how to get Glass to connect locally and caveats of doing so.

.. code-block:: bash

    python manager.py --serialport /dev/ttyACM0 server <server port here>

Basic
-----

Connected to an Arduino (tested with Uno and Due), the following uses the scroll gesture to control a servo on Pin 9 and an LED on pin 13 (both specified in the .ino file).  We access them by sending data of the form ["arduinobasic", device, value] where devices index contiguously into servo pins then LEDs (i.e., binary pin, need not control an LED) specified in the .ino file.  For Servos the value is written directly to the servo and for LEDs any non-zero value is "on" and zero is "off".

.. code-block:: html

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

Neopixels are independently addressable LED strips that can display multiple colors, change brightness, and display patterns over time.  Using the included neopixel .ino file and sample script https://api.wearscript.com/#/gist/9390329/glass.html complex patterns can be programmed and executed on the Arduino including loops.


Powertail
---------
The model used is https://www.adafruit.com/products/268#Description with an Arduino Uno

+--------------------------+---------------------------------------+
| Arduino Pin              | Connected to                          |
+--------------------------+---------------------------------------+
| 3.3v                     | power tail 1: +in                     |
+--------------------------+---------------------------------------+
| pin #4                   | power tail 2: -in                     |
+--------------------------+---------------------------------------+
| ground                   | power tail 3: Ground                  |
+--------------------------+---------------------------------------+

Bluetooth version
See the :ref:`bluetooth` page for setup details (same pins).


.. code-block:: guess

    #include <SoftwareSerial.h>

    SoftwareSerial mySerial(3, 2); // RX, TX

    void setup()  
    {
      Serial.begin(57600);
      while (!Serial) {
      }
      mySerial.begin(9600);
      pinMode(4, OUTPUT);
    }

    void loop()
    {
      if (mySerial.available()) {
	char c = mySerial.read();
	if (c == '0')
	  digitalWrite(4, 0);
	else
	  digitalWrite(4, 1);
	Serial.write(c);
      }
      if (Serial.available()) {
	delay(10); // HACK
	mySerial.write(Serial.read());
      }
    }

.. code-block:: guess

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', function () {
	    WS.gestureCallback('onGestureSWIPE_RIGHT', function () {
		WS.bluetoothWrite('20:13:12:05:04:11', '1');
	    });
	    WS.gestureCallback('onGestureSWIPE_LEFT', function () {
		WS.bluetoothWrite('20:13:12:05:04:11', '0');
	    });
	    WS.bluetoothList(function (devices) {
		WS.log(JSON.stringify(devices));
	    });
	});
    }
    window.onload = main;
    </script>
    </body>
    </html>
