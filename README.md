# Weather Display — ESPHome Configs

ESPHome YAML configurations for ESP32-based weather displays driven by Home Assistant sensors (Weather Underground via Wunderground integration).

## Configs

| File | Hardware | Display | Resolution |
|------|----------|---------|------------|
| `eink/weather-eink.yaml` | Freenove ESP32-S3 WROOM | Waveshare/GoodDisplay 4.2" e-paper (SSD1683) | 400×300 |
| `tft/freenove-display.yaml` | Freenove ESP32-S3 WROOM | ILI9341 TFT | 320×240 |
| `oled/weather-oled.yaml` | Seeed XIAO ESP32-C6 | ILI9341 TFT | 320×240 |

The MDI icon font (`tft/materialdesignicons-webfont.ttf`) is shared by both active configs — only the weather glyphs referenced in each config are compiled into firmware.

---

## eink/weather-eink.yaml

E-ink weather display on a **GoodDisplay GDEY042T81** (Waveshare 4.2" compatible, 400×300, black/white, SSD1683 controller).

### Features

- Large current temperature, humidity, and wind speed/direction in header
- 5-day forecast with MDI weather icons, forecast high/low temps, and precipitation %
- Current condition summary text below the forecast grid
- Bottom ribbon with sunrise/sunset times, barometric pressure, and pressure trend
- Last-updated timestamp (America/Chicago timezone)
- 5-minute refresh interval (appropriate for e-ink to avoid ghosting)

### Wiring (Freenove ESP32-S3 WROOM → Waveshare 4.2" e-paper)

| Display Pin | ESP32-S3 Pin |
|-------------|--------------|
| VCC         | 3.3V         |
| GND         | GND          |
| CLK         | GPIO12       |
| DIN (MOSI)  | GPIO11       |
| CS          | GPIO10       |
| DC          | GPIO9        |
| RST         | GPIO8        |
| BUSY        | GPIO4        |

### Home Assistant Sensors Required

All sensors are pulled from a Weather Underground integration (`kianevad37_*`). Adjust `entity_id` values to match your HA instance.

- `sensor.kianevad37_temperature` — current temperature (°F)
- `sensor.kianevad37_relative_humidity`
- `sensor.kianevad37_wind_speed`
- `sensor.kianevad37_wind_direction_cardinal`
- `sensor.kianevad37_pressure` — barometric pressure (inHg)
- `sensor.kianevad37_weather_summary_0..4` — condition text for 5 days
- `sensor.kianevad37_forecast_temperature_0d` — today's forecast high
- `sensor.kianevad37_forecast_temperature_2d..8d` — days 2–5 forecast highs
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

## tft/freenove-display.yaml

TFT color weather display on an **ILI9341** screen (320×240 landscape) using a Freenove ESP32-S3 WROOM. Includes backlight PWM with automatic dawn/dusk dimming.

### Features

- Current temperature, humidity, wind speed/direction in header bar
- 5-day forecast with dynamic day-of-week labels, MDI weather icons, forecast high/low temps, and precipitation %
- Bottom ribbon with sunrise/sunset times, barometric pressure, and pressure trend
- Auto-dimming to 50% between dusk and dawn

### Wiring (Freenove ESP32-S3 WROOM → ILI9341)

| Display Pin | ESP32-S3 Pin |
|-------------|--------------|
| VCC         | 3.3V         |
| GND         | GND          |
| SCK         | GPIO12       |
| MOSI        | GPIO11       |
| MISO        | GPIO13       |
| CS          | GPIO10       |
| DC          | GPIO9        |
| RST         | GPIO8        |
| LED         | GPIO14       |

---

## oled/weather-oled.yaml

Minimal ILI9341 TFT config for the Seeed XIAO ESP32-C6 — bring-up/hardware-test config only.
