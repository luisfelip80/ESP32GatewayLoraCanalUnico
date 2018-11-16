# ESP32SingleChannelGateway
Um Fork do popular ESP8266 gateway canal único adaptado para ESP32 na frequência de 915Mhz banda.

# Instruções
- Copie as bibliotecas(libraries) para ~/Documentos/Arduino/libraries
- Para o nó lora, verifique na biblioteca lmic o arquivo config.h e tenha certeza que a banda está em 915Mhz apenas.

# Traduzir depois:
- Original: https://ldsrc.blogspot.com/2018/01/lorawan-working-example-from-network.html
Just listing the things I did to get it working (hopefully useful for others)
- install Arduino IDE https://www.arduino.cc/en/Main/Software3
- install ESP8266 core as per github https://github.com/esp8266/Arduino6
- install ESP32 core as per github https://github.com/espressif/arduino-esp3210
- Execute the Arduino IDE and goto “File->Preferences” and make a note of the “Sketchbook Location”. This is the “stub” folder which you will reference when loading folders/files (we will call it STUB from now on) It is usually something like “C:\Users???\Documents\arduino”.
- Download ZIP file from https://github.com/kersing/ESP-1ch-Gateway-v5.025
- Extract the ZIP file (preferably using 7-zip http://www.7-zip.org/1 )
- Copy everything from the extracted file folder “libraries” and paste into STUB\libraries"
- Copy folder “ESP-sc-gway” from the zip file and paste into “STUB\libraries”
- Go to “STUB\libraries\ESP-sc-gway\ESP-sc-gway.h” and alter the code using “wordpad”.
You need to update the following 3 sections
1 Gateway Ident definitions
(description, Lat long)
2 ntp
(for UK, change to NTP_TIMESERVER “uk.pool.ntp.org” and NTP_TIMEZONES 0
3 definition of wpas wpa[]
Change code from this: (change the wifi and passwd also take off the if-else-endif statement)
#if 0
wpas wpa[] = {
{ “” , “” }, // Reserved for WiFi Manager
{ “ape”, “apePasswd” }
{ “ape”, “apePasswd” }
};
#else
Place outside version control to avoid the risk of commiting it to github :wink:
#include “d:\arduino\wpa.h”
#endif
to this
wpas wpa[] = {
{ "" , "" }, // Reserved for WiFi Manager
{ "YOUR WIFI SSID", "YOUR WIFI SSID PASSWORD" },
{"",""}
};
Execute the Arduino IDE
Ensure you have installed both the SPIFFS and U8G2 libraries as shown (ignore the others shown in this pic) using “Sketch->Include Library->Manage Libraries”
spiffs_library
spiffs_library.jpg792x454 74.9 KB
u8g2
u8g2.jpg801x279 83.3 KB
Upload the sketch via “File->Examples->ESP-sc-gway”. Set up the port and upload into your Heltec WiFi LoRa 32 device, (when the device re-boots, you should see info displayed on the OLED)
When the device has booted and logged into your wifi network, find the IP address it is using from your router and enter this IP address into a web browser to display a webpage of Gateway details - note the 8 byte “Gateway ID” (you need this for the TTN console)
Log into the TTN console and setup a new gateway - You need to tick the “I’m using the legacy packet forwarder” checkbox and paste the 8 byte “Gateway ID” seen from the webpage.
setup a few other things like frequency and save!
thats it - amazing !!!
2) Example TX code for HelTec Lora (for use with Gateway above)
Download the zip file for LMIC https://github.com/matthijskooijman/arduino-lmic8
unzip and rename folder “arduino-lmic-master” to “arduino-lmic” (there can be a clash with other versions of the “LMIC” library)
Put the renamed “arduino-lmic” folder into the STUB/libraries
(optional) If you have version problems when compiling the code, try removing the standard library called “IBM_LMIC_framework” from STUB/libraries (you can easily put it back using Sketch->Include Library->Manage Libraries)
Copy and paste the preformatted text seen below into the Arduino IDE.
The 3 lines that need changing are prefixed with “$$”. Remove the “$$” and replace the code using info defined in TTN - remember that in TTN, you can only switch the device from OTAA to ABP after you have saved the device in TTN!!! Once the device has been changed to “ABP”, you will then see the 3 codes in the console that you need to copy back to the Arduino code. i.e.
For DEVADDR/Device Address - just copy the hex and add after the 0x in the code
For NWKSKEY and APPSKEY - click the eye (show code) then the <> (C-style), click the “copy” button and then use that to paste into the code (do not press the ⇆ button and check that your showing MSB before copying!)

