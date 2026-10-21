# AQUELON Local

Connect your AQUELON V2 controller directly to Home Assistant's local MQTT
broker.

## Configure

1. The existing Mosquitto/MQTT app and the MQTT integration must be running.
2. Set **Controller address** to its local HTTP address and **Controller device
   ID** to its immutable AQUELON ID. Reserve the address in the router.
3. Choose a name. To keep the current entities during migration, preserve the
   device ID and discovery prefix exactly.
4. With **Enable control** on, enter the existing six-digit **controller owner
   PIN**, not your Home Assistant password. Missing/invalid PIN format stops
   startup; the app does not guess it. A wrong PIN stops further login attempts
   until you correct the option and restart.
5. Keep the 5-second refresh and `homeassistant` discovery prefix unless your
   existing MQTT integration is configured otherwise.
6. Save, then start the app. It obtains the local MQTT connection from
   Supervisor; you do not need to copy the broker password.

The default all-zero device ID is a placeholder. It must be replaced before
the app can connect to your controller.

## Migrate from the standalone bridge

Install and configure this app first, without starting it. Stop the old bridge
and disable its automatic restart before starting this app. Never run both
instances for the same controller together: they use the same MQTT client ID.
Keep the same control/read-only mode as before. The same device ID and discovery
prefix retain the HA entities, history and automation references.

Do not remove the existing MQTT integration or the AQUELON device. If the new
app cannot start, stop it before temporarily restarting the previous bridge.

## What appears

20 entities with control enabled: water/outdoor measurements, humidity, targets,
the three original operating modes, Comfort temperature adjustment, timed
lighting/mood/aerator/massage/manual circulation switches, output/fault status
and diagnostics. Temper target is currently read-only. Manual circulation is a
timed request, separate from the combined pump output.

Existing automatic shutoff, bookings and hardware protections remain in the
controller. This app does not expose direct heater/pump relay commands,
chemical dosing, fault acknowledgement, service overrides.

## Credentials and permissions

Only the `mqtt:need` service is requested. No Home Assistant Core API, full
Supervisor API, Docker socket, host network, device access, shared folders or
inbound listening ports are requested. The startup reads its own protected
options, drops to UID/GID 1000, then connects to MQTT and the controller.
The Supervisor service token is removed from the process environment and is
not passed to controller/MQTT connections or saved in another file.

The owner PIN is kept in the app's normal Supervisor-managed options. The
password UI masks it; that is not a claim that `/data/options.json` itself is
encrypted. Protect Home Assistant and its backups. Controller owner login uses
its existing HTTP API on the trusted LAN; no TLS is added to that hop.

## Availability and restarts

The local controller, Home Assistant and MQTT must be powered and reachable.
Internet is required to download/build/install updates, not for the normal
local data path. With a missing/frozen controller, entities become unavailable.
Restarts republish discovery and actual state, without replaying commands.
The HA and host clocks must be synchronized: source timestamps reject commands
older than 15 seconds or more than 5 seconds into the future.

Start on boot may be enabled on the Info page. Use the app's Log page to check
startup and connection errors. The program never logs passwords/PINs or tokens.
For configuration changes, save and restart this app; do not restart the main
controller or change its MQTT server.
