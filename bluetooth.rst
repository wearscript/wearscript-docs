Bluetooth
=========


Keyboard -> Glass
------------------

By connecting a BT keyboard to Glass you can control the timeline and use the keypresses inside WearScript like you would in a normal webpage.  Keyboards like this http://www.amazon.com/dp/B005C6CVAE/ work well with Glass.  First we have to install the Settings.apk so that we can access the pairing options.  Install the Settings.apk from here http://www.glassxe.com/2013/05/23/settings-apk-and-launcher2-apk-from-the-hacking-glass-session-at-google-io/ and start the settings using

.. code-block:: bash

  adb shell am start -a android.intent.action.MAIN -n com.android.settings/.Settings

You may want to remove the settings apk after you are done as it cause annoying crashes on startup

.. code-block:: guess

  adb uninstall com.android.settings

Put your device into a bluetooth pairing mode.  Select Bluetooth, then use the touchpad to select your device.  You'll be asked to type a code using the keyboard and then you are all done.

Arduino <-> Glass
------------------

Bluetooth module: http://www.amazon.com/dp/B0093XAV4U

Arduino code.  Make sure to plug the RX from BT into TX of arduino which is pin 2 below.

.. code-block:: c

    #include <SoftwareSerial.h>
    SoftwareSerial mySerial(3, 2); // RX, TX
    void setup() {
      Serial.begin(57600);
      while (!Serial) {
      }
      mySerial.begin(9600);
    }

    void loop() {
      if (mySerial.available())
          Serial.write(mySerial.read());
      if (Serial.available()) {
          delay(10); // HACK
          mySerial.write(Serial.read());
      }
    }

.. code-block:: html

    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function main() {
          if (WS.scriptVersion(1)) return;
          WS.serverConnect('{{WSUrl}}', function () {
              WS.bluetoothWrite('20:13:12:05:04:11', 'bluetooth message from glass to arduino');
              WS.bluetoothRead('20:13:12:05:04:11', function (c) {
                    WS.log('BT:' + c)
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
