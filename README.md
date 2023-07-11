# HomeAssistant Custom Integration for Dyson

This is a Home Assistant custom integration for dyson under active development.

- It does not rely on a dyson account. Which means once configured, the integration will no longer login to the Dyson cloud service so fast and more reliable start up process.
- Config flow and discovery is supported, so easier configuration.

## Migration from shenxn/ha-dyson

If you used the original repository from shenxn, you can migrate fairly easily:

### Experimental no-reconfiguration migration

 This is less proven, but it is possible to switch over with zero impact to your current integration configuration, entities/devices, or dashboards. I don't know what side-effects it may have though (leftover old config data might start causing issues or something - no guarantees).

1. Remove the ha-dyson and ha-dyson-cloud custom repositories from HACS
    - _Without_ removing the integrations themselves
3. Install the new [ha-dyson](https://github.com/libdyson-wg/ha-dyson)
    - If an update is available, update the ha-dyson repository using the HACS updater

### Proven some-reconfiguration migration

This is proven to work without any side effects. If you used the default IDs for the entities and devices, then you'll just need to re-configure the devices but your dashboards will not need updating.

1. Remove the Dyson Local and Dyson Cloud _integrations_ from your /config/integrations page.
1. Remove the Dyson Local and Dyson Cloud _integrations_ from your /hacs/integrations page.
2. Remove the dyson-ha and dyson-ha-cloud custom repositories from HACS
3. Add the new [dyson-ha](https://github.com/libdyson-wg/ha-dyson)
    - If an update is available, update the ha-dyson repository using the HACS updater

## Installation

The minimum supported Home Assistant version is 2021.12.0.

You can install using HACS. If it is not yet available in the default HACS search, you can add https://github.com/libdyson-wg/ha-dyson as a custom repository. 

You can also install manually by copying the `custom_components` from this repository into your Home Assistant installation.

### Dyson Devices Supported

Dyson Local uses MQTT-based protocol to communicate with local Dyson devices using credentials. Only WiFi enabled models have this capability. Currently the following models are supported, and support for more models can be added on request.

- Dyson 360 Eye robot vacuum
- Dyson 360 Heurist robot vacuum
- Dyson Pure Cool
- Dyson Purifier Cool
- Dyson Purifier Cool Formaldehyde
- Dyson Pure Cool Desk
- Dyson Pure Cool Link
- Dyson Pure Cool Link Desk
- Dyson Pure Hot+Cool
- Dyson Pure Hot+Cool Link
- Dyson Purifier Hot+Cool
- Dyson Purifier Hot+Cool Formaldehyde
- Dyson Pure Humidity+Cool
- Dyson Purifier Humidity+Cool
- Dyson Purifier Humidity+Cool Formaldehyde

### Dyson Cloud

Dyson Cloud uses HTTP-based API to communicate with cloud service. Currently it supports getting device credentials and show all devices as discovered entities under the Integrations page. It also supports getting cleaning maps as `camera` entities for 360 Eye robot vacuum.

## Setup

### Setup using device WiFi information

Note: Some new models released after 2020 do not ship with a WiFi sticker. They are still supported by this integration, but the Dyson Cloud integration is necessary for initial setup. After setup, however, the Dyson Cloud integration can be removed.

Find your device WiFi SSID and password on the sticker on your device body or user's manual. Don't fill in your home WiFi information. Note that this method only uses SSID and password to calculate serial, credential, and device type so you still need to setup your device on the official mobile app first.

### Setup using Dyson cloud account

You can also set up Dyson Cloud first so that you don't need to manually get device credentials. To do so, go to **Configuration** -> **Integrations** and click the **+** button. Then find Dyson Cloud. After successful setup, all devices under the account will be shown as discovered entities and you can then set up Dyson Local with single click. Leave host blank to using zeroconf discovery. After that, you can even remove Dyson Cloud entity if you don't need cleaning maps. All local devices that are already set up will remain untouched.

### Setup manually

If you want to manually set up Dyson Local, you need to get credentials first. Clone or download https://github.com/libdyson-wg/libdyson-neon, then use `python3 get_devices.py` to do that. You may need to install some dependencies using `pip3 install -r requirements.txt`.

## FAQ

### I got "not a valid add-on repository" when I try to add this repo

This is a **custom integration** not a **custom add-on**. You need to install [HACS](https://hacs.xyz/) and add this repo there.
