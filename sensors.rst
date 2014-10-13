Sensors
=======

sensorOff(int type) : void
   Turns off sensor


sensorOn(String type, double period, [Function callback]) : void
     Turn on the sensor and produce data no faster than the specific period.
     Optional callback name that is called at most once per period of the form `function callback(data)` with data being an object with the properties:

     :name(string): Unique sensor name (uses Android name if one exists)
     :type(int): Unique sensor type (uses Android type if one exists), convert between them using WS.sensor(name) -> type
     :timestamp(double): Epoch seconds from when we get the sensor sample (use this instead of Raw unless you know better)
     :timestampRaw(long): Potentially differs per sensor (we use what they give us if available), but currently all but the light sensor are nanosec from device uptime
     :values(double[]): Array of float values (see WS.sensor docs for description)

              For the Android built in sensors see the Android docs for their values, custom values are:
                - battery: Values [battery_percentage] (same as displayed in the Glass settings)
                - pupil: Values [pupil_y, pupil_x, radius]
                - gps: Values [lat, lon]

iBeacons
--------

WS.beacon(fn range, fn enter, fn exit) : void
    Starts scanning for beacons and performs callbacks.
    The fields in the callback are channel_name, UUID, powerLevel, majorNum, and minorNum.

Sensor Types
------------
Sensors have unique names and integer types that are used internally and can be used as WS.sensor('light') which returns 5.  The standard Android sensor types are positive and custom types are given negative numbers.

===================  =======
      Type            Value
===================  =======
ibeacon               -8
pupil                 -2
gps                   -1
accelerometer         1
magneticField         2
orientation           3
gyroscope             4
light                 5
gravity               9
linearAcceleration    10
rotationVector        11
===================  =======
