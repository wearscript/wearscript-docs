Publish/Subscribe
==================

publish(String channel, args...) : void
  Sends PubSub messages to other devices

subscribe(String channel, Function callback) : void
  Receives PubSub messages from other devices.  Callback is provided the data expanded (e.g., if ['testchan', 1] is received then callback('testchan', 1) is called).  Using javascript's 'arguments' functionality to get variable length arguments easily.


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
