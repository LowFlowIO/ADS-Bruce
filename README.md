# ADS-B Radar for Bruce JS

An offline-capable tactical ADS-B radar display script written for the **Bruce JS** firmware runtime environment on the **M5StickC Plus2**. It tracks, calculates, and visually maps live aircraft data around a specific geographic coordinate.

## Features

* **Dynamic Radar Sweep:** Minimalist retro-style PPI radar display with an animated sweeping arm.
* **Real-time Traffic Tracking:** Filters and maps up to 20 aircraft within a custom range ring.
* **Flight Route Lookup:** Integrated lookup utilizing multiple APIs (`adsb.lol` and `adsbdb.com`) with local cross-session route caching to limit networking over trivial tasks.
* **Flicker-Free UI:** Separate rendering boundaries split the radar frame from the side information panel, keeping display updates fast and smooth.
* **Adaptive Units:** Automatically handles geographic conversions (Haversine distance and bearing calculations) to output metric information (meters, km/h, m/s).

---

## Hardware & Environment

* **Target Device:** M5StickC Plus2
* **Firmware Runtime:** [Bruce JS](https://github.com/m5stack/Bruce) (JavaScript Engine)
* **Default Airspace:** London Heathrow Airport (`EGLL`) region.

---

## Controls

The script handles native M5Stick hardware buttons exposed via the Bruce JS `keyboard` module, alongside standard keyboard fallbacks.

| Action | Hardware Button / Key | Description |
| :--- | :--- | :--- |
| **Previous** | Up / Previous Button | Cycle to the previous (closer) aircraft |
| **Next** | Down / Next Button | Cycle to the next (further) aircraft |
| **Select** | Select Button / Enter | Force manual refresh of aircraft data & routes |
| **Escape** | Back / Escape Button | Safely exit the application |

---

## Configuration

You can customize the targeted location, scan range, and refresh timings by editing the global constants at the top of the script:

```javascript
var AREA_LAT = 51.4706;       // Target Latitude (Default: Heathrow)
var AREA_LON = -0.4619;       // Target Longitude
var RANGE_KM = 42;            // Radar scanning radius limit in kilometers
var REFRESH_MS = 15000;       // API Polling interval (15 seconds)
var MAX_PLANES = 20;          // Max number of simultaneous objects to track
```

---

## API Integrations & Offline Handling

The application leverages the following endpoints for fetching telemetry:

* **Data Feed:** `api.adsb.lol/v2/point/`
* **Route Feeds:** `api.adsb.lol/api/0/routeset` & `api.adsbdb.com/v0/callsign/`

### Offline & Legacy Resilience

* **Wi-Fi Check:** Leverages `wifi.connectDialog()` to handle connection drops gracefully without crashing the loop.
* **Fault-Tolerant Logging:** Safely suppresses missing native log macros (like `println()`) present in older or customized Bruce builds to guarantee high runtime uptime.
