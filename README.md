# Weather Display — ESPHome Configs

ESPHome YAML configurations for ESP32-based weather displays driven by Home Assistant sensors (Weather Underground via Wunderground integration).

## Configs

| File | Hardware | Display | Resolution |
|------|----------|---------|------------|
| `weather-eink.yaml` | ESP32 DevKit | Waveshare 7.5" V2 e-paper | 800×480 |
| `freenove-display.yaml` | ESP32-S3 DevKitC-1 | ILI9341 TFT (Freenove) | 320×240 |
| `weather-oled.yaml` | Seeed XIAO ESP32-C6 | ILI9341 TFT | 320×240 |

---

## weather-eink.yaml

E-ink (e-paper) weather display on a **Waveshare 7.5" V2** (800×480, black/white).

### Features

- Large current temperature, humidity, and wind speed/direction in header
- 5-day forecast with MDI weather icons, high/low temps, and precipitation %
- Bottom ribbon with sunrise/sunset times, barometric pressure, and pressure trend
- Last-updated timestamp
- 5-minute refresh interval (appropriate for e-ink to avoid ghosting)

### Wiring (ESP32 DevKit → Waveshare 7.5" V2)

| Display Pin | ESP32 Pin |
|-------------|-----------|
| VCC         | 3.3V      |
| GND         | GND       |
| CLK         | GPIO18    |
| DIN (MOSI)  | GPIO23    |
| CS          | GPIO5     |
| DC          | GPIO17    |
| RST         | GPIO16    |
| BUSY        | GPIO4     |

### Home Assistant Sensors Required

All sensors are pulled from a Weather Underground integration (`kianevad37_*`). Adjust `entity_id` values to match your HA instance.

- `sensor.kianevad37_temperature` — current temperature (°F)
- `sensor.kianevad37_relative_humidity`
- `sensor.kianevad37_wind_speed`
- `sensor.kianevad37_wind_direction_cardinal`
- `sensor.kianevad37_pressure` — barometric pressure (inHg)
- `sensor.kianevad37_weather_summary_0..4` — condition text for 5 days
- `sensor.kianevad37_forecast_temperature_2d..8d` — forecast highs
- `sensor.kianevad37_forecast_temperature_1n..9n` — forecast lows/overnight
- `sensor.kianevad37_precipitation_probability_0d..8d` — precip % per day
- `sensor.sun_next_dawn` / `sensor.sun_next_dusk` — sunrise/sunset (ISO UTC)

### Secrets

Add to your `secrets.yaml`:

```yaml
wifi_ssid: "YourNetwork"
wifi_password: "YourPassword"
fallback_password: "YourFallback"
ota_password: "YourOTAPassword"
weather_display_api_key: "base64-encoded-32-byte-key"
```

---

## freenove-display.yaml

TFT color weather display on an **ILI9341** screen (320×240 landscape) using an ESP32-S3 DevKitC-1. Includes backlight PWM with automatic dawn/dusk dimming.

## weather-oled.yaml

Minimal ILI9341 TFT config for the Seeed XIAO ESP32-C6 — test card only, intended as a bring-up/hardware-test config.

---

## Shared Assets

- `materialdesignicons-webfont.ttf` — MDI icon font; only the weather glyphs referenced in each config are compiled into the firmware.
