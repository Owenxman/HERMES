# HERMES

**Hardware Environmental & Rotational Motion Evaluation Sensor**

HERMES is a USB-connected sensor board that reads motion, environmental, and GPS data and sends it to a computer in real time. The data is visualized in a browser dashboard that connects directly over Web Serial.

> 🛰️ **Live dashboard:** https://owenxman.github.io/HERMES/
> Works in desktop Chrome and Edge. Use **Demo data** if you don't have hardware connected.

## What this is

The board combines a 9-axis IMU, environmental sensor, and GPS module with an ATmega328 microcontroller. The microcontroller reads everything and streams it over USB serial to a computer.

The goal is simple: plug it in, open the dashboard, and see live motion + environmental data without any setup beyond a serial connection.

## Hardware

| Part | What it does |
|------|--------------|
| **ICM-20948** | 9-axis IMU (accelerometer, gyroscope, magnetometer) |
| **BME680** | Temperature, humidity, pressure, gas / VOC |
| **GPS module** | Position data over UART |
| **ATmega328-A** | Main microcontroller that reads sensors |
| **FT231XS** | USB to serial bridge |
| **AP2112K-3.3** | 3.3V regulator |

Connection: USB-C (power + data), 8 MHz clock, ICSP header for programming.

## Dashboard

The dashboard is a single `index.html` file. There is no backend, no build system, and nothing to install.

It reads serial data from the board and visualizes it in real time:

| View | Shows |
|------|-------|
| **Orientation** | 3D model that moves with roll, pitch, and yaw |
| **Accelerometer** | X/Y/Z arrows + combined force vector |
| **Magnetometer** | Compass-style heading display |
| **Environment** | Live graphs for temperature, humidity, pressure, and gas |
| **GPS** | Latitude, longitude, altitude, and satellite count |
| **Raw telemetry** | Live serial log + update rate (Hz) |

Everything runs in the browser. The board connects directly to your computer using Web Serial, and nothing gets uploaded anywhere.

## How to run it

### Locally

Open `index.html` in Chrome or Edge. Click **Connect device**, select the FT231XS serial port, and set the baud rate (default is `115200`).

### Hosting it

Any static HTTPS host works — Web Serial requires HTTPS.

**GitHub Pages**

1. Put `index.html` in the root of the repo.
2. Go to **Settings → Pages**.
3. Set **Source: Deploy from branch**, **Branch: `main` / `/root`**, then **Save**.
4. The site will be live at `https://owenxman.github.io/HERMES/`.

Every push updates the site automatically.

Other options: Cloudflare Pages, Netlify, Vercel.

### Browser support

Web Serial only works in Chromium-based browsers on desktop: **Chrome, Edge, Brave, Opera**.

Firefox, Safari, and mobile browsers will still load the page, but cannot connect to the board. Demo mode still works everywhere.

## Serial format

The board prints one line per update over serial. The dashboard tries to auto-detect the format. You can use any of:

**JSON (recommended)**

```json
{"roll":2.1,"pitch":-5,"yaw":143,"ax":0.01,"ay":0.0,"az":1.0,"heading":143,"temp":22.4,"hum":41,"pres":1012.8,"gas":52000,"lat":44.47,"lon":-73.21,"alt":61,"sats":9}
```

**key=value**

```
roll=2.1 pitch=-5 yaw=143 ax=0.01 ay=0 az=1 temp=22.4 hum=41 pres=1012.8 gas=52000
```

**CSV** — first send a header line, then numeric rows:

```
roll,pitch,yaw,ax,ay,az,heading,temp,hum,pres,gas,lat,lon,alt,sats
2.1,-5,143,0.01,0,1,143,22.4,41,1012.8,52000,44.47,-73.21,61,9
```

Not all fields are required. You can start with just IMU data and add sensors later.

### Fields

| Field | Aliases | Units |
|-------|---------|-------|
| `roll` / `pitch` / `yaw` | `r` / `p` / `y` | degrees |
| `qw` / `qx` / `qy` / `qz` | — | quaternion |
| `gx` / `gy` / `gz` | `gyroX/Y/Z` | deg/s |
| `ax` / `ay` / `az` | `accelX` / `accel_x` | g |
| `mx` / `my` / `mz` | `magX/Y/Z` | µT |
| `heading` | `hdg` / `head` | degrees |
| `temp` | `temperature` / `t` | °C |
| `hum` | `humidity` / `rh` | % |
| `pres` | `pressure` / `baro` / `hPa` | hPa |
| `gas` | `voc` / `iaq` | Ω / kΩ |
| `lat` / `lon` | `latitude` / `longitude` | decimal degrees |
| `alt` | `altitude` | meters |
| `sats` | `satellites` | count |

**Orientation priority:** quaternion → roll/pitch/yaw → gyro integration

**Heading priority:** heading → yaw → magnetic calculation (mx/my)

## Repo structure

```
HERMES/
├── index.html      # dashboard
├── firmware/       # microcontroller code (not finished yet)
├── hardware/       # KiCad + PCB files (not finished yet)
└── README.md
```

## Firmware

The firmware runs on an ATmega328 at 8 MHz. It reads:

- ICM-20948 over I²C
- BME680 over I²C
- GPS over UART (NMEA)

Then it prints telemetry over USB serial in one of the supported formats.

This section will get updated once the final firmware is cleaned up and documented.

## License

Released under the **MIT License** — you're free to use, copy, modify, and distribute this project, including commercially, as long as the copyright notice and license text are included. See the [`LICENSE`](LICENSE) file for the full terms.
