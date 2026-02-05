This fork is designed to address a couple of issues that are either down to quirks of my smart meter or energy supplier (Green Energy UK). 

The first is that for some reason the gas tariff reported by the display is out by a factor of 10 (instead of ~7p/unit, it's coming back with ~0.7p/unit), this despite the daily charge on the display being correct. So there's just a simple multiplier in the code to deal with that.

The second is that the daily electricity usage figures reset about 15 minutes before midnight, this means that the day's usage recorded by HA is basically the standing charge for the following day. The fix I've implemented (which borrows a small portion of @merewords fix for megakid#87 in the original repo for multivalued tariffs, which I'll most likely end up subsuming as well when we switch to one of those in the near future), tests if the current reading is less than the previous and if so, continues to report that until the following day. In testing, this worked as expected however it had an unexpected side effect, the following day's readings all started out negative. Took me a while to figure this out but it's down to the sensor having a state_class of TOTAL rather than TOTAL_INCREASING. I think that the only reason it works in the original form, is that it is reliant on the display resetting sometime *after* midnight, which then serves to effectively cancel out the previous day's readings (if that makes sense). However if it resets at (or maybe close to) midnight that may not happen.

Caveat: I've only just started playing with HA so probably don't know what I'm doing...

# hildebrand_glow_ihd
Hildebrand Glow IHD Local MQTT Home Assistant integration

Inspired and heavily based on [@robertalexa](https://github.com/robertalexa) forum thread here: https://community.home-assistant.io/t/glow-hildebrand-display-local-mqtt-access-template-help/428638

# Guide

## Installation & Usage

1. Install MQTT Addon (https://home-assistant.io/components/mqtt/) - required for this integration to work
2. Buy a Hildebrand Glow IHD device (https://shop.glowmarkt.com/products/display-and-cad-combined-for-smart-meter-customers)
3. Follow this blog post to connect the Hildebrand Glow IHD device (https://medium.com/@joshua.cooper/glow-local-mqtt-f69b776b7af4)
4. Add repository to HACS (see https://hacs.xyz/docs/faq/custom_repositories) - use "https://github.com/fridgemagnet3/ha_hildebrand_glow_ihd_mqtt" as the repository URL.
5. Install the `hildebrand_glow_ihd_mqtt` integration inside HACS
6. Restart HA
7. Add Hildebrand Glow IHD MQTT integration using the normal HA integration configuration screen - note if you leave the Device ID as '+' it will automatically detect all Hildebrand Glow IHD devices publishing to your MQTT - this is the recommended way.
8. Your various `sensor`s will be named something like `sensor.smart_meter...` grouped as devices.

![image](https://user-images.githubusercontent.com/1478003/173249987-4724af89-ceaa-4422-a426-4b8a2b16d98e.png)
