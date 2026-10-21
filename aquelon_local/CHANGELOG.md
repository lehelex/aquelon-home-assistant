# Changelog

## 0.2.1

- Public metadata-only Home Assistant repository; application source stays private.
- Prebuilt amd64/arm64 bytecode images, checked natively with MQTT on both CPUs.
- Generic example configuration; no workshop address, ID or credentials.
- Same bridge engine and entity/command contract as app 0.2.0 (engine 0.1.1).
- Migrating from a local app requires configuring the new app and stopping the old one first.

## 0.2.0

- First local Supervisor app with automatic local MQTT service discovery.
- Startup reads protected options and drops to UID/GID 1000 before network use.
