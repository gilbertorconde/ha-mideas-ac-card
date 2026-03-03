# Midea AC card for lovelace

`custom_card_gil_ac` is a standalone JavaScript Lovelace card that replicates the look and feel of the official **Midea app** for Home Assistant `climate` entities.

> **Required integration:** This card is designed specifically for the [**Midea Smart AC**](https://github.com/mill1000/midea-ac-py) custom integration by [@mill1000](https://github.com/mill1000), which controls Midea (and associated brands — Carrier, Toshiba, Samsung, LG, Electrolux, and many more) air conditioners via LAN. Install it via HACS or manually before using this card.

Key features at a glance:

- **Draggable semicircular thermostat arc** — drag the handle or tap +/− to set the target temperature in 0.5 °C steps; arc colour follows the active HVAC mode.
- **Stats bar** — outdoor temperature, indoor temperature, and humidity (all optional; auto-derived from the entity name).
- **2 × 2 tile grid** — Mode, Fan Speed, Airflow, and Breeze & Presets tiles; each opens a bottom-sheet panel that auto-scrolls into view when needed.
- **Mode sheet** — one button per HVAC mode with inline SVG icons; mode list is read dynamically from `hvac_modes`.
- **Fan Speed sheet** — Auto toggle + numeric slider (0–100 %) linked to a companion `number` entity.
- **Airflow sheet** — Up/Down and Left/Right oscillation pills; two rows of 5 louver-position buttons with inline SVG indicators showing the active position in the mode accent colour. Angle selectors are disabled while the corresponding oscillation axis is active.
- **Breeze & Presets sheet** — Breeze-mode pills (Normal / Breeze Away / Breezeless), Output Rate pills (Full / 75 % / 50 %), and preset-mode pills (Sleep / Boost / Away); all with inline SVG icons. Breeze and rate controls are available in Cool mode only and are automatically greyed out otherwise.
- **Feature rows** — iECO toggle (uses the `eco` preset), LED Display switch, Purifier switch.
- **Self-clean button** — small pill in the header that triggers self-clean; pulses while cleaning is in progress (works even while the AC is off).
- **Power button** — prominent power button at the bottom; remembers the last active mode on power-on.
- **Mode-adaptive accent colour** — a single CSS custom property (`--mode-color`) drives all accents; adapts automatically to Cool / Heat / Dry / Fan / Auto / Off.
- **Zero external dependencies** — single vanilla-JS file, no HACS frontend card required.
- **Auto-derived companion entities** — companion entity IDs are generated automatically from the climate entity name; only override the ones that differ.

## Screenshots

<table>
<tr>
  <td align="center"><b>Off</b></td>
  <td align="center"><b>Cool mode</b></td>
  <td align="center"><b>Heat mode</b></td>
</tr>
<tr>
  <td><img src="docs/screenshot-off.png" width="160"/></td>
  <td><img src="docs/screenshot-cool.png" width="160"/></td>
  <td><img src="docs/screenshot-heat.png" width="160"/></td>
</tr>
<tr>
  <td align="center"><b>Mode sheet</b></td>
  <td align="center"><b>Fan Speed sheet</b></td>
  <td align="center"><b>Airflow sheet</b></td>
</tr>
<tr>
  <td><img src="docs/screenshot-mode-sheet.png" width="160"/></td>
  <td><img src="docs/screenshot-fan-sheet.png" width="160"/></td>
  <td><img src="docs/screenshot-airflow-sheet.png" width="160"/></td>
</tr>
</table>

## Credits

Author: [gil](https://github.com/condegil) — 2026  
Version: 2.0.0

## Changelog

<details>
<summary>Versions</summary>

**2.0.0**  
Complete rewrite as a self-contained vanilla-JS custom element.  
Renamed to `midea-ac-card.js`, now installable via HACS.  
Added draggable thermostat arc, bottom-sheet panels, inline SVG icons for modes / presets / breeze modes / louver indicators, fan-speed slider, self-clean button, mode-adaptive theming, auto-derived entity IDs, and smart scroll-into-view for sheets.

**1.0.0**  
Initial release (button-card YAML approach).

</details>

---

## Installation

### Option A — HACS (recommended)

1. In Home Assistant, open **HACS → Frontend**.
2. Click the three-dot menu (⋮) in the top-right and choose **Custom repositories**.
3. Paste `https://github.com/gilbertorconde/lovelace-midea-ac-card` and select **Dashboard** as the category.
4. Click **Add**, then search for **Midea AC Card** and install it.
5. Reload your browser / clear the cache.

[![Add to HACS](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=gilbertorconde&repository=lovelace-midea-ac-card&category=dashboard)

### Option B — Manual

1. Download `midea-ac-card.js` from the [latest release](https://github.com/gilbertorconde/lovelace-midea-ac-card/releases/latest).
2. Copy it to `/config/www/lovelace-midea-ac-card/midea-ac-card.js`.
3. Add the following under **Settings → Dashboards → Resources** or in `ui-lovelace.yaml`:

```yaml
resources:
  - url: /local/lovelace-midea-ac-card/midea-ac-card.js
    type: module
```

### Add the card to your dashboard

```yaml
type: custom:custom-card-gil-ac
entity: climate.living_room_ac
```

All companion entity IDs are auto-derived from the climate entity name (see [Auto-derived entities](#auto-derived-entities)) so the minimal config above is usually all you need.

---

## Configuration

| Variable        | Required | Default | Description                                                     |
| --------------- | -------- | ------- | --------------------------------------------------------------- |
| `entity`        | ✅       | —       | Main `climate.*` entity                                         |
| `indoor_temp`   | ☑        | auto    | `sensor.*` for indoor temperature                               |
| `outdoor_temp`  | ☑        | auto    | `sensor.*` for outdoor temperature                              |
| `fan_speed`     | ☑        | auto    | `number.*` for fan speed percentage (0–100 %)                   |
| `display`       | ☑        | auto    | `switch.*` for LED display on/off                               |
| `breeze_away`   | ☑        | auto    | `switch.*` for Breeze Away mode                                 |
| `breezeless`    | ☑        | auto    | `switch.*` for Breezeless mode                                  |
| `purifier`      | ☑        | auto    | `switch.*` for air purifier                                     |
| `swing_h_angle` | ☑        | auto    | `select.*` for horizontal louver angle (`off`, `pos_1`–`pos_5`) |
| `swing_v_angle` | ☑        | auto    | `select.*` for vertical louver angle (`off`, `pos_1`–`pos_5`)   |
| `rate_select`   | ☑        | auto    | `select.*` for output rate (`off`, `gear_75`, `gear_50`)        |

✅ = required · ☑ = optional (auto-derived or safely hidden when omitted)

All optional properties are safely ignored when omitted — their corresponding UI sections are hidden or disabled automatically.

### Minimal example

```yaml
type: custom:custom-card-gil-ac
entity: climate.living_room_ac
```

### Full example (Midea AC LAN integration)

```yaml
type: custom:custom-card-gil-ac
entity: climate.living_room_ac
indoor_temp: sensor.living_room_ac_indoor_temperature
outdoor_temp: sensor.living_room_ac_outdoor_temperature
fan_speed: number.living_room_ac_fan_speed
display: switch.living_room_ac_display
breeze_away: switch.living_room_ac_breeze_away
breezeless: switch.living_room_ac_breezeless
purifier: switch.living_room_ac_purifier
swing_h_angle: select.living_room_ac_horizontal_swing_angle
swing_v_angle: select.living_room_ac_vertical_swing_angle
rate_select: select.living_room_ac_rate_select
```

### Overriding a single entity

Supply only the keys whose names differ from the Midea AC LAN defaults. Set a key to `null` to disable that feature entirely:

```yaml
type: custom:custom-card-gil-ac
entity: climate.living_room_ac
purifier: null # hide the purifier row
display: switch.my_custom_display_entity
```

---

## Auto-derived entities

When a companion entity is not explicitly set in the card config, the card derives its ID automatically from the climate entity name using the Midea AC LAN integration's standard naming convention.

For `entity: climate.living_room_ac` the derived IDs are:

| Key                 | Derived entity ID                              |
| ------------------- | ---------------------------------------------- |
| `indoor_temp`       | `sensor.living_room_ac_indoor_temperature`     |
| `outdoor_temp`      | `sensor.living_room_ac_outdoor_temperature`    |
| `fan_speed`         | `number.living_room_ac_fan_speed`              |
| `display`           | `switch.living_room_ac_display`                |
| `breeze_away`       | `switch.living_room_ac_breeze_away`            |
| `breezeless`        | `switch.living_room_ac_breezeless`             |
| `purifier`          | `switch.living_room_ac_purifier`               |
| `swing_h_angle`     | `select.living_room_ac_horizontal_swing_angle` |
| `swing_v_angle`     | `select.living_room_ac_vertical_swing_angle`   |
| `rate_select`       | `select.living_room_ac_rate_select`            |
| `self_clean_sensor` | `binary_sensor.living_room_ac_self_clean`      |
| `self_clean_btn`    | `button.living_room_ac_start_self_clean`       |

If a derived entity is not found in HA states the corresponding UI element is automatically hidden or disabled.

---

## UI layout

```
┌──────────────────────────────────────────┐
│  Living Room AC       [✦] ● Cool         │  ← header: name, self-clean pill, mode chip
│                                          │
│           ╭──────────────────╮           │
│          ╱   ◉ (drag handle)  ╲          │  ← draggable SVG arc (track + mode fill)
│   [−]   │       22.5 °C        │   [+]   │  ← +/− buttons
│          ╲                    ╱          │
│           ╰──────────────────╯           │
│     ☁ 16.3°      🏠 23.2°      💧 48%    │  ← stats bar
├──────────────────────────────────────────┤
│  ┌────────────────┐ ┌────────────────┐   │
│  │ Mode           │ │ Fan Speed      │   │  ← tiles (tap → bottom sheet)
│  │ ❄ Cool         │ │ Auto        ⟳  │   │
│  └────────────────┘ └────────────────┘   │
│  ┌────────────────┐ ┌────────────────┐   │
│  │ Airflow        │ │ Breeze & …     │   │
│  │ H2 V3       ↗  │ │ 〰 💤          │   │
│  └────────────────┘ └────────────────┘   │
├──────────────────────────────────────────┤
│  ♻  iECO   Improves efficiency    [●]    │  ← feature rows
│  🖥  LED Display                   [○]    │
│  🌿  Purifier                     [○]    │
├──────────────────────────────────────────┤
│                  [ ⏻ ]                   │  ← power button
└──────────────────────────────────────────┘
```

### Bottom sheets

Each tile opens a panel that slides up from the bottom of the card. If the sheet is not fully visible, the page scrolls automatically to align the sheet bottom with the viewport bottom.

| Sheet                | Contents                                                                                                                                                                                                                                                               |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Mode**             | One button per HVAC mode with inline SVG icon; list is read from `hvac_modes`. Active mode is highlighted.                                                                                                                                                             |
| **Fan Speed**        | Auto toggle row; when Auto is off a range slider controls the `fan_speed` number entity (0–100 %).                                                                                                                                                                     |
| **Airflow**          | Up/Down and Left/Right oscillation pills → `climate.set_swing_mode`. Two rows of 5 louver-position buttons with inline SVG icons; active position highlighted in mode colour. Rows are dimmed while the corresponding oscillation axis is active.                      |
| **Breeze & Presets** | Breeze-mode pills (Normal / Breeze Away / Breezeless) and Output Rate pills (Full / 75 % / 50 %) — available in Cool mode only. Preset pills (Sleep / Boost / Away) — availability follows `preset_modes` from the climate entity. All pills include inline SVG icons. |

---

## Mode colour theming

| HVAC mode  | Accent colour      |
| ---------- | ------------------ |
| `cool`     | `#00a0e9` (blue)   |
| `heat`     | `#ff8c00` (orange) |
| `dry`      | `#00bcd4` (teal)   |
| `fan_only` | `#43a047` (green)  |
| `auto`     | `#7b1fa2` (purple) |
| `off`      | `#757575` (grey)   |

The accent colour is applied to: thermostat arc fill, tile left-border, active pill buttons, feature-row toggle background, sheet active elements, and the fan-speed slider thumb.

---

## Airflow louver icons

All louver SVG path data is embedded directly in the JavaScript — no external files are served. Each icon shows:

- A dark circular background
- The louver body
- 5 directional fins (pos_1 … pos_5); the active position's fin is drawn in the mode accent colour, the rest in `#888`

A `−` button at the left of each row resets the position to `off`.

Oscillation takes priority — when an oscillation axis is active, its louver row is dimmed and non-interactive until oscillation is turned off.

---

## Preset mode behaviour

Preset availability is dynamic and depends on the current HVAC mode. The card reads `attributes.preset_modes` from the climate entity on every update; buttons are automatically enabled or greyed out as the mode changes.

- The **iECO** feature row uses the `eco` preset.
- **Sleep**, **Boost**, and **Away** presets appear in the Breeze & Presets sheet; they are greyed out when not listed in `preset_modes` for the current mode.
- Tapping an active preset toggles it off (sets `preset_mode: none`).

---

## Breeze & Presets tile

The tile always shows the current state as icons side by side:

- In Cool mode: the active breeze-mode icon (Normal / Breeze Away / Breezeless).
- In any mode: the active preset icon (Sleep / Boost / Away), if one is set.
- Defaults to Normal breeze icon when no special mode is active.

---

## Self-clean

The self-clean button appears as a small pill in the card header. It:

- Works while the AC is **on or off**.
- Calls `button.press` on the `self_clean_btn` entity.
- Shows a **pulsing animation** while the `self_clean_sensor` binary sensor reports cleaning in progress.
- Is hidden entirely when neither entity exists.

---

## Requirements

- Home Assistant 2023.4 or newer (modern Web Component support)
- The [**Midea Smart AC**](https://github.com/mill1000/midea-ac-py) custom integration installed and at least one `climate.*` entity configured
  - Install via HACS (search for "Midea Smart AC") or manually by copying `custom_components/midea_ac` into your HA config directory
  - Supports Midea OEM brands: Carrier, Toshiba, Samsung, LG, Electrolux, Friedrich, Pioneer, Mr. Cool, and [many more](https://github.com/mill1000/midea-ac-py#readme)
- No additional HACS **frontend** cards required
