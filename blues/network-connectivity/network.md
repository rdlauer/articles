# Use the Right Network Connection for Your IoT Solution

Depending on where you live, there are Internet connection options that we default to. In urban settings we have become accustomed to ubiquitous WiFi. In more rural areas cellular or maybe LoRaWAN are what we first consider.

Let me propose that we collectively challenge our assumptions of what the "best" network connectivity option is for our IoT deployments. What you once cast aside as too expensive, impractical, or unusable may now be a reality. Likewise, a combination of connectivity options may be what's right for you.

While this may seem self-serving...

Stick around as we go through each modern network connection option, their pros and cons, and which scenarios are best.

## Key Considerations

physical location, power requirements + power outages, high availability, low latency, range, customer ux

## Wired Ethernet

what it is

What is generally considered the fastest (low latency), pushes the most data (high bandwidth), and most reliable, wired ethernet is every IoT developer's deployment dream.

As much as we'd love the story to end there, clearly it doesn't. IoT deployments, especially those considered "edge devices", are rarely within distance of the wired ethernet options found in office buildings and even some homes. Aside from single board computers like Raspberry Pi, ethernet is rarely found packaged with IoT hardware.

Ethernet add-on modules are also not cheap and add some bulk to the average project.

There are tradeoffs of course.

When does wired ethernet win?

- Low latency and high availability are a critical requirements.
- You're pushing a lot of data (e.g. high res images and video).
- End user experience (if it's plug-and-play).
- It's available in the deployment location.

When does wired ethernet lose?

- Low-profile hardware solutions that can't fit the ethernet jack?
- Low-power installations (ethernet consumes)
- No-power scenarios (ethernet doesn't function without power)

## WiFi

Those of us old enough to remember the first WiFi routers can probably remember the first time we accessed the Internet in a wireless setting. WiFi has fundamentally altered how the average person connects to and interacts with the Internet.

WiFi is...



## Cellular



## LoRa and LoRaWAN



## Bluetooth

