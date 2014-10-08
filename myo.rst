Myo
===
The Myo uses bluetooth-le and can only be connected to Android Kit Kat devices (e.g., Nexus 5).

This demonstrates pairing the Myo, saying out loud which gesture is performed. Remember, the setup gesture must be performed every time the device is paired before it will return data. NONE is produced after a gesture is stopped (e.g., a clenched fist will stay FIST until it is released and then will be NONE).  It also streams the accelerometer, orientation, and gyro to the sensors tab (see WS.dataLog).

.. code-block:: html

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function server() {
	WS.myoPair(function () {
      WS.say("paired myo");
  });
	// Currently one of {NONE, FIST, FINGERS_SPREAD, WAVE_IN, WAVE_OUT, THUMB_TO_PINKY, ARM_RECOGNIZED, ARM_LOST}
	WS.gestureCallback('onMyo', function (x) {
	    WS.say(x);
	});
	// Value is a quaternion (w, x, y, z) and (pitch, yaw, roll) for convenience
	WS.sensorOn('myoOrientation', .15, function (data) {
	    WS.log('got data: ' + JSON.stringify(data));
	});
	// 3 axis accel (x, y, z)
	WS.sensorOn('myoAccelerometer', .15);
	// 3 axis gyro (x, y, z)
	WS.sensorOn('myoGyroscope', .15);
	WS.dataLog(false, true, .15);
    }
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', server);
    }
    window.onload = main;
    </script></body></html>
