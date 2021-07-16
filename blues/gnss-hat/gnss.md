# GPS Gets a Boost with the Mobile Tracker Hat

*Introducing a new product into the Blues Wireless ecosystem: the Mobile Tracker Hat external GPS module.*

If you've done any work with GNSS/GPS on low-power devices, you know what a struggle it can be in certain conditions. For instance, you may have experienced:

- Taking a long time (minutes!) to acquire satellite signals;
- Needing an unreasonably clear view of the sky;
- Consuming more power than what is reasonable for an edge computing device.

These are some of the reasons we decided to utilize the Quectel series of cellular modules on the Blues Wireless Notecard.

*Why?*

With a low-power draw, integrated GNSS, and support for many active or passive GPS antennas, the Quectel was an easy choice for providing both cellular and GPS capabilities on a single board.

IMAGE OF NOTECARD WITH GPS HIGHLIGHTED?

> While we may sometimes use GNSS and GPS interchangeably, GNSS (or Global Navigation Satellite System) is includes different types of satellite-based positioning systems. GPS (or Global Positioning System) is a type of GNSS.

## GPS and the Notecarrier

The tiny 30x35mm Notecard pictured above needs cellular and GPS antennas to be useful. This is why the Notecarrier-A and Notecarrier-AF models include integrated antennas.

IMAGE OF NOTECARRIER-A?

A positive side effect of this configuration is it allows for the Notecard + Notecarrier-A to be configured as a standalone asset tracker solution:

1. Events transferred by the Notecard are tagged with time and location.
2. Location is obtained using the GPS module, and time is available from both the cellular network and GPS.
3. To optimize power consumption, the Notecard has a MEMS-based, LIS2DTW accelerometer that determines when use of the GPS is not required.

Even if you're not building a "tracking" solution, the Notecard API was constructed in a way to allow for intuitive enabling and usage of GPS-acquired location data:

CODEZ

These requests result in responses like:

CODEZ

## Introducing the Mobile Tracker Hat

While I've painted a positive picture of GPS with the Notecard so far, what if your deployment:

- Doesn't have a reliable view of the sky?
- Struggles to acquire GPS satellites because of its physical location?
- Requires concurrent cellular and GPS connectivity?

It's because of these situations that we are happy to introduce a new, more powerful, GNSS/GPS solution for the Notecarrier-A: the Blues Wireless Mobile Tracker Hat.

IMAGE

The Mobile Tracker Hat uses the Quectel L86 GNSS module as an external GPS that sits directly on the exposed headers of the Notecarrier-A.

The Quectel L86 module provides numerous benefits, including:

- Low power consumption when active (~26mA)
- GPS "hot start"calculate and predict orbits automatically using up to three days of accumulated data
- High satellite sensitivity of -167dBm tracking and -149dBm acquisition

## Mobile Tracker Hat Use Cases

When might you want to use the Mobile Tracker Hat along with a Notecarrier-A you ask? 

### Concurrent Cellular Data and GPS

If you think about it, with a standard Notecarrier-A tracker setup, your cellular module has to switch back and forth between cellular to send data and GPS to track location. While this works fine for many implementations,there are times when those lag times of restarting and finding new cell towers or 
GPS satellites takes too long. Maybe you miss out in a couple of minutes 
of location data as the system switches functions. Not an issue any longer 
with this external GPS hat.

### GPS Hot Start

With this tracker hat, GPS signals will be super quick to lock on, because the 
almanac and ephemeris data will be retained between GPS readings. 
With a standard Notecarrier-A configuration, some of that data is retained, 
but not all of it, that’s called a warm start.

### Snap-On Support

The convenience of adding this to an existing Notecarrier-A tracker can’t really be matched.

## Next Steps

If you'd like to know more about the Mobile Tracker Hat, you can:

1) Take a look at the API commands required to use it. For instance, to configure it as a standalone tracker, you only have to issue a few requests:

```
{"req":"hub.set","mode":"continuous","product":"com.xxx.yyy:track"}
{"req":"card.aux","mode":"track","gps":true,"datarate":9600}
{"req":"card.location.mode","mode":"continuous"}
```

2) Browse the shop to buy your own!