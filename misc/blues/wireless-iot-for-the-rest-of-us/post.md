# Wireless IoT for the Rest of Us

When predicting the next 10 years of growth segments in IT, the famed technology analysts of our time (Gartner, Forrester, IDC, and the like) seem to strike out as often as they make contact. I get it, reading the tea leaves of tech trends is not for the feint of heart.

However, one artifact of Gartner I'm a huge fan of is their [Hype Cycle](https://www.gartner.com/en/research/methodologies/gartner-hype-cycle). Gartner takes us on an annual roller coaster of technology niches as they advance through early life stages:

IMAGE?

An example of this in practice is the 2020 Hype Cycle for the Internet of Things (IoT):

IMAGE

This chart gets into some of the nuanced segments of IoT, but if you blur your eyes a bit, you can let yourself see the maturation of IoT as a whole. The "Internet of Things" itself is nestled in the *Trough of Disillusionment*, poised to break out in the next 2-5 years.

So how exactly do we shake off the haters and advance IoT into the *Plateau of Productivity*?

## The Three Pillars of IoT for Developer Success

My current thoughts on the mainstream developer success of IoT is focused on three pillars:

1) Cellular Connectivity

I want my devices to be able to communicate everywhere, anywhere, all the time, and at a reasonable price. As a hobbyist I can't stomach paying yet another monthly cellular bill. As a large organization we don't want unpredictable pricing, nor do we want to pay for data we don't utilize.

2) Familiar Developer Tooling

As a modern developer, I'm having to learn new languages/frameworks/libraries at a blinding pace. In a perfect world, I could re-use existing constructs and focusing on building.

3) Cloud ???

I'll likely be pulling a lot of data out of my devices when they are deployed. Instead of writing yet another new API to handle this data, I need my data automagically piped to my cloud of choice. Whether it's AWS, Azure, or GCP, I shouldn't have to re-learn something.

## The Future is Wireless

Bold statement huh? Yes, we've been walking around with wireless computers in our pockets for years. WiFi is ubiquitous. We work untethered on a daily basis.

The IoT devices we utilize have similarly been communicating via the same mechanisms:

1. Hard-wired ethernet
2. WiFi
3. Bluetooth
4. Cellular

So if the "future is wireless", let's focus on the three of these that drop the wire. One of them is notoriously unreliable with limited range (Bluetooth), another is not available in the field (WiFi), and the third (cellular) is expensive at scale with monthly service commitments even at 2G speeds.

So it's safe to say that the future of IoT relies on cellular. But more than that I believe the other two pillars of IoT adoption and success are familiar tooling/languages and cloud integrations.

## The Future is (Blues) Wireless

Which is why I find the offerings from Blues Wireless so intriguing for three reasons:

1. Cost
2. Tools
3. Cloud

Blues Wireless positions themselves as "the fastest path to build and deploy cellular connected products". In my time tinkering with their hardware and services, I can wholeheartedly say I think they are on to something.

Here are my quick takes on how Blues is tackling some key blockers in the IoT industry:

### Cost

If cellular connectivity is critical, someone has to solve the unpredictable pricing problem. If I invest in 1000 MCUs and am paying $10/month for 2G access plus data fees(!), my costs are unpredictable and unreliable. Even if I'm a hobbyist IoT tinkerer, I'm not likely to get sucked into yet another cellular service plan.

Blues Wireless appears to crack this nut with a straightfoward $49 one-time fee that includes a Notecard (their MCU with on-board GPS and cellular) plus 10 years (yes, YEARS) of cellular service with 500MB of data included.

TEN YEARS GIF

### Tools/Languages?

The Blues Wireless Notecard speaks your language. Tired of writing archaic AT commands? The Notecard API is driven wholely by JSON.

IMAGE

Blues also provides firmware libraries for your language of choice:

- Arduino
- Python
- C/C++
- Go

### Cloud Connectivity



The Blues Wireless Notehub is a secure data pipe for your IoT data and devices. Configure and manage fleets and firmware, and route data to any 3rd party cloud application.