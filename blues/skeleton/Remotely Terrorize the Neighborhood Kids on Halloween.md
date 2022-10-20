# Remotely Terrorize the Neighborhood Kids on Halloween

Who doesn't love Halloween? The candy. The costumes. The decorations. Traumatizing small children with scary skeleton props. 🎃💀

GIF

In this project, I'll show you how to remotely trigger a jump-scare skeleton with a cellular IoT project, giving you the opportunity to be the most hated grown-up on your block!

## Starting at the End

Let's begin our journey with the end result. This stock jump-scare skeleton lets you to trigger the action via detected motion or a footpad. Unfortunately the motion-triggering is a little *too* aggressive, often ruining the opportunity for a real surprise when kids are inspecting _what's in the bag_.

IMAGE

Ideally we want to manually trigger the skeleton by replacing the footpad action with a relay switch, but _remotely_ from a distant location. Therefore, we need a way to send a signal from a host MCU to toggle said relay from any distance.

Since this IoT deployment will be outdoors, a reasonable connectivity option is cellular, specifically the prepaid cellular Notecard from Blues Wireless. To minimize cost, we will use a new Notecard feature to set the state of a Notecard AUX pin to toggle the relay switch, removing the need for a host MCU on the skeleton side of the equation.

So we will **push a button** that **sends an event to the cloud** with the Notecard, our **cloud app will then relay that event** to the Notecard attached to the skeleton, and **SURPRISE!**

![from button to skeleton](button-to-cloud.png)

## Configuring the Button Host

Using the STM32-based Blues Wireless Swan as our "button host" MCU, we can wire up a simple push button.

FRITZING/PIC

Our Arduino sketch starts out quite simple, verifying the push button is wired up properly by lighting the onboard LED of the Swan when the button is pressed:

```
const int buttonPin = 6; // the digital pin of the push button
int buttonState = 0;     // variable for reading button status

void setup()
{
  // initialize digital pin LED_BUILTIN as an output
  pinMode(LED_BUILTIN, OUTPUT);
  // initialize the push button pin as an input
  pinMode(buttonPin, INPUT);
}

void loop()
{
  // read the state of the push button value:
  buttonState = digitalRead(buttonPin);

  // check if the push button is pressed
  if (buttonState == HIGH)
  {
    // turn LED on
    digitalWrite(LED_BUILTIN, HIGH);
  }
  else
  {
    // turn LED off
    digitalWrite(LED_BUILTIN, LOW);
  }
}
```

## Button Host: Send an Event over Cellular

Next, we want to wire up a Blues Wireless Notecard and its carrier board (the Notecarrier-A) to our button host MCU over I2C with a Qwiic cable.

IMAGE

What's the Notecard you ask? The Notecard provides an easy path to low-power cellular IoT through a prepaid system-on-module (including 500MB of data and 10 years of global service) AND its paired cloud service, Notehub. The Notecard starts at $49. No monthly fees, unless you plan on routing more than 5,000 events per month (then pricing starts at $0.00075/event).

IMAGE

With the Notecard, gone are the days of archaic AT commands to manage your cellular modem. The Notecard is all JSON, all the time.

For example, want to access the onboard GPS module to get your Notecard's location? Just use the card.location API request like so:

```
CODE
```

Back to the project. We will start our code updates in the `setup()` method by associating the Notecard with a new project on Notehub (i.e. the `product` arg below):

```
J *req = NoteNewRequest("hub.set");
JAddStringToObject(req, "mode", "continuous");
JAddStringToObject(req, "product", "<your-unique-productuid>");
JAddBoolToObject(req, "sync", true);
notecard.sendRequest(req);
```

Where is the JSON? This code snippet uses the Notecard note-arduino library which produces the following JSON for the Notecard:

```
{
  "req": "hub.set",
  "mode": "continuous",
  "product": "<your-unique-productuid>",
  "sync": true
}
```

> **NOTE:** The completed sketch is available in this GitHub repository.

When the push button is pressed, we want to send an event (a.k.a. a Note) to our Notehub project. We can do this in the `loop()` method:

```
J *req = NoteNewRequest("note.add");
JAddStringToObject(req, "file", "surprise.qo");
JAddBoolToObject(req, "sync", true);
J *body = JCreateObject();
JAddBoolToObject(body, "jump", true);
JAddItemToObject(req, "body", body);
notecard.sendRequest(req);
```

Again, the output of this Arduino C code is a simple JSON object that is delivered automatically to the Notecard:

```
{
  "req": "note.add",
  "file": "surprise.qo",
  "body": {"jump": true},
  "sync": true
}
```

Now, when the button is pressed, a Note is sent to Notehub along with some additional metadata about the cell tower used, timestamps, and so on:

IMAGE

## Route to AWS Lambda with Notehub

This Note in Notehub doesn't really do us much good (yet), because we need to **send the event to our skeleton**.

To do so, we need to _route_ the Note we just created to an AWS Lambda function. This Lambda function will then use the Notehub API to notify the Notecard attached to the skeleton that it should jump.

### AWS Lambda Function

Let's start with the Lambda function itself, written in Python (because why not):

```
import requests
import time

def lambda_handler(event, context):
    url = 'https://api.notefile.net/v1/projects/<project-uid>/devices/<device-uid>/environment_variables'
    timestamp = int(time.time())
    payload = '{"environment_variables":{"_aux_gpio_set":",,low,,1000,' + str(timestamp) + ',60"}}'
    headers = {'content-type': 'application/json', 'Accept-Charset': 'UTF-8', 'X-SESSION-TOKEN': '<your-auth-token>'}
    r = requests.put(url, data=payload, headers=headers)
```

The `url` variable is the call to the Notehub API (using the unique ids of the Notehub project and the Skeleton Notecard we are sending the notification to).

The `payload` variable sets the `_aux_gpio_set` environment variable to, for example:

```
,,low,,1000,1665941585,60
```

While at first a bit odd, this tells the Notecard attached to the skeleton to pulse `AUX3` `LOW` for 1000 ms, valid from the UNIX epoch time of 1665941585 for 60 seconds. The empty spaces between the commas are where you could specify the states of the other AUX pins, 1, 2, or 4.

> Please consult the full AUX mode docs provided by Blues Wireless for additional details.

Confused? 🤔 Hold on to your questions because we will come right back to these environment variable things in a minute!

### The AWS Lambda Route in Notehub

So how exactly do we hit this AWS Lambda endpoint? Via a Notehub route, which allows us to send event data to any arbitrary endpoint.

> **NOTE:** Learn more about Notehub routes with these cloud-specific tutorials.

In the Notehub UI, we can create an AWS route that hits the Lambda function whenever a Note named `surprise.qo` is sent from our button host:

IMAGE

But we admittedly got a little ahead of ourselves, so let's take a quick look at Notecard environment variables.

## Environment Variables

Environment variables are key-value pairs that are shared between Notehub and Notecards.

They can be edited via the Notehub UI or the Notehub API (like we are doing today) and are useful for creating shared variables between a single device or a fleet of devices.

In our case, we are going to **set the state of a Notecard AUX pin without using an MCU** attached to the skeleton.

To summarize, our Lambda function sets the `_aux_gpio_set` environment variable on the Skeleton Notecard. The variable we send triggers a "pulse" by setting `AUX3` `LOW` for 60 seconds (toggling the relay switch). Once it's done, the Notecard will reset `AUX3` back to its original `HIGH` state.

GIF?

## Listen for the Event on the Skeleton Notecard

Since our skeleton doesn't have a host MCU, we need to configure its Notecard to automatically pull in that environment variable and be ready to set the state of its AUX pins.

To perform the one-time configuration of the Skeleton Notecard, we need to send two JSON commands directly to it. Using the Skeleton Notecard's carrier board, we connect it over Micro USB to our computer and use the in-browser serial terminal provided at dev.blues.io.

First, we want to configure the Skeleton Notecard to associate with the _same_ project as the Button Notecard with a `hub.set` request:

```
{"req":"hub.set","product":"<your-product-uid>","mode":"continuous","sync":true}
```

We then need to tell the Skeleton Notecard to listen for AUX GPIO changes with a `card.aux` request (in this case, set `AUX3` `HIGH` and listen for changes):

```
{"req":"card.aux","mode": "gpio","state": [{},{},{"high": true},{}]}
```

### Custom Relay Carrier

Last but certainly not least, we need to connect the previously-used FOOTPAD connector on the skeleton to a custom Notecarrier board that hosts the Skeleton Notecard.

IMAGE

This "relay carrier" allows you to control any normal outlet or high voltage product from anywhere. The carrier comes with two relays that are rated at a max of 220V@10Amp allowing you to control any electric appliance rated at under 2000 watts. The design is open source and available here on GitHub.

## The Scary Results

GIF?

To summarize, here are the steps we've taken to make this work:

1. We wired up a simple push button to our host MCU.
2. We wired a Notecard to our MCU and send an event/note to Notehub when the button is pressed.
3. Notehub routes that event to a Lambda function.
4. The Lambda function uses the Notehub API to set an environment variable on the Skeleton Notecard.
5. The Skeleton Notecard toggles the relay switch to elicit Halloween scares!

*Looking for some next steps?*

- Consult this GitHub repo for the full project code.
- Take 10% off your own Blues Wireless Starter Kit.
- Use the Notecard Simulator to try before you buy.

Happy Hack-o-ween! 🎃
