Basics
======

.. raw:: html

  <div style="position: relative;margin-bottom: 30px;padding-bottom: 56.25%;padding-top: 30px; height: 0; overflow: hidden;">
    <iframe style="position: absolute;top: 0;left: 0;width: 100%;height: 100%;" src="http://www.youtube.com/embed/en5jDhPok_8" frameborder="0"></iframe>
  </div>

In your Javascript environment, an object `WS` is initialized and injected for you with the following methods.

scriptVersion(int version) : boolean
     Checks if the webview is running on a specific version.

.. code-block:: javascript

    if (WS.scriptVersion(1)) return;

say(String message) : void
   Uses Text-to-Speach to read text

.. code-block:: javascript

    WS.say('Welcome to wearscript');

serverConnect(String server, Function callback) : void
  Connects to the WearScript server, if given '{{WSUrl}}' as the server it will substitute the user configured server.  Some commands require a server connection.

  * Callback takes no parameters and is called when a connection is made, if there is a reconnection it will be called again.


.. code-block:: javascript

   WS.serverConnect('{{WSUrl}}', function () {
       WS.say('connected');
   });


log(String message) : void
  Log a message to the Android log and the JavaScript console of the webapp (if connected to a server).

.. code-block:: javascript

    WS.log('Welcome to wearscript');

sound(String type) : void
  Play a stock sound on Glass.  One of TAP, DISALLOWED, DISMISSED, ERROR, SELECTED, SUCCESS.

.. code-block:: javascript

    WS.sound('SUCCESS');

shutdown() : void
  Shuts down wearscript

.. code-block:: javascript

    WS.shutdown();


activityCreate() : void
  Creates a new activity in the foreground and replaces any existing activity (useful for bringing window to the foreground)

.. code-block:: javascript

    WS.activityCreate();

activityDestroy() : void
  Destroys the current activity.

.. code-block:: javascript

    WS.activityDestroy();

wake() : void
  Wake the screen if it is off, shows whatever was there before (good in combination with WS.activityCreate() to bring it forward).

.. code-block:: javascript

    WS.wake();

liveCardCreate(boolean nonSilent, double period) : void
  Creates a live card of your activity, if nonSilent is true then the live card is given focus.  Live cards are updated by polling the current activity, creating a rendering, and drawing on the card.  The poll rate is set by the period.  Live cards can be clicked to open a menu that allows for opening the activity or closing it.

liveCardDestroy() : void
  Destroys the live card.


displayWebView() : void
  Display the WebView activity (this is the default, reserved for future use when we may have alternate views).
