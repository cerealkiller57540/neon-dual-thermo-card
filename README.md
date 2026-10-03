<div align="center">

# 🌡️ Neon Dual Thermometer Card

**Two neon thermometers side by side for Home Assistant, with real glass, liquid and a plasma core rendered in WebGL.**

[![HACS Custom][hacs-badge]][hacs-url]
[![Release][release-badge]][release-url]
[![Validate][validate-badge]][validate-url]
[![License: MIT][license-badge]][license-url]

[![Open your Home Assistant instance and open this repository in HACS.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=cerealkiller57540&repository=neon-dual-thermo-card&category=plugin)

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-dual-thermo-card/main/images/main.gif" alt="Neon Dual Thermometer Card: outdoor and living-room thermometers with a plasma core, rising bubbles and a 24 h history graph" width="448">

</div>

Outdoor on one side, indoors on the other, one card. Each thermometer is a glass tube that refracts what sits behind the card, filled with a liquid whose colour follows five temperature zones. The bulb holds a small plasma reactor whose arcs get more nervous as the temperature rises, and air bubbles climb faster when it is hot. Underneath, a 24 h history graph draws both curves, with an optional pressure trace in the background.

<img src="https://raw.githubusercontent.com/cerealkiller57540/neon-dual-thermo-card/main/images/variants.png" alt="The CSS card (left) and the WebGL card (right) with the same data, without any theme" width="800">

*Left: `neon-dual-thermo-card` (SVG/CSS). Right: `neon-dual-thermo-card-webgl`. Same data, no theme.*

## ✨ Features

- **Two cards in one install**
  - `neon-dual-thermo-card-webgl`: refracting glass, liquid, plasma and bubbles rendered by a WebGL shader (recommended).
  - `neon-dual-thermo-card`: lighter version, same layout, drawn in SVG/CSS.
- **Five colour zones** per thermometer, with your own thresholds, shared or per side.
- **Secondary readouts** under each thermometer: humidity, plus any sensor you like (pressure, illuminance, CO₂…) with its own label and unit.
- **History graph** of both temperatures over the last N hours, with an optional background pressure curve.
- **Optional centre column** for wind speed.
- **Tap and hold actions** per side (`more-info` by default).
- **Full visual editor**: every glass, liquid and plasma setting is a slider, no YAML needed.
- Pauses its animation when off-screen, releases its WebGL context when removed (Android WebViews cap a page at 8 contexts), and falls back to SVG if WebGL is not available.

## 📦 Installation

### HACS (recommended)

1. Click the **Open in HACS** button above, or add this repository as a custom repository in HACS (category **Dashboard**): `https://github.com/cerealkiller57540/neon-dual-thermo-card`.
2. Download **Neon Dual Thermometer Card**.
3. Reload your browser.

HACS registers one resource, `neon-dual-thermo-card.js`. It loads the WebGL variant on its own, so **do not** add `neon-dual-thermo-card-webgl.js` as a second resource.

### Manual

1. Copy both files from [`dist/`](dist) to `config/www/neon-dual-thermo-card/`.
2. Add a dashboard resource: URL `/local/neon-dual-thermo-card/neon-dual-thermo-card.js`, type **JavaScript module**.

## 🚀 Usage

Add a card from the dashboard editor and search for **Neon Dual Thermo**, or use YAML:

```yaml
type: custom:neon-dual-thermo-card-webgl
entity_left: sensor.outdoor_temperature
name_left: Outdoor
humidity_entity_left: sensor.outdoor_humidity
entity_right: sensor.living_room_temperature
name_right: Living room
humidity_entity_right: sensor.living_room_humidity
secondary_entity_right: sensor.living_room_pressure
secondary_label_right: PRESSURE
secondary_unit_right: hPa
entity_bg_pressure: sensor.outdoor_pressure    # optional, drawn behind the graph
```

## ⚙️ Options

Every per-side option exists as `…_left` and `…_right`.

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_left` / `entity_right` | string | **required** | Temperature sensors |
| `name_left` / `name_right` | string | `friendly_name` | Label above each thermometer |
| `humidity_entity_left` / `_right` | string | — | Humidity readout |
| `secondary_entity_left` / `_right` | string | — | Any extra sensor, shown under the humidity |
| `secondary_label_left` / `_right` | string | — | Its label |
| `secondary_unit_left` / `_right` | string | — | Its unit |
| `temp_min` / `temp_max` | number | `-20` / `40` | Scale of both tubes (also `temp_min_left`, `temp_max_right`…) |
| `unit` | string | `°C` | Unit shown after the value |
| `decimal_places` | number | `1` | Decimals |
| `zone_thresholds` | list | `[5, 15, 22, 28]` | Four limits between the five colour zones (also `zone_thresholds_left` / `_right`) |
| `color_zone1` … `color_zone5` | colour | theme | Zone colours, coldest to hottest (also per side) |
| `color_primary` / `color_secondary` | colour | theme | Lines and text, left / right |
| `color_background` | colour | theme | Tube interior |
| `show_history` | bool | `true` | History graph |
| `history_hours` | number | `24` | Its time span |
| `entity_bg_pressure` | string | — | Pressure sensor drawn behind the graph |
| `entity_wind` / `name_wind` / `wind_unit` | string | — / `VENT` / `km/h` | Optional centre column |
| `show_plasma` | bool | `true` | Plasma reactor in the bulbs |
| `color_plasma_ring1` / `color_plasma_ring2` | colour | auto | Plasma ring colours (also per side) |
| `plasma_saturation` | number | `1.8` | Plasma colour saturation |
| `animation_speed` | number | `1` | Global animation speed |
| `tap_action_left` / `_right` | action | `more-info` | Standard Home Assistant action |
| `hold_action_left` / `_right` | action | `none` | Standard Home Assistant action |
| `name_font_family`, `value_font_family`, `sensor_font_family` (and `…_font_size`) | string | theme | Fonts |
| `glitch_cat` | bool | `false` | A glitching cat walks along the coldest curve now and then (see FAQ) |
| `glitch_cat_image` / `glitch_cat_size` / `glitch_cat_chance` | string / number / number | — / `26` / `0.12` | Its image, height in px and chance per tick (~6 s) |

**WebGL card only**

| Option | Default | Description |
|---|---|---|
| `wgl_enabled` | `true` | `false` = draw the SVG thermometers instead |
| `wgl_liquid` | `profond` | Liquid style: `profond` (deep, coloured body) or `lumineux` (glowing, lets the background through) |
| `wgl_refract` | `10.5` | Glass lens strength, in px of background displacement |
| `wgl_clarity` | `1.00` | `0` = opaque tube like the SVG card, `1` = clear glass |
| `wgl_plasma` | `2.40` | Plasma arcs in the bulb |
| `wgl_bubbles` | `0.55` | Air bubble visibility |
| `wgl_bloom` | `1.75` | Glow around the liquid and bulb |

The other `wgl_*` settings (`chroma`, `fresnel`, `menisc`, `ripple`, `liq_body`, `liq_glow`, `sss`, `liq_transmit`, `liq_core`, `plasma_drive`, `orbits`, `bubble_count`, `bubble_size`, `bubble_speed`, `bubble_heat`, `bdeform`, `outline`) are easiest to tune from the visual editor, where each one is a slider with its range.

## ❓ FAQ

**Which languages are supported?** English and French. The editor and the card texts follow your Home Assistant language: French if it is French, English otherwise. Reload the page after changing the language.

**The glass looks flat.** The WebGL card refracts the background *behind* the card. On a plain dark theme there is little to bend; on a theme with a background image the effect is much stronger.

**Where is the cat?** `glitch_cat` is off by default and no image ships with the card. Point `glitch_cat_image` at your own transparent GIF or PNG (for example `/local/my-cat.gif` in `config/www/`).

**Some cards go blank on my Android phone.** Android WebViews keep at most 8 WebGL contexts per page and drop the oldest one. This card uses a single context for both thermometers. If you run many WebGL cards on one view, use `neon-dual-thermo-card` (SVG) or `wgl_enabled: false` on some of them.

**Which theme is in the screenshots?** Neo Tokyo, the author's own dark theme (not published). The card works with any theme.

## 🌃 More neon cards

This card is part of a family. See the full collection at [**Home-Assistant-Neon-Cards**](https://github.com/cerealkiller57540/Home-Assistant-Neon-Cards).

---

## 🐾 Support this project

If you enjoy these cards, please consider donating to **Quatre Pattes**, an animal rescue organization.

[![Sauver des animaux](https://img.shields.io/badge/🐾%20Sauver%20des%20animaux-Faire%20un%20don-ff69b4?style=for-the-badge)](https://don.quatre-pattes.org/s/?_jtsuid=70083177244599792679303)

> 💛 No need to support me — just help the animals. Thank you!

---

## 🤝 Contributing

1. Fork the repo
2. Create your branch: `git checkout -b feature/my-card`
3. Commit and push
4. Open a Pull Request

---

## 📄 License

[MIT License][license-url]

[hacs-badge]: https://img.shields.io/badge/HACS-Custom-orange.svg?style=for-the-badge
[hacs-url]: https://hacs.xyz
[release-badge]: https://img.shields.io/github/v/release/cerealkiller57540/neon-dual-thermo-card?style=for-the-badge
[release-url]: https://github.com/cerealkiller57540/neon-dual-thermo-card/releases
[validate-badge]: https://img.shields.io/github/actions/workflow/status/cerealkiller57540/neon-dual-thermo-card/validate.yml?branch=main&label=HACS&style=for-the-badge
[validate-url]: https://github.com/cerealkiller57540/neon-dual-thermo-card/actions/workflows/validate.yml
[license-badge]: https://img.shields.io/github/license/cerealkiller57540/neon-dual-thermo-card?style=for-the-badge
[license-url]: LICENSE
