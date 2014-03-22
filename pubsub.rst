Publish/Subscribe
==================

publish(String channel, args...) : void
  Sends PubSub messages to other devices

subscribe(String channel, Function callback) : void
  Receives PubSub messages from other devices.  Callback is provided the data expanded (e.g., if ['testchan', 1] is received then callback('testchan', 1) is called).  Using javascript's 'arguments' functionality to get variable length arguments easily.


Example: Ping/Pong
------------------

.. code-block:: html

    <!-- WearScript on Glass/Android -->
    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', function () {
	    WS.subscribe('pong', function (chan, timestamp0, timestamp1, groupDevice) {
	        WS.log('Pong: ' + groupDevice + ': Remote - Glass0: ' + (timestamp1 - timestamp0) + ' Glass1 - Glass0: ' + ((new Date).getTime() / 1000) - timestamp0);
	    });
	    setInterval(function () {
	        WS.publish('ping', 'pong', (new Date).getTime() / 1000);
	    }, 250);
	});
    }
    window.onload = main;
    </script></body></html>


To start the manager in client mode (i.e., it connects to the Go server) use the following.  All messages are sent example_ping <-internet-> go server <-internet-> glass which means you will have higher latency than connecting directly but you can stil use the Playground fully.  This is normally recommended for development.

.. code-block:: bash

    python example_ping.py client <client endpoint here>


To start the manager in server mode (e.g., Glass connect directly to this script, bypassing the Go server) use the following.  Note that in your script you have to change WS.serverConnect to use your server's ip and port (e.g., ws://localip:localport) which mean you won't get playground connectivity (e.g., no logs, can't resend code) until you stop the script ("Ok Glass" -> "Wear a Script" -> Scroll to Stop).  The benefit is that you can get substantially lower latency connecting directly to your computer.

.. code-block:: bash

    python example_ping.py server <server port here>

.. code-block:: python

    # Python: Client or Server
    import wearscript
    import argparse
    import time

    def callback(ws, **kw):

	def get_ping(chan, resultChan, timestamp):
	    ws.publish(resultChan, timestamp, time.time(), ws.group_device)

	ws.subscribe('ping', get_ping)
	ws.handler_loop()

    wearscript.parse(callback, argparse.ArgumentParser())

.. code-block:: python

    // Go: Server
    package main

    import (
        "code.google.com/p/go.net/websocket"
        "github.com/OpenShades/wearscript-go/wearscript"
        "fmt"
        "net/http"
        "time"
    )
    func wshandler(ws *websocket.Conn) {
	    // Single user mode, see wearscript-server for multi-user/device example
	    Manager, err := wearscript.ConnectionManagerFactory("server", "demo")
	    if err != nil {
		return
	    }
	    Manager.Subscribe("ping", func (c string, dataBin []byte, data []interface{}) {
	        resultChan, ok := data[1].(string)
	        if !ok {return}
	        Manager.Publish(resultChan, data[2], time.Now().UnixNano() / 1000000000., Manager.GroupDevice())
	    })
	    conn, _ := Manager.NewConnection(ws)
	    Manager.HandlerLoop(conn)
    }
    func main() {
        http.Handle("/", websocket.Handler(wshandler))
	err := http.ListenAndServe(":8081", nil)
	if err != nil {
	    fmt.Println("Serve error")
	}
    }


https://raw.github.com/wearscript/msgpack-javascript/master/msgpack.js
https://raw.github.com/wearscript/wearscript-js/master/wearscript-client.js

.. code-block:: html

    <!-- JavaScript client in a webpage -->
    <html><head><script src="msgpack.js"></script><script src="wearscript-client.js"></script></head>
    <body><script>
    var ws = new WearScriptConnection(new WebSocket('CLIENT ENDPOINT HERE'), "client", "demo");
    ws.subscribe('ping', function (chan, resultChan, timestamp) {
	ws.publish(resultChan, timestamp, (new Date).getTime() / 1000, ws.groupDevice);
    });
    </script></body></html>

Example: Image/Sensor Stream
----------------------------


.. code-block:: html

    <!-- WearScript on Glass/Android -->
    <html style="width:100%; height:100%; overflow:hidden">
    <body style="width:100%; height:100%; overflow:hidden; margin:0">
    <script>
    function main() {
	if (WS.scriptVersion(1)) return;
	WS.serverConnect('{{WSUrl}}', function () {
	    WS.sensorOn('accelerometer', .25);
	    WS.cameraOn(1);
	    WS.dataLog(false, true, .15);
	});
    }
    window.onload = main;
    </script></body></html>

.. code-block:: python

    # Python: Client or Server
    import wearscript
    import argparse


    def callback(ws, **kw):

	def get_image(chan, timestamp, image):
	    print('Image[%s] Time[%f] Bytes[%d]' % (chan, timestamp, len(image)))

	def get_sensors(chan, names, samples):
	    print('Sensors[%s] Names[%r] Samples[%r]' % (chan, names, samples))

	ws.subscribe('image', get_image)
	ws.subscribe('sensors', get_sensors)
	ws.handler_loop()

    wearscript.parse(callback, argparse.ArgumentParser())

.. code-block:: go

    // Go: Server
    package main

    import (
	   "code.google.com/p/go.net/websocket"
	   "github.com/OpenShades/wearscript-go/wearscript"
	   "fmt"
	   "net/http"
    )

    func wshandler(ws *websocket.Conn) {
	 // Single user mode, see wearscript-server for multi-user/device example
	 Manager, err := wearscript.ConnectionManagerFactory("server", "demo")
	 if err != nil {
	    return
	    }
	    Manager.Subscribe("image", func (c string, dataBin []byte, data []interface{}) {
				       timestamp := data[1].(float64)
						 image := data[2].(string)
						       fmt.Println(fmt.Sprintf("Image[%s] Time[%f] Bytes[%d]", c, timestamp, len(image)))
						       })
						       Manager.Subscribe("sensors", func (c string, dataBin []byte, data []interface{}) {
										    names := data[1]
											  samples := data[2]
												  fmt.Println(fmt.Sprintf("Sensors[%s] Names[%v] Samples[%v]", c, names, samples))
												  })
												  conn, _ := Manager.NewConnection(ws)
												  Manager.HandlerLoop(conn)
    }

    func main() {
	 http.Handle("/", websocket.Handler(wshandler))
	 err := http.ListenAndServe(":8081", nil)
	 if err != nil {
	    fmt.Println("Serve error")
	    }
    }


.. code-block:: html

    <!-- JavaScript in a webpage -->
    <html><head><script src="wearscript-client.js"></script></head>
    <body><script>
    var ws = new WearScriptConnection(new WebSocket(URL), "client", "demo");
    ws.subscribe('image', function (chan, timestamp, image) {
	console.log(JSON.stringify({chan: chan, timestamp: timestamp,
							   image: btoa(image)}));
    });

    ws.subscribe('sensors', function (chan, names, samples) {
	console.log(JSON.stringify({chan: chan, names: names,
						       samples: samples}));
    });
    </script></body></html>
