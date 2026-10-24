# AQUELON Local

## Configuration

Install and configure an MQTT service and Home Assistant's MQTT integration
before starting AQUELON Local. The app uses the MQTT service supplied by
Home Assistant Supervisor.

| Option | Description |
| --- | --- |
| Controller address | Local HTTP address of your AQUELON controller, for example `http://192.168.1.100`. Use an address reserved in your router. |
| Controller device ID | The controller's permanent AQUELON device ID. Replace the all-zero example before starting the app. |
| Device name | The name displayed for the device in Home Assistant. |
| Enable control | Enable operating mode, Comfort target, and timed function controls. Turn off for a monitoring-only installation. |
| Controller owner PIN | The existing six-digit controller owner PIN. Required when control is enabled; this is not your Home Assistant password. |
| Refresh interval in seconds | Status polling interval, from 3 to 10 seconds. The default is 5 seconds. |
| MQTT discovery prefix | Use `homeassistant` unless your MQTT integration is configured with a different discovery prefix. |

Save your configuration, then start the app. After changing configuration,
save and restart the app. Enable **Start on boot** on the app's Info page if
you want it to start automatically with Home Assistant.

## Entities and controls

With control enabled, AQUELON Local provides 21 entities:

| Group | Entities |
| --- | --- |
| Measurements | Water temperature, outdoor temperature, outdoor humidity, active target, Temper target |
| Controls | Comfort thermostat, operating mode and Comfort target temperature |
| Timed switches | Lighting, mood lighting, aeration, massage, manual circulation |
| Status | Control state, circulation output, heating output, fault, low water warning |
| Diagnostics | Connection status, fault key, controller uptime |

Comfort target adjustment uses 0.5 degrees Celsius steps from 30 to 43 degrees
Celsius. The Temper target is read-only. Timed switches use the controller's
configured automatic shutoff times.

## Comfort thermostat

Add the **Comfort thermostat** entity to a Home Assistant thermostat card for
current water temperature and Comfort target adjustment in 0.5 °C steps
(30–43 °C). Changing the target does not turn heating on. **Heat** selects
Comfort mode; **Off** selects the controller's Off mode. The activity indicator
uses the controller's reported heating output and is not an independent
measurement of heater power.

The thermostat is specifically for Comfort. It shows **Off** while the controller
is in Temper mode, and continues to display the saved Comfort target. The separate
**Operating mode** entity shows the actual Off / Temper / Comfort mode; that mode
is also available in the thermostat's attributes. The Temper target remains
read-only. Existing entities and their identifiers are preserved.

The thermostat becomes unavailable when control authentication or required
temperature readings are unavailable. Changes appear after controller readback;
the card does not assume that a command succeeded.

Manual circulation is a timed request. Turning that request off does not cancel
other circulation demands. Heating and circulation output sensors show commanded
output status, rather than independent confirmation that physical equipment is
running. The controller remains responsible for equipment supervision.

## Availability

Home Assistant, its MQTT service, and the controller must be powered and reachable
on the local network. Internet access is needed for app installation and updates,
but not for normal local communication.

Unavailable or stale controller data makes the entities unavailable. Reconnection
and app restarts republish discovery and current state. Commands are not queued
for later replay. Home Assistant's clock must be correct because commands carry
timestamps and expire if delayed.

## Existing Home Assistant entities

Run only one AQUELON Local instance for a given controller. When replacing an
existing instance, configure the new instance without starting it, stop the old
instance and disable its automatic startup, then start the new one.

Keep the same device ID, MQTT discovery prefix, and control or monitoring mode.
This preserves entity IDs, history, and automation references. Do not delete the
existing AQUELON MQTT device during the change.

## Troubleshooting

- **The app will not start:** check its Log page, controller address, device ID,
  MQTT service, and required owner PIN. The example device ID is not a valid
  installation setting.
- **Control is unavailable:** confirm that **Enable control** is on and the
  controller owner PIN is correct. A rejected PIN stops further login attempts;
  correct the setting and restart the app.
- **Entities are unavailable:** check network access to the controller and the
  MQTT service, and confirm that only one instance is using the same device ID.
- **The device is not discovered:** check the MQTT integration and ensure its
  discovery prefix matches the app configuration.

## Credentials

The app requests the MQTT service it needs from Supervisor. It does not require
Home Assistant Core API access, shared host folders, or inbound listening ports.

The owner PIN is stored in Supervisor-managed app options. The password field
masks it on screen; protect Home Assistant and its backups. The controller's
HTTP connection is intended for a trusted local network. Do not expose it or
an unencrypted MQTT broker directly to the internet.

When requesting support, include the app version and relevant error messages.
Do not post PINs, passwords, tokens, or complete app configurations.
