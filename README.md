# esphome-config

ESPHome configurations for the devices in our home (lights, wall switches, plugs, sensors).
Compiled and flashed from the ESPHome dashboard running on ASROCK.

> **This repo is public.** Every secret lives in `secrets.yaml`, which is git-ignored.
> Never put SSIDs, passwords, keys or IPs directly in a device file.

## Layout

| Path | What it is |
|---|---|
| `base.yaml` | Shared package: Wi-Fi (static IP + fallback AP), captive portal, logger, encrypted API, OTA, HA time, Wi-Fi diagnostics, restart buttons. |
| `<device>.yaml` / `.yml` | One file per device. Sets `substitutions` (`devicename`, `fullname`, `ipPrefix`, `id`), includes `base.yaml`, then adds the platform block and components. |
| `secrets.example.yaml` | Template of every `!secret` key used. Copy to `secrets.yaml`. |
| `copychan.h` | Custom float output that mirrors one level to two channels. Currently unused. |
| `espsense.h` | Old ESPSense (TP-Link Kasa emulation) header. Currently unused; kept for reference. |
| `archive/` | Retired device configs, not built. |

Each device's static IP is `${ipPrefix}.${id}` (`ip_prefix` comes from `secrets.yaml`), so pick a free `id` for new devices.

## Secrets

```bash
cp secrets.example.yaml secrets.yaml   # then fill real values from Vaultwarden
```

Keys: `iot_ssid`, `iot_psswd`, `fallback_psswd`, `ota_psswd`, `esphome_encryption_key`, `ip_prefix`, `iot_gateway`, `subnet`.
Generate an API key with `openssl rand -base64 32`.

## Build / flash

1. Pull this repo into the ESPHome dashboard's config directory (`/config` in the container).
2. Make sure `secrets.yaml` exists there.
3. In the dashboard, **Validate**, then **Install → Wirelessly** (OTA, uses `ota_psswd`).
   CLI equivalent: `esphome run <device>.yaml --device <ip>`.
4. First flash of a new device: **Install → Plug into this computer** (or Tuya-Cloudcutter/LTChipTool for BK72xx).
5. If a device can't join Wi-Fi it starts the fallback AP `${devicename}` (password `fallback_psswd`) with a captive portal.

Changing the API key or OTA password: flash with the *old* OTA password first, or the device will refuse the update.

## Adding a device

```yaml
substitutions:
  devicename: my-device
  fullname: "My Device"
  ipPrefix: !secret ip_prefix
  id: "123"

packages:
  device_base: !include base.yaml

esp8266:
  board: esp01_1m
```

## Device inventory

| File | Platform / board | Name | Function |
|---|---|---|---|
| `back-entry-light.yml` | bk72xx / wb3s | Back Entry Light | Tuya RGBWW downlight |
| `back-entry-switch.yaml` | esp8266 / esp01_1m | Back Entry Switch | Wall switch (relay, button, motion/lux) |
| `bar-le-light.yaml` | bk72xx / wb3s | Bar LE Light | Tuya RGBWW downlight |
| `bar-lw-light.yaml` | bk72xx / wb3s | Bar LW Light | Tuya RGBWW downlight |
| `bar-ue-light.yaml` | bk72xx / wb3s | Bar UE Light | Tuya RGBWW downlight |
| `bar-uw-light.yaml` | bk72xx / wb3s | Bar UW Light | Tuya RGBWW downlight |
| `crock-pot.yml` | esp8266 / esp8285 | Crock-Pot | Crock-Pot controller w/ power monitoring |
| `dining-room-m-light.yml` | bk72xx / wb3s | Dining Room M Light | Tuya RGBWW downlight |
| `dining-room-ne-light.yml` | bk72xx / wb3s | Dining Room NE Light | Tuya RGBWW downlight |
| `dining-room-nw-light.yml` | bk72xx / wb3s | Dining Room NW Light | Tuya RGBWW downlight |
| `dining-room-se-light.yml` | bk72xx / wb3s | Dining Room SE Light | Tuya RGBWW downlight |
| `dining-room-sw-light.yml` | bk72xx / wb3s | Dining Room SW Light | Tuya RGBWW downlight |
| `fireplace-left-light.yaml` | bk72xx / generic-bk7231n-qfn32-tuya | Fireplace Left Light | Tuya RGBWW downlight |
| `fireplace-right-light.yaml` | bk72xx / generic-bk7231n-qfn32-tuya | Fireplace Right Light | Tuya RGBWW downlight |
| `floor-lamps-switch.yaml` | esp8266 / esp01_1m | Floor Lamps Switch | Wall switch (relay, button, motion/lux) |
| `front-closet-light.yml` | esp8266 / esp01_1m | Front Closet Light | Light / night light |
| `garage-doors.yml` | esp32 / esp32dev | Garage Doors | 2x garage door relay + reed, BLE proxy |
| `holiday-plug-1.yml` | esp8266 / esp01_1m | Holiday Plug 1 | Smart plug |
| `holiday-plug-2.yml` | esp8266 / esp01_1m | Holiday Plug 2 | Smart plug |
| `karlee-string-lights.yml` | esp8266 / esp01_1m | Karlee String Lights | Light / night light |
| `laundry-leak-detector.yaml` | ? / d1_mini | Kitchen Leak | DHT temp/humidity (leak detector) |
| `laundry-room-light.yml` | bk72xx / wb3s | Laundry Room Light | Tuya RGBWW downlight |
| `living-room-n-light.yml` | bk72xx / wb3s | Living Room N Light | Tuya RGBWW downlight |
| `living-room-ne-light.yml` | bk72xx / wb3s | Living Room NE Light | Tuya RGBWW downlight |
| `living-room-nw-light.yml` | bk72xx / wb3s | Living Room NW Light | Tuya RGBWW downlight |
| `living-room-s-light.yml` | bk72xx / wb3s | Living Room S Light | Tuya RGBWW downlight |
| `living-room-se-light.yml` | bk72xx / wb3s | Living Room SE Light | Tuya RGBWW downlight |
| `living-room-sw-light.yml` | bk72xx / wb3s | Living Room SW Light | Tuya RGBWW downlight |
| `lower-foyer-light.yml` | bk72xx / generic-bk7231t-qfn32-tuya | Lower Foyer Light | Tuya RGBWW downlight |
| `main-bath-switch.yml` | esp8266 / esp01_1m | Master Bath Switch | Wall switch (relay, button, motion/lux) |
| `master-night-light.yml` | esp8266 / esp01_1m | Master Night Light | Light / night light |
| `power-monitor-plug-8.yml` | esp8266 / esp01_1m | Power Monitor Plug 8 | Smart plug |
| `smart-plug-17.yml` | esp8266 / esp01_1m | Smart Plug 17 | Smart plug |
| `smart-plug-5.yml` | esp8266 / esp01_1m | Piano Room Lamp | Smart plug |
| `sump-level.yml` | esp8266 / d1_mini_lite | Sump Level | Ultrasonic level sensor |
| `up-bath-night-light.yml` | esp8266 / esp01_1m | Up Bath Night Light | Light / night light |
| `up-bath-switch.yml` | esp8266 / esp01_1m | Up Bath Switch | Wall switch (relay, button, motion/lux) |
| `water-softener.yml` | esp8266 / d1_mini | Water Softener Salt Level | Ultrasonic level sensor |
