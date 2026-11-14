# My Thermometer

A one-page web app for a Xiaomi LYWSD03MMC running the [pvvx ATC_MiThermometer](https://github.com/pvvx/ATC_MiThermometer) firmware.
It connects over Web Bluetooth and downloads the readings stored on the thermometer.

## Features
- Shows only the **last X days**. Set the number of days in the page. Settings are saved in your browser.
- Shows the current value, min/max/average and a temperature and humidity chart.
- Optional smoothing and battery line.
- Daily summary table and CSV export.
- Stores downloaded data in the browser (up to 60 days), so you can view it later without connecting.
- **Sync clock** button. History records need a correct device clock.

## Run on GitHub Pages
1. Create a new public repository on GitHub, e.g. `thermometer`.
2. Upload `index.html` (and this README).
3. Open **Settings → Pages → Build and deployment** and pick **Deploy from a branch**, then `main` / `(root)`.
4. After about a minute, open `https://<your-user>.github.io/thermometer/`.

Web Bluetooth needs HTTPS, which GitHub Pages provides. It needs Chrome or Edge on desktop or Android. On iPhone, use the Bluefy browser.

## Thermometer settings (in pvvx TelinkMiFlasher)
- History logging must be on. The "Averaging measurements" value sets how often a record is stored.
- If the thermometer has a PIN code set, remove it or connect once with the official flasher.

## Protocol (for hacking)
Service `0x1F10`, characteristic `0x1F1F` (write + notify).
- `35 cntL cntH skipL skipH` returns records `skip+1 … cnt`, newest first. Each notification has this layout:
  `35 idxL idxH | time u32 | temp s16 ×0.01 °C | hum u16 ×0.01 % | vbat u16 mV`. The 3-byte packet `35 00 00` ends the block.
- `23 t0 t1 t2 t3` sets the device clock (u32 seconds, little-endian).
