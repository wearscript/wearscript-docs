Wifi
====

wifiOn([Function callback]) : void
  Turn on wifi scanning with optional callback

  * Callback has parameters of the form function callback(results) where results is an array of objects each of the form following `ScanResult <http://developer.android.com/reference/android/net/wifi/ScanResult.html>`_ except for the timestamp...
  * timestamp(double): Epoch seconds from when we get the wifi scan
  * capabilities(string):  Authentication, key management, and encryption schemes supported by the access point
  * SSID(string): network name
  * BSSID(string):  address of the access point
  * level(int): detected signal level in dBm (may have a different interpretation on Glass)
  * frequency(int):  frequency in MHz of the channel over which the client is communicating with the access point

.. code-block:: javascript

    WS.wifiOn(function (results) {
        WS.log(JSON.stringify(results));
    });

wifiScan() : void
  Request a wifi scan.

.. code-block:: javascript

    WS.wifiScan();

wifiOff() : void
  Turn off wifi

.. code-block:: javascript

    WS.wifiOff();
