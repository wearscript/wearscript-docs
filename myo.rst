Myo
===

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
