# Is the Raspberry Pi a Practical Solution for Remote Monitoring?

I'm sure a good portion of you saw the latest [Explaining Computers](https://www.youtube.com/channel/UCbiGcwDWZjz05njNPrJU7jA) video comparing various Raspberry Pi models and how long they last on a 12V lead acid battery versus a USB battery pack. If not, it's worth a watch:

https://www.youtube.com/watch?v=lPyDtuzYE5s

This inspired me to think more about how useful a Raspberry Pi 4 Model B could *really* be in a remote, battery-powered setting. I mean, it's tempting, right? When developing on a single-board computer (SBC) you get access to the full Python language and all of its libraries. There is a file system that can handle whatever you throw at it (within reason). Virtually anything you want to run on a generic Linux distro can run on a Pi.

But...the full Raspberry Pi were never really meant to run off-grid. That's what the [Raspberry Pi Zero](https://www.raspberrypi.org/products/raspberry-pi-zero/) and [Raspberry Pi Pico](https://www.raspberrypi.org/products/raspberry-pi-pico/) are for after all.

IMAGE

However, there are scenarios where, whether it's out of convenience or necessity, using a Raspberry Pi to its fullest extent in the wilds is worth trying.

In this experimental project, I wanted to measure how long a [Raspberry Pi 4 Model B](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) would last on a USB battery pack in a real world setting as it gathers sensor data and relays it to the cloud.

I'll tackle this with:

- Raspberry Pi 4 Model B (of course)
- The largest (affordable) battery pack I could find (a 30,000 mAh beast)
- The [Blues Wireless Notecard](https://blues.io/) (for remote cellular and an onboard temp sensor)

## My Power Consumption Hypothesis

Let's start with the estimated power consumption of a Raspberry Pi 4 Model B. According to the [Raspberry Pi docs](https://www.raspberrypi.org/documentation/hardware/raspberrypi/power/README.md), I could expect about **600mA** of consumption from an "active" RPi. Obviously this is a very rough estimation considering everything an RPi could, or could not, be doing.

The ROMOSS 30,000mAh power bank fit the bill, as it also provides the required **pass-through charging** capability.

The only other piece of hardware is the Blues Wireless Notecard. Looking at the [Notecard datasheet](https://dev.blues.io/hardware/notecard-datasheet/note-wbex-500/#power-information), we see:

> The Notecard typically sits in an ~8µA idle mode waiting for a request from the host MCU, however the Notecard current draw increases to the ~250mA range when the modem is active.

Since it's difficult to predict precisely how often and for how long the Raspberry Pi will use the cellular capabilities, the Notecard datasheet additionally specifies:

> Although the Notecard typically draws very little current, this supply should be designed with a **150mA** budget allocated to the Notecard.

*Get out your calculators!*

600mA + 150mA = **750mA**.

Therefore...

30,000 mAh battery x 0.75 (assuming we will get about 75% of the advertised capacity) = 24,000 mAh.

22,500/750 = **30 hours**.

At this stage, I'm highly skeptical that the RPi + Notecard will actually draw that much current. I also think I might be underselling the battery pack.

Keep in mind, too, that I'm not power-optimizing anything on the Pi. See the end of this article for some tips and tricks!

## A Bit About the Notecard

I chose to use the Notecard (and its companion Notecarrier-Pi HAT host) as it's the easiest solution I know of to add network connectivity when Wi-Fi or wired Ethernet isn't available.

It also includes an onboard temperature sensor and **consumes a mere 8mA** when idle, making it a great option for battery-powered deployments.

IMAGE

The Notecard is...

The Notecarrier-Pi acts as a host HAT for the Notecard. It provides the interface between the Raspberry Pi and the Notecard. With pass-through headers, it fits right in with whatever other Pi HATs you are using.

The Notecard also has an associated cloud service called Notehub.io. Notehub acts as a secure conduit to take your data from device to cloud. You can view submitted events (a.k.a. "notes") in Notehub, or securely route them to your cloud provider of choice.

The beauty of the Notecard...security...etc

## Project Setup

Since I am accessorizing my Raspberry Pi with only the Notecard/Notecarrier-Pi HAT and a USB battery pack, the hardware setup is quite simple.

IMAGE

To make this as real-world as possible, I'm going to write a Python script that samples temperature data from the Notecard at five minute intervals.

At roughly the same five minute interval, the Notecard relay data to Notehub.io (but only if there is pending data to send). This *periodic* mode helps to reduce the battery draw that would exist with a continuous cellular connection.

The completed Python script is available here on GitHub. Let's walk through the important sections here:

### Initialize the Notecard


### Gather Sensor Data


### Send Data over Cellular

## Cloud Reporting

I decided to add a simple cloud-based dashboard to visualize this data I'm sending. One of the advantages of using Notehub is its built-in routing capabilities.

In this project, I decided to use Datacake as it's incredibly easy to securely deliver data to and create an engaging little dashboard report. After following the full routing tutorial on the Blues Wireless Developer Portal, I was able to create a chart showing the gathered temperature data over time.

IMAGE

## Drumroll Please!

Recall how I estimated that we'd see about 30 hours of run time? Well, with this remotely-functioning Raspberry Pi running off of a 30,000 mAh battery pack, I started receiving data in Notehub at DATE. The last evidence of life from the Raspberry Pi was at DATE.

This means I got ??? hours of run time out of the Raspberry Pi. Not bad!



## Power Optimization Tips

I specifically avoided performing any power optimization steps on the Raspberry Pi before I started this project. I wanted to provide the most straightforward deployment as possible.

However, there are actually some simple tweaks we could make to squeeze out a few extra hours of run time!

### asdf

What could I do better? There are a variety of minor power optimizations I could've made to my Raspberry Pi. This may have given me some extra minutes of time. I could also add a PiJuice HAT and attach a solar panel (which I did in a previous project) to extend the available time significantly on sunny days.

### Turn Off USB Controller

**Power Saved? Approximately 100mA.**

If you're running your Raspberry Pi in a headless configuration, it's likely you can get away without powering the onboard USB controller. Even if you aren't using a mouse or keyboard, they are still powered.

To disable the USB controller:

```
echo '1-1' |sudo tee /sys/bus/usb/drivers/usb/unbind
```

To re-enable the USB controller:

```
echo '1-1' |sudo tee /sys/bus/usb/drivers/usb/bind
```

### Disable HDMI Output

**Power Saved? Approximately 30mA.**

Again, on a headless Pi you likely don't need to hook up a monitor. If that's the case, you can also disable the HDMI output.

To disable HDMI:

```
sudo /opt/vc/bin/tvservice -o
```

To re-enable HDMI:

```
sudo /opt/vc/bin/tvservice -p
```

### Clock Down the CPU

**Power Saved? Varies.**

If you don't require the full Raspberry Pi CPU (which, granted, is likely overkill for many remote monitoring situations anyway), you can save a few mA by clocking down the CPU cores.

In the `/boot/config.txt` file you can change the following parameters and reboot:

```
arm_freq_min=250
core_freq_min=100
sdram_freq_min=150
over_voltage_min=0
```

### Disable Wi-Fi and Bluetooth

**Power Saved? Approximately 40mA.**

If your solution isn't using Wi-Fi or Bluetooth (like the one we put together here), you can likely disable them.

> IMPORTANT: If you disable HDMI, USB, and Wi-Fi at the same time you'll have trouble interfacing with your Pi!

To disable Wi-Fi and Bluetooth, open that `/boot/config.txt` file and add these parameters and reboot:

```
dtoverlay=disable-wifi
dtoverlay=disable-bt
```

To re-enable them, simply remove the parameters from the file and reboot.

### Disable LEDs

**Power Saved? Approximately 10mA.**

Now we are digging into the nitty gritty!

We can disable the on-board LEDs on the Pi by again editing the `/boot/config.txt` file and adding the following and rebooting:

```
dtparam=act_led_trigger=none
dtparam=act_led_activelow=on
```

### Add Solar

Maybe the most obvious tip of them all for remote deployments is to source some additional power from nature! In past projects I've used a 40W solar array to power a remote birding solution and a PiJuice HAT with solar for a crypto miner.

IMAGE?

## What's Your Remote RPi Use Case?

Hopefully this has inspired some of you to use your Raspberry Pi off-grid. Happy hacking!
