# NOTORYX Ops user guide

## Install

### Raspberry Pi (or any Debian-based Linux)

1. Download `notoryx-ops-<version>-pi.tar.gz` and unpack it on the Pi.
2. In that folder run `sudo ./install.sh`. It installs the station as a service called `notoryx-ops`.
3. Edit `/etc/notoryx-ops/config.toml` to set your radio, then `sudo systemctl restart notoryx-ops`.
4. Open `http://127.0.0.1:8484` on the Pi. To open it from another computer, set `host = "0.0.0.0"` and an `admin_token` first.

Logs: `journalctl -u notoryx-ops -f`

#### Desk or wall screen (kiosk)

With a touchscreen on the Pi, the station can run its own screen with no computer:

1. After `install.sh`, run `./kiosk.sh` as the user who signs in to the desktop (not with sudo).
2. Restart the Pi. Chromium opens `http://127.0.0.1:8484/kiosk` full screen: the live map with a big status bar (radio, battery, packets a minute, nodes today, alerts, clock).
3. Turn off **Screen Blanking** in Raspberry Pi Configuration › Display. The kiosk dims itself at night instead; tap to wake it. **Menu** sets dimming to at night, always or never, and goes back to the full dashboard.
4. Left alone for 2 minutes, the screen goes back to the live map.

`./kiosk.sh --remove` takes it out again. Any browser can open `/kiosk` too.

### Mac

1. Install Python 3.10 or newer from python.org if you don't have it.
2. Download `notoryx-ops-<version>-mac.zip`, unzip it and move **NOTORYX Ops** to Applications.
3. The app isn't signed yet: the first time, right-click it and choose **Open**.
4. It sets itself up on first launch (about a minute), starts the station and shows the dashboard in its own window. Closing the window keeps the station running; click the Dock icon to bring it back. **Quit** (Cmd-Q) stops the station. **Stop station** on the Station page stops it too and offers to start it again.
5. The NOTORYX Ops menu has Edit Settings, Show Station Log and Start or Restart Station. The Go menu jumps to each page (Cmd-1 to Cmd-9), and View has zoom, reload and Open in Browser.

Settings: `~/Library/Application Support/NOTORYX Ops/config.toml`. Log: `station.log` in the same folder.

### Docker

1. Download `notoryx-ops-<version>-docker.zip` and unzip it.
2. Run `docker compose up -d` in that folder.
3. Settings are in `data/config.toml`. Because requests come from the Docker network, set an `admin_token` to make changes from the dashboard.

## Settings

Open **Settings** in the sidebar (Cmd-, in the Mac app). It has the look and distance units, the radio (with **Find ports** for USB and **Scan** for Bluetooth), channels, fleet checks, alerts (with **Send a test alert**), MQTT brokers, station and access. **Save** applies changes right away; only the listen address and port wait for a restart. Passwords, tokens and webhook links are never shown again after saving.

Everything is stored in a settings file you can also edit by hand. The main parts:

| Section | What to set |
|---|---|
| `[station]` | Name, web port, `host`, `admin_token`, how long to keep packets, `stale_after_hours` (0 = off, 1, 6, 24, 72 or 168) |
| `[radio]` | `transport` = `serial`, `ble`, `tcp` or `none`, plus the port, Bluetooth address or TCP host |
| `[channels]` | Channel keys to decrypt: `"public"`, `"#hashtag"` or `"Name=32 hex digits"` |
| `[fleet]` | Poll interval (minimum 5 minutes), spacing between repeaters, quiet hours, low battery volts, expected firmware |
| `[alerts]` | Discord webhook URL, generic webhook URL and its signing secret |
| `[[mqtt]]` | One block per broker that carries observer packets |

Restart the station after editing the file by hand.

### Distance units

**Look and units → Distances** switches every page between **Kilometres and metres** and **Miles and feet**: range rings, Coverage hex sizes, radii, heights and areas, link lengths, survey accuracy and the public map's position detail. Heights you type are converted to metres for the station. The choice is saved in this browser; until you pick one, browsers set to the United States start in miles.

### Stale and ignored nodes

**Mark nodes stale after** (Off, 1 h, 6 h, 24 h, 3 days or 7 days; default 24 h) labels any node or observer not heard for that long as **stale · age** with a clock icon, and fades it on the Live map. A node never heard yet is never stale, and neither is one whose time can't be trusted (0, before 2020, or more than a day ahead, as from a radio whose clock was never set): it shows "unknown" instead of an age. The fleet's own **Quiet after** alert is separate and unchanged.

**Ignore** a node from the Nodes page, its Live map popup, a packet's detail or the Analytics advert table. Ignored nodes are hidden from the map, node and packet lists and analytics, and never raise alerts; any open alerts about one are cleared. Their packets are still stored. The **Ignored nodes** card in Settings lists them with **Show again**. Ignoring needs the admin token like any other change. Only packets that carry a node's full key (adverts) can be tied to it; other packets name their sender by one byte, which several nodes can share, so those stay visible.

## Pages

- **Live map:** every node with a known position, links between them (colour = signal), and routes drawn as packets arrive. Your repeaters have a ring, red when quiet. Stale nodes are faded and labelled. **Show range rings** draws four distance rings, with spokes every 45° and N/E/S/W tags, around the station radio's position, sized to the part of the map the panels and controls leave clear, with every label kept in that part; the choice is saved in this browser. **Replay last 24 h** plays back the day at up to 300× speed.
- **Packets:** live feed with filters. Click a packet for its decoded fields, every observer that heard it, and the raw bytes. **Decode a packet** decodes pasted hex.
- **Nodes:** every node the station has heard, with type, prefix, last heard and a stale label, plus **Ignore**.
- **Channels:** messages on channels you have keys for, and hashes of other channels heard.
- **My fleet:** repeaters you own. Add one by picking it from repeaters the station has heard, with its admin password. Click a row for battery, noise and airtime history and its neighbours. **Bulk settings** reads each repeater's current value, shows what would change, and sends only after you confirm.
- **Observers:** each radio feeding the station, how much it hears and its average signal, with a stale label when it has gone silent for longer than the stale setting.
- **Coverage:** hexes where a node was heard directly, coloured by best SNR, and nodes far from every known repeater.
- **Analytics:** totals, a weekday-by-hour activity map, packet types, busiest relays and senders, and the 256 one-byte prefixes with clashes and free ones.
- **Alerts:** open and cleared alerts. Alerts clear on their own once the problem is gone.
- **Station:** radio and broker state, decoding counts, fleet check timing, and Stop station.
- **Settings:** everything you can change. See above.

## Airtime

Every fleet request goes over the air. By default each repeater is polled every 30 minutes with 20 seconds between repeaters. A poll is a login plus a status request (asked once more if the first reply never comes); firmware is read once a day and neighbours every 6 hours. Raise `poll_minutes` on a busy mesh.

## Security

- Admin passwords and channel keys stay in the station's database and settings file, both readable only by the station user.
- Without an `admin_token`, changes are only accepted from the station computer itself. With one, every change needs it.
- The dashboard has no login for reading. Keep it on your own network.

## Observer messages over MQTT

The station reads JSON messages with the raw packet in `raw` (or `hex`, `payload`, `data`, `packet`), and optional `SNR`, `RSSI`, `origin` and `origin_id`. Plain hex message bodies work too. Messages marked `"direction": "tx"` are skipped. The default topic filter is `meshcore/+/+/packets`.
