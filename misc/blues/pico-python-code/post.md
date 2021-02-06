# Your First Steps with Raspberry Pi Pico and Visual Studio Code

The Raspberry Pi Foundation recently released their first microcontroller board, the Raspberry Pi Pico. This $4 (not a typo!) device is not only a low-cost entry into the Raspberry Pi ecosystem, it's also surprisingly useful for embedded IoT development.

While Raspberry Pis are best known as single board computers (e.g. the Raspberry Pi 1/2/3/4 models) for playing games, browsing the web, and such, the Pico was designed for use in a variety of physical computing solutions like controlling motors, reading sensors, and even [machine learning](https://github.com/raspberrypi/pico-tflmicro). As with other Raspberry Pi hardware, it's developer-friendly and can be programmed with C/C++ and MicroPython (a Python derivative).

Let's take a look at how we can go from unboxing our Pico to becoming productive IoT developers by utilizing an established language (MicroPython) and a universally-adored IDE (Visual Studio Code).

## Why MicroPython?

As just mentioned, you can develop on the Pico MCU using either C/C++ or MicroPython. While I fully admire all of you C developers out there (my tiny brain has never grokked it), I also happen to appreciate higher level languages like Python for their ease of use and developer experience. From the MicroPython docs:

> MicroPython is a lean and efficient implementation of the Python 3 programming language that includes a small subset of the Python standard library and is optimised to run on microcontrollers and in constrained environments.

And recall, we are working on a $4 MCU with minimal RAM and storage. MicroPython, being a trimmed down derivative of Python, IMO is a nearly perfect solution for programming your Pico. The Pico port of MicroPython also includes additional modules for accessing some of the Pico-specific hardware.

## Why Visual Studio Code?

Sorry, we will get to the meat of the content soon, trust me. If you have previous experience working with Raspberry Pi hardware and MicroPython, your default IDE is probably Thonny.

IMAGE

Don't get me wrong, Thonny is a super fun little IDE to work with. The problem for me is that it's not very robust. It doesn't provide the expansive access to extensions that I need to work with Python. And it's just not part of my default workflow. I'm a big fan of meeting developers where they already are (via popular languages AND tooling), so leveraging the uber-popular Visual Studio Code for developing on my Pico is where I'm going to start.

## Set Up Your Pico for MicroPython

The first thing you need to know is you program your Pico by connecting it to your computer and then simply dragging-and-dropping a file onto it. Pretty magical.

Thankfully the folks at Raspberry Pi have done the heavily lifting to let us install MicroPython easily.

Navigate to the [Raspberry Pi Pico docs](https://www.raspberrypi.org/documentation/pico/getting-started/) and click on the "Getting started with MicroPython" tab:

IMAGE 

Follow the instructions provided under "Drag and drop MicroPython" (summarized here):

1. Download the MicroPython UF2 file.
2. Push and hold the BOOTSEL button and plug your Pico into your computer.
3. Browse available drives and look for "RPI-RP2".
4. Drag-and-drop the downloaded UF2 file onto RPI-RP2.
5. Your Pico will automatically reboot...and now you are running MicroPython!

## Visual Studio Code...Meet Pico

The beauty of VS Code is its massive extension marketplace. A fine example of this is the [Pico-Go extension](https://marketplace.visualstudio.com/items?itemName=ChrisWood.pico-go) that we will install and use to connect to our Pico from VS Code.

If you've never installed a VS Code extension before (you're missing out!) just head to the Extensions tab and search for `Pico-Go`:

IMAGE

Provided your Pico is still plugged in, the extension will automatically discover any boards attached. You should see the following in the built-in terminal window:

	Searching for boards on serial devices...
	Connecting to COM3...
	
	>>>

The `>>>` prompt tells you that you're in the MicroPython REPL and ready to execute some Python! To test this out, enter the simplest of Python commands:

	>>> print("Hello Pico")
	Hello Pico

GIF?

Now that we know we can communicate with the Pico, let's take the next baby step and actually interact with the device hardware.

## LED Goes BLINK

The only hardware on the Pico that we can visually interact with is a tiny LED. So let's write a simple program that turns the LED on and off.

In VS Code, create a new project directory and a `main.py` file in that directory. In `main.py` we can start by adding this line:

	from machine import Pin

The `machine` module is used to control your on-chip hardware. Next, let's set an `led` variable to the GPIO pin 25, where our LED is connected:

	led = Pin(25, Pin.OUT)

Finally, to turn the LED on:

	led.value(1)

Save `main.py` and find the "arrow Upload" command at the bottom of your VS Code window. This will transfer your current project to the Pico and run `main.py`. You can also use the "> Run" command to run the code without physically transferring the file.

Your Pico's LED should now be in a static "on" position:

IMAGE

If you want to get fancier, you can `toggle` the LED to blink on-and-off by adding this code:

	import utime
	
	while True:
		led.toggle()
		utime.sleep_ms(30)

## Adding an External Input and Output

Let's take our Pico journey up a notch by integrating it with some external devices. In this section, we will turn an external LED on and off using a push-button.

> Not exactly setting the world on fire here, but it's a good next step for those of us just learning the Pico and Micropython!

To continue along, you'll need a few additional pieces of hardware:

1. Breadboard
2. Jumper wires
3. LED
4. 330ohm resistor
5. Push-button switch

Our finished circuit should look something like this:

IMAGE

When understanding why the components are laid out the way they are, it's always helpful to consult the Raspberry Pi Pico pin layout:

IMAGE

Our circuit is completed by connecting:

1. `GND38` to negative side of the power rail.
2. `3V3` (power) to positive side of the power rail.
3. `GP15` to the anode (longer) side of LED via the 330ohm resistor.
4. `GP14` to read the press-button.
5. Negative power rail to cathode (shorder) side of LED.
6. Positive power rail to power the press-button (make sure it's on the diagonal side from the other connection).

Create a new file in VS Code and, at the top, import the same two modules we did in the previous section (`machine`, and `utime`):

	from machine import Pin
	import utime

Create references to our press-button `button` and external LED `led` by referencing the pin locations used above:

button = Pin(14, Pin.IN)
led = Pin(15, Pin.OUT)

Finally, create a never-ending loop that will match the led value (remember 1 = on and 0 = off) to the value of whether or not the button is pressed (the same 1 = pressed and 0 = not pressed). Finally, add a short sleep between iterations to provide a more realistic experience when pressing/releasing the button:

	while True:
	    led.value(button.value())
	    print(button.value())
	    utime.sleep(0.1)

And that's it! Now when you hold the button down, the LED should light up.

## Adding a Thread

This is cool and all, but let's look at how we can take advantage of a Python feature known as threading. By default, our Python programs are single-threaded, meaning they run in sequential order from top to bottom. What if we wanted to execute another task while doing something else?

For example, what if we wanted to press the button once to turn the LED off or on, instead of having to actively hold the button down?

To do this, we will use threading.





from machine import Pin
import utime
import _thread

button = Pin(14, Pin.IN)
led = Pin(15, Pin.OUT)

global button_pressed
button_pressed = False

def read_button():
    global button_pressed
    while True:
        if button.value() == 1:
            button_pressed = not button_pressed
            utime.sleep(0.5)

_thread.start_new_thread(read_button, ())

while True:
    if button_pressed == True:
        led.value(1)
    else:
        led.value(0)




https://www.tomshardware.com/how-to/raspberry-pi-pico-setup

https://www.tomshardware.com/how-to/raspberry-pi-pico-ultrasonic-sensor

https://www.raspberrypi.org/documentation/pico/getting-started/