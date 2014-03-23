Pebble
======
The Pebble connects to an android device over bluetooth, and requires the Pebble Application on the phone. The app can be downloaded from the play store.

To use Pebble with Wearscript, the Wearscript [Pebble client](https://github.com/wearscript/wearscript-pebble) must be installed on the Pebble. Installation instructions are included in the repository.

This script demonstrates pebble click events, setting text in the view, vibration, and using accelerometer data.

.. code-block:: html
    
  
    <html style="width:100%; height:100%; overflow:hidden">
    <head>
    </head>
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <canvas id="canvas" width="640" height="360" style="display:block"></canvas>
    <script>

    function cb(data) {
        WS.log('x: ' + data['values'][0] + ' y: ' + data['values'][1] + ' z: ' + data['values'][2]);
        WS.pebbleSetBody('x: ' + data['values'][0] + ' y: '+ data['values'][1] + ' z: ' + data['values'][2], false);
    }

    function onPebbleSingleClick(name) {
        WS.log('onPebbleSingleClick: ' + name);
        switch(name)
        {
            case 'up':
                WS.say('up');
                WS.pebbleSetTitle("pressed up", false);
                break;
            case 'down':
                WS.say('down');
                WS.pebbleSetTitle("pressed down", false);
                break;
            case 'select':
                WS.say('select');
                WS.pebbleSetTitle("pressed select", false);
                break;
        }
    }

    function onPebbleLongClick(name) {
        WS.log('onPebbleLongClick: ' + name);
        switch(name)
        {
            case 'up':
                WS.say('long up');
                WS.pebbleVibe(0);
                break;
            case 'down':
                WS.say('long down');
                WS.pebbleVibe(1);
                break;
            case 'select':
                WS.say('long select');
                WS.pebbleVibe(2);
                break;
        }
    }

    function server() {
        WS.log('Welcome to WearScript-Pebble');
        WS.say('Welcome to WearScript-Pebble');
        WS.pebbleSetBody('connected!', false);
        WS.sensorOn(WS.sensor("pebbleAccelerometer"), .25, 'cb');
        WS.pebbleSetSubTitle('Accel Data', false);
    }

    function main() {
        if (WS.scriptVersion(1)) return;
        WS.serverConnect('{{WSUrl}}', 'server');
    }
    window.onload = main;
    </script>
    </body>
    </html>

