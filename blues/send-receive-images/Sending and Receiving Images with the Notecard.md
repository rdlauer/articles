# Sending and Receiving Images with the Notecard and Notehub

*Learn how to use the card.binary APIs to send and receive images from a Micro SD card.*

The Notecard was originally designed as a low-bandwidth and low-power device that effortlessly syncs arbitrary JSON payloads (generally key/value pairs of strings, booleans, and integers) with the cloud. However, one of the most-repeated requests we hear from customers is the ability to _also_ sync binary data (usually in the form of images).

With the release of the [Notecard developer firmware release v5.3.1](https://dev.blues.io/notecard/notecard-firmware-updates/#v5-3-1-september-18th-2023), this is now possible AND relatively easy to implement by using the new [card.binary APIs](https://dev.blues.io/api-reference/notecard-api/card-requests/#card-binary)

> Better yet, use one of our supported firmware libraries [note-arduino](https://dev.blues.io/tools-and-sdks/firmware-libraries/arduino-library/) or [note-python](https://dev.blues.io/tools-and-sdks/firmware-libraries/python-library/) that abstract away the complexity of the `card.binary` requests.

In this article, I'm going to show an example in Arduino/C of how to:

1. Request a PNG image from a cloud endpoint.
2. Save that PNG image to a Micro SD card.
3. Read a PNG image from a Micro SD card.
4. Send a PNG image to a cloud endpoint.

## Hardware Setup

For this example, I'll be using the following hardware:

- Blues Swan (STM32-based host MCU)
- Blues Cellular Notecard
- Blues Notecarrier F (carrier board)
- SD Card Reader with a Micro SD card SparkFun Qwiic OpenLog https://www.sparkfun.com/products/15164

Since I'm using the Notecarrier F, no wiring is actually required:

1. The Swan MCU fits into the available Feather headers.
2. The Notecard slots into the open M.2 connector on the Notecarrier.
3. The SD card reader connects over I2C via the Qwiic connector.

IMAGE

## Notecard and SD Card Reader Config

> Looking for the completed code? Check out this GitHub repository.

hub.set etc in setup
sd card reader
test with a note.add

## Request a PNG File from the Cloud

The [PNGenc library](https://github.com/bitbank2/PNGenc) is the killer app of PNG image encoding/decoding on Arduino. This library requires only a minimal amount of RAM and has no other dependencies, making it an obvious choice when working with compressed images.

## Save the PNG File to a Micro SD Card

## Read a PNG File from a Micro SD Card

## Send the PNG File to the Cloud