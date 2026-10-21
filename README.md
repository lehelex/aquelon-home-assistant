# AQUELON Local for Home Assistant

Connect your AQUELON controller to Home Assistant using MQTT discovery.
View temperatures and operating status, adjust the Comfort target, and control
timed functions from your Home Assistant dashboards and automations.

[![Add repository to Home Assistant](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2Flehelex%2Faquelon-home-assistant)

Repository URL: `https://github.com/lehelex/aquelon-home-assistant`

## Requirements

- Home Assistant OS with Supervisor and Home Assistant Core 2026.10.0 or later.
- An amd64 or aarch64 Home Assistant host.
- A running MQTT service, such as the Mosquitto broker app, and Home Assistant's
  MQTT integration.
- An AQUELON controller reachable from Home Assistant on your local network.

## Installation

1. Add this repository using the button above, or enter its URL in the
   Home Assistant app store's repository settings.
2. Install **AQUELON Local**.
3. Open its configuration and enter your controller's address, device ID, and
   preferred device name. Replace both example values with your own details.
4. To enable control, enter the controller's existing six-digit owner PIN.
   For monitoring only, turn off **Enable control**.
5. Save the configuration, then start the app.
6. Find your AQUELON device under **Settings > Devices & services > MQTT**.

The app obtains its MQTT connection from Home Assistant Supervisor. No separate
MQTT username or password is required in the app configuration. Reserve your
controller's IP address in your router so it remains reachable.

## Available entities

With control enabled, the app provides 20 entities covering:

- Water temperature, outdoor temperature and humidity, and temperature targets.
- Off, Temper, and Comfort operating modes, plus Comfort target adjustment.
- Timed lighting, mood lighting, aeration, massage, and manual circulation.
- Circulation and heating status, fault indications, and diagnostics.

The Temper target is read-only. Heating and circulation output entities report
status; they do not directly switch the heater or pump relays. Manual circulation
is a timed request and may differ from the pump's combined operating status.

## Operation and updates

Communication between Home Assistant, MQTT, and the controller stays on your
local network. Internet access is required to install and update the app, but
not for normal operation. Updates are provided through the Home Assistant app
store as prebuilt images; no local compilation or GitHub account is required.

If the controller or MQTT becomes unreachable, the affected entities become
unavailable. The app restores discovery and current state after reconnection.
The controller continues to manage its own operation and safety protections.

Run only one app instance for each controller. Keep Home Assistant and its
backups protected because the app configuration includes the owner PIN.

[Configuration and troubleshooting](https://github.com/lehelex/aquelon-home-assistant/blob/main/aquelon_local/DOCS.md) |
[Changelog](https://github.com/lehelex/aquelon-home-assistant/blob/main/aquelon_local/CHANGELOG.md)
