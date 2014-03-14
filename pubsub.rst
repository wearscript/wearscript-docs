PubSub Messages
===============

publish(String channel, args...) : void
  Sends PubSub messages to other devices

subscribe(String channel, Function callback) : void
  Receives PubSub messages from other devices.  Callback is provided the data expanded (e.g., if ['testchan', 1] is received then callback('testchan', 1) is called).  Using javascript's 'arguments' functionality to get variable length arguments easily.
