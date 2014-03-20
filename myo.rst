Myo
===
The Myo uses bluetooth-le and can only be connected to Android Kit Kat devices (e.g., Nexus 5).  As the SDK is not publicly available you'll need to get the Myo binary of WearScript (currently a private branch) from us.  If you are part of the Myo Alpha program we can add you to the repository.

This demonstrates training the Myo, saying out load which gesture is performed.  NONE is produced after a gesture is stopped (e.g., a clenched fist will stay FIST until it is released and then will be NONE).  It also streams the accelerometer, orientation, and gyro to the sensors tab (see WS.dataLog).

.. code-block:: guess

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function server() {
	WS.myoTrain()
	// Currently one of {NONE, FIST, FINGERS_SPREAD, WAVE_IN, WAVE_OUT}
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
