# Sending and Receiving Binary Files with the Notecard and Notehub

*Learn how to use the card.binary APIs to send and receive files from a Micro SD card.*

The Notecard was originally designed as a low-bandwidth and low-power device that effortlessly syncs arbitrary JSON payloads with a cloud endpoint (e.g. key/value pairs of strings, booleans, and integers). However, one of the most-repeated requests we heard from customers is the ability to _also_ sync binary data (usually in the form of images).

With the release of the [Notecard developer firmware release v5.3.1](https://dev.blues.io/notecard/notecard-firmware-updates/#v5-3-1-september-18th-2023), this is now possible and easy to implement by using the new [card.binary APIs](https://dev.blues.io/api-reference/notecard-api/card-requests/#card-binary).

> Better yet use one of our supported firmware libraries, [note-arduino](https://dev.blues.io/tools-and-sdks/firmware-libraries/arduino-library/) or [note-python](https://dev.blues.io/tools-and-sdks/firmware-libraries/python-library/), that abstract away the complexity of the `card.binary` requests.

In this article, I'm going to show an example in Arduino/C of how to:

1. Request a JPEG image from a cloud endpoint.
2. Write that JPEG image to a Micro SD card.
3. Send that same JPEG image back to a different cloud endpoint.

## Hardware Setup

For this example, I'll be using the following minimally-required hardware:

- Blues Swan (STM32-based host MCU)
- STLINK Programmer/Debugger (optional, but makes working with STM32-based MCUs much more pleasant)
- Blues Cellular Notecard
- Blues Notecarrier A (a development/carrier board for interfacing with the Notecard)
- [Adafruit's Micro SD breakout board](https://www.adafruit.com/product/254)

| SD Breakout | Blues Swan |
| ----------- | ---------- |
| GND | GND |
| 3v | 3V3 |
| CLK | CK |
| DO | MI |
| DI | MO |
| CS | A3 |

Next, attach the Notecarrier A to the Swan via a Qwiic cable. Note that the Notecarrier A also needs to be powered separately either over Micro USB or an attached LiPo battery.

Your project should look like the following (with or without the STLINK debugger of course):

IMAGE

## Format the SD Card

If you haven't already, be sure the SD card you're using is already formatted as either FAT16 (if <= 2GB) or FAT32. If not, you can use a USB SD card reader with your PC to format it using the SD Association's Memory Card Formatter (available on [macOS/Win](https://www.sdcard.org/downloads/formatter/) and [Linux](https://www.sdcard.org/downloads/sd-memory-card-formatter-for-linux/)).

Now it's time to write the sketch!

BEGIN NOTE

Looking for a shortcut? Check out this gist to see the completed sketch.

END NOTE

## Setup the Notecard and SD Card Breakout

Include the necessary libraries in your project, including [note-arduino] for the Notecard and the SD??? for the breakout board. Also, add the variables needed to interface with the components:

```c
CODE
```

Initialize the Notecard and link your Notecard to your project in Notehub.

```c
  notecard.begin();

  J *req = notecard.newRequest("hub.set");
  JAddStringToObject(req, "product", "<your-product-uid>");
  JAddStringToObject(req, "mode", "continuous");
  JAddBoolToObject(req, "sync", true);
  notecard.sendRequestWithRetry(req, 5);

  // wait until we are connected to notehub
  bool connected = false;
  while (!connected)
  {
    req = notecard.newRequest("hub.status");
    J *rsp = notecard.requestAndResponse(req);
    connected = JGetBool(rsp, "connected");
    delay(2000);
  }
```

Next, reset the Notecard's binary storage area, so we can start fresh
  
```c
  NoteBinaryStoreReset();

  // init the SD card for usage
  stlinkSerial.println("Initializing SD card...");

  if (!card.init(SPI_HALF_SPEED, chipSelect))
  {
    stlinkSerial.println("SD card initialization failed!");
    while (1)
      ;
  }

  // try to open the 'volume'/'partition' - it should be FAT16 or FAT32
  if (!volume.init(card))
  {
    stlinkSerial.println("Could not find FAT16/FAT32 partition.\nMake sure you've formatted the card");
    while (1)
      ;
  }
```

## Request a JPEG File from the Cloud

```c
  if (J *req = NoteNewRequest("web.get"))
  {
    JAddStringToObject(req, "route", "GetImage");
    JAddBoolToObject(req, "binary", true);
    NoteRequest(req)
    ...
```

asdfasdf

```c
 uint32_t data_len = 0;
    NoteBinaryStoreDecodedLength(&data_len);

    // Create a buffer to receive the entire data store. This buffer must be
    // large enough to hold the encoded data that will be transferred from
    // the Notecard, as well as the terminating newline.
    // `NoteBinaryMaxEncodedLength()` will compute the worst-case size of
    // the encoded length plus the byte required for the newline terminator.
    uint32_t rx_buffer_len = NoteBinaryCodecMaxEncodedLength(data_len);
    uint8_t *rx_buffer = (uint8_t *)malloc(rx_buffer_len);

    // Receive the data
    NoteBinaryStoreReceive(reinterpret_cast<uint8_t *>(rx_buffer), rx_buffer_len, 0, data_len);
    notecard.logDebugf("\n[INFO] Received %d bytes.\n", data_len);


```

## Save the JPEG File to a Micro SD Card

```c
    // save the buffer to the specified file name on the SD card
    SD.begin(chipSelect);
    SD.remove("logo.jpg"); // remove the file if it already exists
    myImageFile = SD.open("logo.jpg", FILE_WRITE);

    if (myImageFile)
    {
      myImageFile.write(rx_buffer, data_len);
      myImageFile.close();
      stlinkSerial.println("Completed writing the file!\n");
    }
```

## Send the JPEG File to a Different Cloud Endpoint

Routing is awesome.

### Routing Data with Notehub


### Back to the Sketch

```c
    const uint32_t notecard_binary_area_offset = 0;
    NoteBinaryStoreTransmit(reinterpret_cast<uint8_t *>(rx_buffer), data_len, sizeof(rx_buffer), notecard_binary_area_offset);
    notecard.logDebugf("\n[INFO] Transmitted %d bytes.\n", data_len);

    // Send the binary data to Notehub
    {
      // Submit binary object to the Notehub using `note.add`. This will send
      // the binary to Notehub in the `payload` field of the Note. The payload
      // will not be visible in the Notehub UI, but the data will be forwarded
      // to any pre-configured routes.
      if (J *req = notecard.newRequest("note.add"))
      {
        JAddStringToObject(req, "file", "binary.qo");
        JAddBoolToObject(req, "binary", true);
        JAddBoolToObject(req, "live", true);
        if (!notecard.sendRequest(req))
        {
          // The binary data store is cleared on successful transmission,
          // but we need to reset it manually if the request failed.
          NoteBinaryStoreReset();
        }
      }
    }
```


## Next Steps

larger files? chunks

consult gist...consult note-arduino examples

starter kit



```json
{
  "event": "534330df-41d4-42c0-bbc9-c39909b9df08",
  "session": "4ab215ae-3126-4321-8584-e6bc216d9ec5",
  "best_id": "dev:864049051550715",
  "device": "dev:864049051550715",
  "product": "product:com.blues.rlauer:aaa",
  "app": "app:9a442885-d04d-4f5d-bca5-94eb07f7a27f",
  "received": 1698867979.948566,
  "req": "note.add",
  "when": 1698867978,
  "file": "binary.qo",
  "body": {},
  "payload": "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",
  "best_location_type": "tower",
  "best_location_when": 1698866411,
  "best_lat": 43.0742625,
  "best_lon": -89.442609375,
  "best_location": "Shorewood Hills WI",
  "best_country": "US",
  "best_timezone": "America/Chicago",
  "tower_when": 1698866411,
  "tower_lat": 43.0742625,
  "tower_lon": -89.442609375,
  "tower_country": "US",
  "tower_location": "Shorewood Hills WI",
  "tower_timezone": "America/Chicago",
  "tower_id": "310,410,17169,77315594",
  "fleets": [
    "fleet:4d7ccbcb-a410-4730-9ee2-b483b3523b17"
  ]
}
```