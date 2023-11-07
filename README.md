# homebridge-viewsonic-projector

Homebridge plugin for ViewSonic projectors via serial RS232 as HomeKit TVs. Requires iOS >=12.2 and homebridge >=0.4.46.

This plugin connects to a ViewSonic projector via serial RS232. Developed and tested using a Raspberry Pi Zero W and a USB to serial RS232 cable to connect to a ViewSonic PX701HD. For now, **it only manage power ON / OFF**

## Table of Contents

- [Getting Started](#getting-started)
- [Configuration](#configuration)
  - [Example configuration](#example-configuration)
  - [General configuration](#general-configuration)
  - [Device configuration](#device-configuration)
- [Deep Dive](#deep-dive)
- [TODO](#todo)
- [Support](#support)

## Getting Started

Hardware and OS :

- Install [homebridge](https://github.com/nfarina/homebridge)
- Connect to projector via RS232 _(make sure it appears in /dev/tty)_

For now, this plugin can only be installed manually :

- Install this plugin from terminal :

```sh
sudo hb-shell
cd /var/lib/homebridge/node_modules/
git clone https://github.com/achorein/homebridge-viewsonic-projector.git
```

- Update your config.json, following the example below
- Restart HomeBridge (from UI)

## Configuration

### Example configuration

```json
{
   "bridge": {
     ...
   },
   "accessories": [
     ...
   ],
   "platforms": [
        {
            "devices": [
                {
                    "name": "ViewSonic",
                    "model": "PX701HDP",
                    "adapter": "/dev/ttyUSB0",
                    "baudrate": 115200,
                    "pollingInterval": 6000
                }
            ],
            "platform": "ViewSonic-Projector"
        },
        ...
    ]
}
```

### General configuration

| **Attributes** | **Required** | **Usage**                                                                           |
| -------------- | ------------ | ----------------------------------------------------------------------------------- |
| platform       | **Yes**      | Name of homebridge accessory plugin. Must be **ViewSonic-Projector**.               |
| devices        | **Yes**      | Array of ViewSonic projectors. Each of them must be added manually to the Home app! |

### Device configuration

| **Attributes**  | **Required** | **Usage**                                                        |
| --------------- | ------------ | ---------------------------------------------------------------- |
| name            | **Yes**      | Name of the projector, how you want it to appear in HomeKit.     |
| adapter         | **Yes**      | Path to serial RS232 adapter.                                    |
| model           | No           | Projector model. Only displayed in accessory details in HomeKit. |
| pollingInterval | No           | Polling interval in milliseconds _(Default: 6000)_               |

## Deep Dive

This projector have a strange behaviour, when the **power is off** it use string values to interact (`pow=?`, `pow=on`, ...), but when the **power is on** we can only use the hexadecimal communication (`06 14 00 04 00 34 12 00 00 5E`) as mention in the documentation.

More info :

- [Official documentation](https://www.viewsonicglobal.com/public/products_download/user_guide/Projector/PX701HDP/PX701HDP_UG_FRN.pdf?pass)
- More information on ViewSonic [RS-232 Protocol](https://www.viewsonicglobal.com/public/products_download/user_guide/Projector/PX701-4K_PX728-4K_PX748-4K/PX701-4k_PX748-4K_PX728-4K_%20RS232_table.pdf?pass)
- Thread about projector problem : https://www.remotecentral.com/cgi-bin/mboard/rs232-ip/thread.cgi?812
- Note: Updagrading the projector firmware to v1.01 does not improve the rs-232 communication

Special thanks to the [homebridge-benq-projector](https://github.com/solowalker27/homebridge-benq-projector) plugin, on which this one is very inspired.

## TODO

- [x] Manager power on and power off (+ check status)
- [ ] Plublish NPM package (for auto installation)
- [ ] Handle input management (check status, change to hdmi1/hdmi2/...)
- [ ] Handle eco mode (check status, changetto Eco/Dynamic Eco/SuperÉco+,Normal)

## Support

Enjoying this project? Wanna show some love? Drop a star and consider buying me a coffee to keep me fueled and motivated

<a href="https://www.buymeacoffee.com/achorein" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" ></a>
