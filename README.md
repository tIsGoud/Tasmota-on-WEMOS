# Tasmota on WEMOS

Install Tasmota software on a WEMOS D1 mini (pro) with PlatformIO.

Follow the default instructions from the [Sonoff-Tasmota Uploads](https://github.com/arendst/Sonoff-Tasmota/wiki/Upload) page.

The previous version of the platformio.ini file contained only one [env] version, the current version has two [env] sections:

```
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter, extra scripting
;   Upload options: custom port, speed and extra flags
;   Library options: dependencies, extra library storages
;
; Please visit documentation for the other options and examples
; http://docs.platformio.org/en/stable/projectconf.html

[platformio]
src_dir = sonoff

; Sonoff et al (ESP8266 based)
[env:sonoff]
platform = espressif8266
framework = arduino
board = esp01_1m
board_flash_mode = qio
build_flags = -Wl,-Tesp8266.flash.1m0.ld -DMQTT_MAX_PACKET_SIZE=512
lib_deps = PubSubClient, NeoPixelBus, IRremoteESP8266, ArduinoJSON

; Sonoff Touch and Sonoff 4CH (ESP8285 based)
[env:sonoff-touch-4ch]
platform = espressif8266
framework = arduino
board = esp01_1m
board_flash_mode = dout
build_flags = -Wl,-Tesp8266.flash.1m0.ld -DMQTT_MAX_PACKET_SIZE=512
lib_deps = PubSubClient, NeoPixelBus, IRremoteESP8266, ArduinoJSON
```

My changes to upload the configuration to the WEMOS are below but they can probably be placed in a [env] section next to the existing sections.  
Note to self: Investigate the working of the sections and update this file.

```
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter, extra scripting
;   Upload options: custom port, speed and extra flags
;   Library options: dependencies, extra library storages
;
; Please visit documentation for the other options and examples
; http://docs.platformio.org/en/stable/projectconf.html

[platformio]
src_dir = sonoff

[env:sonoff]
platform = espressif8266
framework = arduino
board = esp01

; Select one of two board_flash_mode options below
; Sonoff Basic et al. (ESP8266 uses dio or qio)
board_flash_mode = dio
; Sonoff Touch and Sonoff 4CH (ESP8285 uses dout)
; board_flash_mode = dout

build_flags = -Wl,-Tesp8266.flash.1m0.ld -DMQTT_MAX_PACKET_SIZE=512
lib_deps = PubSubClient, NeoPixelBus, IRremoteESP8266, ArduinoJSON
```

So to wrap up:
- board changed from esp01_1m to esp01
- board_flash_mode changed from qio to dio

The changes seem minimal and I still don't fully understand why but it worked.
