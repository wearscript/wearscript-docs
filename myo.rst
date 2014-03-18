Myo
===

.. code-block:: guess

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function server() {
	WS.myoTrain()
	WS.gestureCallback('onMyo', function (x) {
	    WS.say(x);
	});
	WS.sensorOn('myoOrientation', .15, function (data) {
	    WS.log('got data: ' + JSON.stringify(data));
	});
	WS.sensorOn('myoAccelerometer', .15);
	WS.sensorOn('myoGyroscope', .15);
	WS.dataLog(false, true, .15);
    }
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', server);
    }
    window.onload = main;
    </script></body></html>
