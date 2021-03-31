# IoT Network Connectivity - An Unapologetic Comparison of Low Power Technologies

# Use the Right Network Connection for Your IoT Solution

# The Evolution of IoT Network Connectivity

# IoT Network Connectivity - Comparing LoRa, Cellular, WiFi, and Other Relevant Technologies

Today's IoT developer is spoiled with a virtual cornucopia of network connectivity options. "Spoiled" is a positive take on the situation, as others might also classify it as "overwhelming". From modern cellular protocols like NB-IoT and CAT-M1, to WiFi and Ethernet, to LoRa and Bluetooth, the options are as plentiful as they are confusing.

What was once an obvious choice may now be more nuanced. For example, in an office setting, WiFi is often the default choice due to its low cost and broad availability. But what happens when the power goes out? If your IoT solution is part of a critical system like fire suppression, you may need a connectivity option external to the building.

Since every IoT application has its own unique requirements, let's step through every major connectivity option and provide guidance to help you determine which is right for you.

IMAGE: https://iot-analytics.com/state-of-the-iot-2020-12-billion-iot-connections-surpassing-non-iot-for-the-first-time/

## Key Considerations

When examining IoT network connectivity options, we have to take a critical look at the deployment scenarios and satisfy unique requirements such as:

1. Range and Physical Location
2. Data Throughput and Latency
3. Power Efficiency
4. High Availability
5. Device Cost
6. End User Experience

## Wired Ethernet

I'm going to go out on a limb and assume you don't need a technical explanation of wired Ethernet. It's the most established and most common LAN technology, and generally connects seamlessly to the Internet via some type of gateway.

The beloved backbone of our Internet is every IoT developer's connectivity dream in terms of bandwidth, speed, and reliability (if only it weren't for some caveats we will get to shortly!).

### When Does Wired Ethernet Win?

- Indoor deployments that provide easy access to a wired Ethernet option.
- High bandwidth and/or low latency solutions (e.g. streaming video).
- If RF interference is a consideration, cable shielding can withstand most interference.
- End user experience (if it's a plug-and-play scenario).

### When Does Wired Ethernet Lose?

- Is physical space an issue? Ethernet add-on boards can add considerable bulk.
- Wired Ethernet is usually not very mobile.
- Low-profile hardware solutions that can't fit the ethernet jack?
- Low-power installations (ethernet consumes)
- No-power scenarios (ethernet doesn't function without power)

## WiFi

Those of us old enough to remember the first WiFi routers can probably remember the first time we accessed the Internet in a wireless setting. WiFi has fundamentally altered how the average person connects to and interacts with the Internet, and the same applies to IoT implementations.

Today's WiFi routers commonly support at least one of the latest wireless standards (802.11n, 802.11ac, or 802.11ax).

### When Does WiFi Win?

- High bandwidth and/or low latency solutions (e.g. streaming video). with reliable signal

### When Does WiFi Lose?

- Non-technical end user experience. WiFi requires configuration that some users don't want to deal with.
- No-power scenarios, if power goes out, you lose wifi
- High power consumption
- Range issues (depending on WiFi standard)
- Remote deployments

### Summary

Standard WiFi (based on 802.11a/b/g/n/ac) is often not the best technology for IoT, but certain IoT applications can leverage installed standard WiFi, especially for in-building or campus environments. Obvious cases include building and home automation and in-house energy management, where installed WiFi can be leveraged as the communication channel and the devices can be connected to electric outlets.

WiFi HaLow, based on 802.11ah, is designed specifically for IoT, however it requires separate (as compared to standard WiFi) infrastructure and specialized clients.


## Cellular

Both cellular IoT (2G, 3G, 4G, and now also 5G) as well as LPWA (NB-IoT, LTE-M, Lora, Sigfox, and others) have been key drivers of the global IoT connectivity market in the past 5 years. The number of active cellular IoT and LPWA connections grew at an annual rate of 43% between 2010 and 2019 and is expected to grow at a rate of 27% going forward, according to IoT Analytics’ new Cellular IoT & LPWA connectivity market tracker.

LTE Cat M: The second generation of Cat.0, LTE Cat M is more effective and efficient than the first generation. It’s one of the most advanced connectivity types on the current LTE infrastructure, and offers some power saving modes ideal for long battery life.

Since many new IoT deployments will consider either LTE Cat M or NB-IoT connections, it’s worth pointing out that LTE Cat M supports a higher data rate and voice over the network, but it’s more expensive than NB-IoT.

NB-IoT: Designed specifically for IoT, NB-IoT (also known as narrowband IoT) is long range, consumes low power, and is extremely reliable. It operates on 4G networks but not on the LTE bands, making it immune to interference from other types of connectivity, and readily available in many countries. However, it does have some latency and doesn’t support real-time transmission like voice calls.

NB-IoT is the short form of Narrowband-Internet of Things. It is specified in LTE Release-13. It is also known as LTE cat-NB1

This is one of the fastest growing types of connectivity, with 685 million connections worldwide predicted by 2021.2

Two of the most popular modern cellular IoT technologies are LTE-M (Long-Term Evolution for Machines) and NB-IoT (Narrowband IoT). LTE-M is faster and is compatible with the existing LTE infrastructure. On the other hand, NB-IoT requires cellular providers to upgrade their hardware, which is why its rollout has been slower than LTE-M so far. NB-IoT also has a wider range than LTE-M, but requires the device to be stationary.

*LTE-M (also known as Cat-M1) is designed for low power applications requiring medium throughput. It has a narrower bandwidth of 1.4 MHz compared to 20 MHz for regular LTE, giving longer range, but less throughput. The throughput is 375 kbps downlink and 300 kbps uplink, providing approximately 100 kbps application throughput running IP. It is suitable for TCP/TLS end-to-end secure connections. Mobility is fully supported, using the same cell handover features as in regular LTE. It is currently possible to roam with LTE-M, meaning it is suitable for applications that will operate across multiple regions. The latency is in the millisecond range offering real time communication for time-critical applications.

LTE-M is perfect for medium throughput applications requiring low power, low latency and/or mobility, like asset tracking, wearables, medical, POS and home security applications.

LTE-M
NB-IoT
NB-IoT (also known as Cat-NB1) is a narrowband technology standard that does not use a traditional LTE physical layer, but is designed to operate in or around LTE bands and coexist with other LTE devices. It has a bandwidth of 200 kHz, giving it longer range and lower throughput compared to LTE-M and regular LTE. The throughput is 60 kbps downlink and 30 kbps uplink. It is suitable for static, low power applications requiring low throughput.

NB-IoT is perfect for static, low throughput applications requiring low power and long range, like smart metering, smart agriculture and smart city applications. It also provides better penetration in, for example, cellars and parking garages compared to LTE-M.
*


*The introduction of LTE-M and NB-IoT to 4G cellular networks is setting the standard for broad range IoT that will deploy at a massive scale in coming years. Designed for reliable, secure, low power operation these standards together with the ubiquitous cellular coverage in most of the world offer an unparalleled choice for Low Power Wide Area Network (LPWAN) IoT connectivity.*

### Wins

Cellular data is encrypted by default, while encryption must be enabled for Wi-Fi connections.
Location Detection
broad coverage
low power consumption

➨As it uses mobile wireless network it offers better scalability, quality of service and security compare to unlicensed LPWA networks such as LoRa/Sigfox.
➨It offers long battery life due to low power consumption or current consumption.
➨It offers extended coverage compare to GSM/GPRS systems.
➨Various network operators in Europe and Asia are supporting it.
➨It transmits data at low bit rates over long distances. The range is better than GSM and LTE.
➨NB-IoT modules are expected to be available at moderate costs.
➨They offer better penetration of structures and better data rates compare to unlicensed band based standards (e.g. LoRaWAN and Sigfox).
➨It co-exists with other legacy cellular systems such as GSM/GPRS/LTE. The NB-IoT compliant devices can be deployed/scheduled within any legacy LTE network. This helps them share capacity as well as other cell resources with the other wireless connected devices.

### Loses
latency, bandwidth

➨It offers lower data rate (about 250Kbps download and 20Kbps upload) compare to LTE cat-M1. The bandwidth is about 200 KHz. Hence it is ideal to use NB-IoT for stationary devices.
➨It does not support VoLTE (Voice Over LTE) for speech transmission. Hence voice transmission is not supported.
➨Roaming is not supported unlike LTE-M and Sigfox. (Expected Soon).


## LoRa

what are they...

Long Range Radio

The LoRa Alliance is an open, non-profit association formed to foster an ecosystem for certain LPWAN technologies. It has about 400 member companies throughout North America, Europe, Africa, and Asia, and its founding members include IBM, MicroChip, Cisco, Semtech, Bouygues Telecom, Singtel, KPN, Swisscom, Fastnet, and Belgacom.

LoRaWAN is the open-standard networking layer governed by the LoRa Alliance. However, it’s not truly open since the underlying chip to implement a full LoRaWAN stack is only available via Semtech. Basically, LoRa is the physical layer: the chip. LoRaWAN is the MAC layer: the software that’s put on the chip to enable networking. A more detailed yet simple introduction to LORAWAN can be found on Jensd’s I/O Buffer’s blog. 

It is low power wide area network which has become popular to due to low power consumption and long coverage range.

Consumer applications on LPWANs are still nascent, so they are not widely used. However, SigFox is gaining momentum in Europe, and IoT Central reports a 6% increase in adoption over the last year. Current applications are mostly industrial and are transitioning into “smart cities,” where utilities and services are finding more use cases.


### wins

Security: AES CCM (128 bit) encryption and authentication
wide outdoor coverage: up to 15-30 in rural areas without lots of interference
EU-based (good penetration)

➨It uses 868 MHz/ 915 MHz ISM bands which is available world wide.
➨It has very wide coverage range about 5 km in urban areas and 15 km in suburban areas.
➨It consumes less power and hence battery will last for longer duration.
➨Single LoRa Gateway device is designed to take care of 1000s of end devices or nodes.
➨It is easy to deploy due to its simple architecture as shown in the figure-1.
➨It uses Adaptive Data Rate technique to vary output data rate/Rf output of end devices. This helps in maximizing battery life as well as overall capacity of the LoRaWAN network. The data rate can be varied from 0.3 kbps to 27 Kbps for 125 KHz bandwidth.
➨It is widely used for M2M/IoT applications.
➨The physical layer uses robust CSS modulation. CSS stands for Chirp Spread Spectrum. It uses 6 SF (spreading factors) from SF 7 to 12. This delivers orthogonal transmissions at different data rates. Moreover it provides processing gain and hence transmitter output power can be reduced with same RF link budget and hence will increase battery life.
➨It uses LoRa moulation which has constant envelope modulation similar to FSK modulation type and hence available PA (power amplifier) stages having low cost and low power with high efficiency can be used.
➨LoRaWAN supports three different types of devices viz. class-A, class-B and class-C. Refer LoRaWAN classes>>.

### loses

low bandwidth (small data chunks)
US-based (low penetration)

➨It can be used for applications requiring low data rate i.e. upto about 27 Kbps.
➨LoRaWAN network size is limited based on parameter called as duty cycle. It is defined as percentage of time during which the channel can be occupied. This parameter arises from the regulation as key limiting factor for traffic served in the LoRaWAN network.
➨It is not ideal candidate to be used for real time applications requiring lower latency and bounded jitter requirements.

others: SigFox, Ingenu, and Weightless

## Bluetooth Low Energy

Bluetooth Low Energy (BLE) is...

Classic Bluetooth, used in mobile phone in-car hands-free systems, is renowned for its high power consumption.

### Wins

➨It offers very low power consumption and hence battery life can be very long. This is due to the fact that both master and slave devices can go to deep sleep mode between transactions. Master informs slave about hopping sequence and when to wake up.
➨It can be used for small size data transfer specially in IoT (Internet of Things) based applications. It offers maximum upto 255 bytes of message capacity.
➨It offers security using 128 bit AES algorithms. Hence encryption is applied to data being communicated between master and slave devices.
➨It uses frequency hopping using 2.4 GHz band, hence it can withstand against interference. Moreover advertising channels are chosen to be different than wifi channel frequencies to avoid interference between wifi and bluetooth devices.
➨The BLE devices are robust to operate in congested environment due to introduction of V5.0
➨It offers reliability and enables digital life.
➨BLE beacon devices have become popular due to speed and range increase in bluetooth 5.0 version.
➨Connection establishment procedure and data transfer is very fast (about 3 ms).
➨Devices from different manufacturers are compatible with the others.
➨There are endless devices using BLE and are inexpensive too.
Bluetooth LE technology, which forms part of the latest Bluetooth 4.1 specification from the Bluetooth Special Interest Group (SIG), is dramatically less power-hungry: it is claimed that typical slave nodes running Bluetooth LE can operate for months or even years on a single coin cell.

### Loses

- reliability
➨It can not be used for higher data rates as offered by wifi and cellular technologies. It supports 1 Mbps & 2 Mbps data rates.
➨It can not be used for long distance wireless communications unlike cellular and wifi devices. It supports upto 200 meters in LOS (Line of Sight).
➨It is open to interception and attack due to wireless transmission/reception.



## Summary

Choosing between Wi-Fi and cellular for your IoT project will depend largely on the priorities of your device and what it will be used for. Ultimately, the decision will come down to making the tradeoff between power consumption, bandwidth, coverage, and security.