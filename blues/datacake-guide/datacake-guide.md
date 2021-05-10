# Visualizing the IoT with Datacake and the Blues Wireless Notecard

The wave of self-described "low code" development solutions has come (and mostly gone) without much fanfare. While there have been some successes, generally speaking, **developers require control above any desire to cut corners**.

We now know the best low code solutions do just enough to get you 70% of the way there, but also provide that bare metal environment to customize the last 30% of your project.

This is where the intersection of the [Blues Wireless Notecard](https://blues.io/products/) and the low code IoT platform [Datacake](https://datacake.co/low-code-iot-platform) come in.

![blues wireless notecard and datacake](notecard-datacake.png)

## What is Datacake?

The Datacake website does a fantastic job of describing what the platform is, at a high level:

> Datacake is a multi-purpose, low-code IoT platform that requires no programming skills and minimal time to create custom IoT applications that can be brought into a white label IoT solution at the push of a button.

In practice, Datacake lets you take that IoT sensor data you've been accumulating, and turn it into engaging data dashboards.

![example mobile and desktop datacake dashboards](datacake-dashboard.png)

These dashboard reports are created in a straightforward point-and-click manner, with little coding required.

## What is the Blues Wireless Notecard?

The Notecard is a cellular and GPS device-to-cloud data-pump that comes with 500 MB of data and 10 years of pre-paid cellular service for $49. No activation charges, no monthly fees.

![blues wireless notecard](notecard.png)

The Notecard itself is a 30mm x 34mm SoM and is ready to embed in a project via its m.2 connector. But to make things easier, the Notecard can utilize a host [Notecarrier expansion board](https://blues.io/products/#notecarrier) for quick prototyping with **virtually any MCU or single-board computer**.

The Notecard speaks JSON. It's every developer's dream, whether you program regularly in Python, Arduino, C, C++ or any other language that can read and write JSON!

## Route Data from Notecard to Datacake

The path of data from the Notecard to your cloud application takes a route like:

1. Your MCU or SBC **gathers** sensor data;
2. The Notecard **delivers** it to [Notehub.io](https://blues.io/services/);
3. Notehub.io **securely relays** the data to your cloud provider (e.g. Datacake).

![notecard to notehub diagram](notecard-notehub-diagram.png)

"Secure" is a key word for the Notecard. With an integrated STSAFE Secure Element with hardware crypto, true hardware random number generator, and a factory-installed ECC P-384 certificate provisioned at chip manufacture, the Notecard takes security to heart.

In addition, all transactions between the Notecard and Notehub.io utilize encrypted "off the Internet" communication protocols with VPN tunneling from cellular provider direct to cloud.

If you'd like to learn more about this Notecard + Datacake integration, take a look at the [complete Datacake routing guide](https://dev.blues.io/build/tutorials/route-tutorial/datacake/) on the Blues Wireless Developer Portal.

## Watch and Learn!

Coming up on ????, folks from both Blues Wireless and Datacake are presenting "xxx". This will be a 45-ish minute webinar covering:

- asdf
- asdf
- asdf

Be sure to grab your seat early as attendance will be limited. Hope to see you there!