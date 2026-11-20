# NOTORYX Ops releases

Downloads for NOTORYX Ops, a MeshCore network dashboard and infrastructure station for a Raspberry Pi, a Mac or Docker.

- **Analyzer:** live packet route map with a 24-hour replay, packet feed and decoder, channel messages, observers, coverage hexes and analytics, including a prefix collision grid.
- **Fleet:** health, history and alerts (Discord or webhook) for the repeaters you own, plus bulk settings that show current values before anything is sent.
- **Coverage and planning:** terrain coverage around any spot, a link profile with line of sight and a signal budget, relay suggestions on high ground, and predictions tuned against links the station has actually heard. Elevation comes from free Copernicus data, downloaded once per area.
- **Field work:** hazard reports (flooded road, fallen tree, power line down and more) sent and received as plain channel messages any MeshCore app can read, teams on their own private channels with position tracks, and a phone survey mode that keeps working with no signal.
- **Integrations:** optional accounts with viewer, operator and admin roles, GeoJSON and GPX exports, Prometheus metrics, a read-only public map (off by default), Home Assistant sensors over MQTT and a PostgreSQL archive.

## Latest

- **[NOTORYX Ops 0.1.7 test 8](../../releases/tag/v0.1.7-test.8)** (19 November 2026)
- **[Meshtastic add-on 0.1.1](../../releases/tag/meshtastic-addon-v0.1.1)** (19 November 2026), for Ops 0.1.7

## Which file

| File | For |
|---|---|
| `notoryx-ops-<version>-pi.tar.gz` | Raspberry Pi or other Debian-based Linux. Unpack, then `sudo ./install.sh` |
| `notoryx-ops-<version>-mac.zip` | Mac. Needs Python 3.10+ from python.org. Unsigned: right-click the app and choose Open the first time |
| `notoryx-ops-<version>-docker.zip` | Docker. Unzip, then `docker compose up -d` |
| `notoryx_ops-<version>-py3-none-any.whl` | Any computer with Python 3.10+: `pip install` it, then `notoryx-ops run --open` |
| `notoryx-ops-<version>-source.tar.gz` | Source code (MIT, except the Meshtastic add-on, which is GPL-3.0) |
| `SHA256SUMS` | Checksums for every file |

To try it without a radio: `notoryx-ops run --demo --open` fills the dashboard with made-up traffic.

The dashboard opens at `http://127.0.0.1:8484`. Settings live in a `config.toml` that's created with comments on first start.

## Meshtastic add-on (optional)

`notoryx_ops_meshtastic-<version>-py3-none-any.whl` is a separate download. It gives the station its own Meshtastic radio, remote admin of other Meshtastic nodes over the air (firmware 2.5+), settings templates with compliance checks, and an optional Meshtastic⇄MeshCore channel bridge. Install it into the station's Python environment, then restart the station:

```
pip install notoryx_ops_meshtastic-<version>-py3-none-any.whl
```

On the Mac app that environment is `~/Library/Application Support/NOTORYX Ops/venv`. The add-on is GPL-3.0 because it uses the Meshtastic Python library; NOTORYX Ops itself is MIT and works without it. Meshtastic® is a registered trademark of Meshtastic LLC; this add-on is not affiliated with Meshtastic LLC.

## Status

Test builds. What has been checked on real hardware:

- **Station:** a Mac station with a MeshCore companion radio over Bluetooth: live packets, the map, stale nodes, the ignore list and range rings.
- **Hazard reports:** a report and its all-clear sent on a test channel, the repeater echo heard once and not duplicated.
- **Meshtastic add-on:** connecting to the station's own Meshtastic radio, reading and changing settings on a second node over the air, renaming it, setting a favourite, rebooting it, and the channel bridge in both directions without echo loops.

Not yet checked on hardware: teams on the radio, applying settings templates through the add-on, and the MeshCore fleet tools on repeaters you own.

## Problems or ideas

Open an issue on this repo.

---
🛠️ AI-assisted: built with Anthropic's Claude, directed and reviewed by [BaconDisk22](https://github.com/BaconDisk22). Checked by automated tests and CI; review before relying on it.
