# NOTORYX Ops releases

Downloads for NOTORYX Ops, a MeshCore network dashboard and infrastructure station for a Raspberry Pi, a Mac or Docker.

- **Analyzer:** live packet route map with a 24-hour replay, packet feed and decoder, channel messages, observers, coverage hexes and analytics, including a prefix collision grid.
- **Fleet:** health, history and alerts (Discord or webhook) for the repeaters you own, plus bulk settings that show current values before anything is sent.

## Which file

| File | For |
|---|---|
| `notoryx-ops-<version>-pi.tar.gz` | Raspberry Pi or other Debian-based Linux. Unpack, then `sudo ./install.sh` |
| `notoryx-ops-<version>-mac.zip` | Mac. Needs Python 3.10+ from python.org. Unsigned: right-click the app and choose Open the first time |
| `notoryx-ops-<version>-docker.zip` | Docker. Unzip, then `docker compose up -d` |
| `notoryx_ops-<version>-py3-none-any.whl` | Any computer with Python 3.10+: `pip install` it, then `notoryx-ops run --open` |
| `SHA256SUMS` | Checksums for every file |

To try it without a radio: `notoryx-ops run --demo --open` fills the dashboard with made-up traffic.

The dashboard opens at `http://127.0.0.1:8484`. Settings live in a `config.toml` that's created with comments on first start.

## Status

Test builds. Not yet tested with a real radio.

## Problems or ideas

Open an issue on this repo.

---
🛠️ AI-assisted: built with Anthropic's Claude, directed and reviewed by [BaconDisk22](https://github.com/BaconDisk22). Checked by automated tests and CI; review before relying on it.
