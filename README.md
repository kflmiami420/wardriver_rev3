
# wardriver.uk Rev3

## Introduction

This repository is for the 3rd revision of the wardriver.uk created by [Joseph Hewitt](https://twitter.com/jhewitt_net). The wardriver.uk scans for nearby WiFi networks and Bluetooth devices and logs information about them to a CSV file which can be uploaded to [Wigle.net](https://wigle.net). A fully assembled Wardriver features 2 ESP32 modules, GPS, a SIM800L GSM module, an i2c LCD, a DS18B20 temperature sensor, and an SPI micro SD card reader/writer.

The main code is split up into 2 separate files (A and B) for the 2 different ESP32 boards within the Wardriver.

For information about the Wardriver hardware, please see [the official wiki](https://wardriver.uk)

## Repository Version

The `main` branch of this project is the testing/alpha branch; you should only use it if you are contributing code or want to help debug the latest features. Please use the `Releases` tab on the right to download stable versions.

## Flashing

The wardriver.uk uses 2 generic ESP32-DevKitC V4 development boards with an ESP32-WROOM-32U chip each. I recommend flashing your ESP32 boards with the Arduino IDE. In order for this to work, you will need to add the following URL to your "Additional Boards Manager URLs" list in the Arduino IDE:
```https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json```

You can edit this list by opening the Arduino IDE preferences with ```File -> Preferences```

You then need to open the Board Manager at ```Tools -> Board -> Boards Manager``` and search for "ESP32". Please install version 2.0.7.

Instructions to install the ESP32 board definitions are also available on this [espressif.com docs page](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html#installing-using-boards-manager)

In order for the project to compile, you need to download the following libraries which can all be installed using the Arduino IDE libraries manager at ```Sketch -> Include Library -> Manage Libraries```

- [MicroNMEA](https://github.com/stevemarple/MicroNMEA) v2.0.x by Steven Marple
- [Adafruit GFX Library](https://github.com/adafruit/Adafruit-GFX-Library) v1.11.x by Adafruit
- [Adafruit SSD1306](https://github.com/adafruit/Adafruit_SSD1306) v2.5.x by Adafruit
- [OneWire](https://www.pjrc.com/teensy/td_libs_OneWire.html) at least v2.3.7 by Paul Stoffregen
- [GParser](https://github.com/GyverLibs/GParser) v1.4.x by AlexGyver

If using the official PCB, the code in "B" corresponds to the board soldered above the silkscreen text "Made by Joseph Hewitt". The source file "A" is for the remaining ESP32. You may flash the boards when they are soldered to the PCB or before soldering -- it's your choice.

To upload code to your ESP32 boards, please use the following configuration in the `Tools` Arduino IDE dropdown menu:

- Board: `ESP32 Dev Module`
- Upload Speed: `921600`
- CPU Frequency: `240MHz (WiFi/BT)`
- Flash Frequency: `80MHz`
- Flash Mode: `DIO`
- Flash Size: `4MB (32Mb)`
- Partition Scheme: `Minimal SPIFFS (1.9MB APP with OTA/190KB SPIFFS)`
- PSRAM: `Disabled`
- Arduino Runs On: `Core 1`
- Events Run On: `Core 1`

If you have upload issues, try lowering the upload speed. Some users also report having to hold the "boot" button on the ESP32 board after connecting it to their PC before pressing the upload button in the Arduino IDE.

## First Time Setup

Please ensure you have a working micro SD card inserted which is formatted to FAT32. The Wardriver will not boot without one.

Add the [GSM cell towers database](https://wardriver.uk/gsm_location_3) to the SD card if you want your wardriver to be able to determine its location in areas with no GPS.

When you first boot your Wardriver after flashing, it should create a WiFi network which you can connect to using a smartphone. The network name and password are both ```wardriver.uk``` by default.

Upon connecting, please visit ```http://192.168.4.1/``` in a web browser.

This page should allow you to connect your Wardriver to an existing network. You should also be given an option to create a fallback network for when your configured WiFi network is unavailable.

Connecting your Wardriver to WiFi is recommended as it allows you to conveniently download the data you have collected wirelessly. If you would prefer to collect your data by plugging the SD card into your PC, you can leave the network configuration blank.

## Restarting the Setup

If you need to reconfigure your Wardriver, follow these steps to start the first time setup again:

- Unplug the power to your Wardriver
- Connect the power to your Wardriver
- Remove the power again when the LCD prompts "Power cycle now to factory reset"

This **does not** delete any collected data -- it simply erases the currently configured network(s) and will prompt you to reconfigure networking on the next boot.

## Collecting and Uploading Data

After the first time setup is complete, plugging your Wardriver into power will cause it to boot normally. If you are in range of your configured WiFi network, it will connect and display its IP address on the LCD. By visiting this IP address in a web browser, you are able to download all of the data you have collected so far. Please ensure your phone/PC is on the same WiFi network as your Wardriver. Simply click on the name of the file you wish to download and your device should download a file with a name in this format: `ID_wd3-NUM.csv` where `ID` corresponds to the unique hardware ID of your wardriver and `NUM` is the incremental ID of your wardriving session.

If you do not access or interact with the web interface after around 1 minute, the Wardriver will boot normally and start collecting data until you remove the power.

### Uploading to Wigle

You may wish to register or login to Wigle before uploading files - this will allow you keep long-term statistics about the data you have collected.

- Visit [Wigle.net](https://wigle.net/) and press the "Uploads" button in the top toolbar.
- Press the yellow "UPLOAD A FILE" button.
- Press the "Browse" button near the bottom left of the popup.
- Choose the file which you downloaded from your Wardriver (A CSV file in this format: `ID_wd3-NUM.csv`)
- Press "Send" at the bottom right of the popup.
