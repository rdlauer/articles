# Visualizing the IoT with Datacake and the Blues Wireless Notecard

*Datacake is a low-code IoT platform that easily consumes data from Notehub.io. Learn more about securely delivering Notecard data to Datacake.*

Last week, Blues Wireless and [Datacake](https://datacake.co/low-code-iot-platform) co-presented **How to Create IoT Asset Tracking Applications with Drag-and-Drop Tooling**. This webinar covered an end-to-end technical demonstration of:

1. Actively **tracking assets** with GPS and cellular, with the [Blues Wireless Notecard](https://blues.io/products/).
2. **Securely delivering** tracking data to the cloud.
3. Building a robust **cloud-based reporting application** with Datacake.

*If you missed it, you can still watch the full recording on YouTube:*

https://youtu.be/1omCtRWjcHg

## What is Datacake?

The Datacake website does a fantastic job of describing what the platform is, at a high level:

> Datacake is a multi-purpose, low-code IoT platform that requires no programming skills and minimal time to create custom IoT applications that can be brought into a white label IoT solution at the push of a button.

In practice, Datacake lets you take that IoT data you've been accumulating, and turn it into engaging data dashboards and reporting solutions.

![blues wireless notecard and datacake](notecard-datacake.png)

These dashboard reports are created in a straightforward point-and-click manner, with little coding required.

## Routing Cellular Data to Datacake

Routing data from the Notecard to a cloud application like Datacake follows a specific path:

1. Your MCU or SBC **gathers** sensor and/or location data;
2. The Notecard **delivers** it to [Notehub.io](https://blues.io/services/);
3. Notehub.io **securely routes** the data to your cloud provider (e.g. Datacake, AWS, Azure, etc).

![notecard to notehub diagram](notecard-notehub-diagram.png)

"Secure" is a key word for the Notecard. With an integrated [STSAFE Secure Element](https://www.st.com/en/secure-mcus/stsafe-a100.html) with hardware crypto, true hardware random number generator, and a factory-installed ECC P-384 certificate provisioned at chip manufacture, the Notecard takes security to heart.

In addition, all transactions between the Notecard and Notehub.io utilize encrypted "off the Internet" communication protocols with VPN tunneling from cellular provider direct to cloud.

## Learn More

If you'd like to learn more about this Notecard + Datacake integration, take a look at the [complete Datacake routing guide](https://dev.blues.io/build/tutorials/route-tutorial/datacake/) on the Blues Wireless Developer Portal.