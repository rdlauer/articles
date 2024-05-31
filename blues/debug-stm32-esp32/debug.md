# Debugging on STM32 and ESP32 with VSCode and PlatformIO

I don't know about you, but the code I write is always perfect. It's bug-free, I 
understand exactly what's happening in every 3rd party library in use, and there 
is never a point where I need to see what's going on under the covers - It Just 
Works™.

Back in reality, it's a minor miracle if I write more than a couple lines of 
code without some flaw. Maybe it's a simple syntax mistake or maybe the logic 
I'm coding fundamentally alters the functional intent of my product. Either way, 
**I need a decent debugger to make me even remotely efficient**.

As someone who grew up in the web/mobile/cloud world, I'm used to using robust 
debuggers in tools like Visual Studio alongside the inspectors available in 
virtually every web browser today. Unfortunately the embedded space still lags 
behind when it comes to ease of both iterative development and debugging of code 
in realtime, on device.

I hope this article provides a little bit of guidance for those of you who, like 
me, certainly _try_ to write bug-free code, but need a little help. 
Specifically, we are going to look at debugging code on both the **STM32 and 
ESP32 architectures** using [Visual Studio Code](https://code.visualstudio.com/) 
and [PlatformIO](https://platformio.org/).

Let's dig in and squash some bugs.

## Debugging on STM32

Step 0: You'll need an in-circuit debugger and programmer for the STM32. Full 
stop.

Using a debugger/programmer has some key advantages like uploading compiled 
firmware without contorting your fingers to press tiny buttons and forcing the 
board into its bootloader. In the context of debugging, it's also the only way 
to properly debug and inspect code while it's running on the device.

The [STLINK](https://www.st.com/en/development-tools/stlink-v3minie.html) line 
from STMicroelectronics and 
[J-Link](https://www.segger.com/products/debug-probes/j-link/) from Segger are 
two popular types of debuggers that work with STM32-based boards.

In this section I'm going to document how to debug with **STLINK-V3MINI**, but 
your experience with other similar programmer/debuggers shouldn't differ too 
much.

### Connecting an STLINK

Depending on the host board you are using, you have a few options when it comes 
to physically connecting your STLINK:

1. Connect STLINK via a 4-pin SWD header.
1. Connect STLINK via a 20-pin JTAG header.
1. Manually wire STLINK via GPIO pins.

<Note>

If you're using a Nucleo or Discover board, these come with an on-board STLINK 
debugger, so using an external STLINK is not necessary!

</Note>

For example, the [Blues Swan](https://shop.blues.com/collections/swan/products/swan) includes a 20-pin JTAG header, so the STLINK-V3MINI slots easily onto the board:

IMAGE

If wiring directly to the GPIO pins from the STLINK, be sure to follow this wiring guide:

| Pin function | Debugger pin     | Target pin    |
|--------------|------------------|---------------|
| Ground pin   | GND              | Any GND pin   |
| +3.3V pin    | VCC / VDD / 3.3V | Any +3.3V pin |
| Clock pin    | SWCLK / SWCK     | PA14          |
| Data pin     | SWDIO            | PA13          |

<Note>

A full set of guides for connecting your STLINK is 
[available here](https://stm32-base.org/guides/connecting-your-debugger.html). 
Segger provides 
[J-Link connection guides](https://www.segger.com/products/debug-probes/j-link/technology/interface-description/) 
as well.

</Note>

Since the STLINK cannot power your host STM32 host directly, you'll need to 
provide power to the board (either via a LiPo battery or by using another USB 
cable).

IMAGE

### Debugging with STLINK-V3MINI

Let's look more closely at how to set breakpoints and use the "step out", "step 
over", and "step into" commands to debug firmware running on an STM32 host.

1. Update your `platformio.ino` file to add the specific debugging tool you're 
   using:

   ```
   debug_tool = stlink
   ```

1. While you're at it, update that same file to include the proper 
   `upload_protocol` which dictates how firmware is uploaded to the device:

   ```
   upload_protocol = stlink
   ```

1. And...that's actually all you have to do to enable debugging in PlatformIO!

1. Next, you can set one or more breakpoints on any line of code by clicking in 
   the small column to the left of any line number. This will add a little red 
   dot (the breakpoint) which will halt execution of the program and allow you 
   to manually debug and inspect relevant variables and properties.

IMAGE

1. With your breakpoint(s) set, it's time to build and deploy to your STM32 
   device. Navigate to the "Run and Debug" tab in VS Code and press `F5` or hit 
   the "Start Debugging" button to build and deploy to your device:

IMAGE

1. Code will be executed and advance line-by-line until a breakpoint is hit. At 
   that time, you'll be allowed to inspect variables, memory allocations, and 
   either step over, into, or out of the current state.

IMAGE

1. One of the more useful features of debugging in VS Code is adding a variable 
   to "watch", which lets you see the value of any specified variable when a 
   breakpoint is hit.
   
   You can even edit variables and see how they impact your device in realtime!

IMAGE

## Debugging on ESP32

Just like when debugging on an STM32, when working with an ESP32-based board 
your journey begins with a debugger/programmer, which is most commonly the 
[ESP-Prog](https://docs.espressif.com/projects/espressif-esp-iot-solution/en/latest/hw-reference/ESP-Prog_guide.html).

IMAGE

You can also use FT2232HL- and FT232H-based debuggers or the aforementioned 
Segger J-Link. Certain ESP32-based dev boards like the ESP-WROVER-KIT ship with 
an integrated debugger.

### Connecting an ESP-PROG

Most ESP-Prog boards ship with multiple ribbon cables that let you connect 
directly to your board, provided a port has been exposed. That's not the norm 
though.

In my case, I'm using Adafruit's HUZZAH32 Feather and therefore have to manually 
wire my host to the ESP-Prog. No worries though, as the wiring required is 
minimal:

| JTAG | ESP32  |
|------|--------|
| GND  | GND    |
| TDO  | GPIO15 |
| TDI  | GPIO12 |
| TCK  | GPIO13 |
| TMS  | GPIO14 |

When all is said and done, the ESP32-Prog is connected via a Micro USB cable, 
properly wired to my Feather, and the Feather is also connected to my computer 
via Micro USB. A bit messy, but it works!

IMAGE

### Debugging with ESP32-Prog

1. Add the following lines to your `platformio.ini` file to specify the 
   debugging tool you're using. Use of `tbreak setup` is useful if you need the 
   debugger to halt anywhere in `setup()`.

   ```
   debug_tool = esp-prog
   debug_init_break = tbreak setup
   ```

1. And that's the only configuration required in PlatformIO!

1. **Using Windows?** You may also have to install Zadig to add the "WinUSB" 
   driver. See 
   [this Hackster tutorial](https://www.hackster.io/brian-lough/use-the-platformio-debugger-on-the-esp32-using-an-esp-prog-f633b6) 
   for more information. Don't worry, it's not as bad as it sounds!

1. Next, just like with STM32, you can set one or more breakpoints on any line 
   of code by clicking in the column to the left of a line number.

IMAGE

1. With your breakpoints(s) set, build and deploy to your ESP32 device by 
   navigating to the "Run and Debug" tab in VS Code and pressing `F5` (or use 
   the "Start Debugging" button).

IMAGE

<Warning>

Since the ESP32 uses GPIO 13 to manage the onboard LED, and ESP-Prog also uses GPIO 13 for `TCK`, you won't be able to use the onboard LED when debugging.

Maybe this will save you the hours it took me to figure that out!

</Warning>

## Wrapping Up

Hopefully this article has helped you take that first step to properly debug on 
STM32 and ESP32 boards with PlatformIO and VS Code. While your mileage may vary 
when diving into the more obscure ST and Espressif configurations, I had luck 
with the following hardware:

### STM32

- [Blues Swan (STM32L4)](https://shop.blues.com/collections/swan/products/swan)
- [STLINKV3-MINI](https://shop.blues.com/collections/accessories/products/stlink-v3mini)

### ESP32

- [Adafruit HUZZAH32 Feather](https://learn.adafruit.com/adafruit-huzzah32-esp32-feather)
- [ESP-Prog](https://www.sparkfun.com/products/19099)

Happy debugging! 🐛
