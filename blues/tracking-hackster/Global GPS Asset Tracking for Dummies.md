# Global GPS Asset Tracking for Dummies

"If you don't know where you are going, you'll end up someplace else." - Yogi Berra.

I couldn't agree any more Yogi!

One of the more intriguing use cases to be born out of the IoT is the ability to actively monitor both the location and condition of, well, anything. Whether you are monitoring the temperature, humidity, and fall detection of a critical vaccine shipment to tracking your location on a cross-country roadtrip, using off-the-shelf sensors with combination cellular and GPS modules has made asset tracking easier than ever.

But what if you don't know where to start? Or what if you don't want to invest hundreds in a decked-out tracker? Well have I got a tutorial for you!

I built a low-cost (< $???) cellular- and GPS-enabled tracking system and sent it, literally, around the world. I tracked its location every day with a cloud-based dashboard and even tweaked some settings on the trackers themselves after I had sent them on their journey.

Let's dive into the hardware used, configuration required, and the cloud dashboard built to monitor this little tracker and its amazing journey around the globe.

## Low-Cost Hardware

When speccing out this project, I really wanted to double-down on the low-cost hardware angle. There are plenty of off-the-shelf asset tracking systems out there, but the prices are generally too high for a minimalist asset tracking solution.

So I started with the following high level minimum requirements:

1. GPS tracking module (no kidding)
2. Cellular module (the only realistic way to get global relaying of data)
3. Low-power hardware (long journey, can't use a power hog)
4. Antennas?
4. Preferably avoid pairing with a host microcontroller to save on power
5. The right battery considering price/performance

### GPS and Cellular in One

We all know the old adage: kill two birds with one stone. How violent! So how about kill three birds with one stone? Wait, still violent.

The core of my asset tracker is the Notecard from Blues Wireless. The Notecard is a device-to-cloud data pump with an onboard cellular and GPS module that, by default, is low power to the tune of ~8mA when idle. Prepaid.

Yes, with global connectivity to the tune of 137 countries and counting.

But I can't drop a Notecard in a box and send it around the world (yet). I need some antennas, a means of powering it, and probably a microcontroller to configure it (or do I?).

### The Right Antennas for the Job

Ever do a search for "cellular antenna" on Digikey? Good luck friend! Luckily Blues Wireless also provides the Notecarrier, which is a line of development boards meant to act as a bridge between a Notecard and your MCU or SBC of choice.

The advantage of the Notecarrier for us though, is two fold:

1. Certain Notecarrier models ship with low profile embedded antennas;
2. Notecarriers also allow for connections to solar panels and LiPo batteries.

