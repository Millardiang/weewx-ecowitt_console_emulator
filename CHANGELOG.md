# Changelog

All notable changes to weewx-ecowitt_console_emulator are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and
version numbers follow the rules in [VERSIONING.md](VERSIONING.md).

## [1.1.1] – 2026-10-15

### Added
- Square layout for near-square screens such as Apple HomeView (1088 × 1088), chosen
  automatically when the screen's width:height is between 0.8 and 1.25, and under
  Settings → Layout. It fills the whole screen, widening up to 5:4 like landscape.
  "Square" is in all 30 languages.
- Language, units, theme and layout can be set in the page address
  (`?lang=fr&units=metric&theme=auto&layout=landscape`), for screens where Settings
  can't be used or isn't kept, such as a page cast to a Google Nest Hub. The README
  explains casting the dashboard to a Nest Hub or Nest Hub Max with CATT.

### Changed
- New Auto theme, now the default: Light from sunrise to sunset and Black from sunset
  to sunrise, switching by itself at the station's sunrise and sunset. The themes it
  uses are set by `theme_day` and `theme_night` in `skin.conf`. A device that already
  chose a theme keeps it; Settings → Theme → Auto (or the theme button) switches to it.

### Fixed
- On screens wider than 16:10 the landscape dashboard now fills the whole width (up to
  2.5:1) instead of leaving bands at the sides: Amazon Echo Show in the Silk browser
  (Show 5, 8, 10, 15, 21), 16:9 tablets, and browsers whose address bar shortens the
  screen. The extra width goes between the panels; the gauges keep their shape.
- The dashboard could sit at the left with empty space to the right in browsers on
  older engines (Silk on some Echo Shows): the full-screen box no longer relies on CSS
  `inset`, the screen size is measured from that box, and the page fits itself again
  after loading and whenever the visible area changes, also when the browser doesn't
  report it. Settings shows the layout and screen size the page sees.

## [1.1.0] – 2026-10-14

### Added
- Indoor temperature chart on every Charts tab (`inTemp`, mappable in `[[FieldMap]]`):
  the average on the Day, Week and Month tabs, the daily high and low on the Year tab.
- The installer asks whether to show VPD (vapour pressure deficit): yes shows the VPD
  reading and its chart as before, no leaves both out. `--vpd=y|n` answers it for
  unattended installs; the answer is stored as `show_vpd` and offered again on
  reinstall. Existing installations keep VPD.
- "Indoor temperature" in all 30 languages.

### Changed
- The readings row spreads its readings evenly whatever their number (six when VPD
  is not shown).
- Installing removes the calendar-year chart files (`chart_2025.json` ...), so WeeWX
  writes them again with the new charts; before, a year's file was never rewritten.
- The VPD chart is always in the chart files and left out by the page when VPD is
  not shown, so changing `show_vpd` takes effect at once on every tab and year.

### Fixed
- With one rain gauge, long words in some languages ran into the rain totals or out
  of the panel: the rate line and the total labels are now fitted to their space,
  and the Czech, Finnish, Hungarian, Norwegian and Tamil labels there are shorter.
- Labels in the readings row could be cut short instead of going onto two lines.

## [1.0.0] – 2026-10-07

First public release. A WeeWX 5 skin and extension that turns a tablet, phone or
browser into a full-screen emulation of the Ecowitt WH2560/HP2560 console.
Released under the GNU General Public License, version 3 or later (see LICENSE).

### Dashboard
- Console-style layout: outdoor temperature ring, wind dial, a temperature/humidity
  channel pair (Indoor and WN31 CH1–8), soil moisture and leaf wetness, lightning,
  leak indicators, PM2.5 and CO₂, barometer with 3-hour trend and pressure-tendency
  forecast, sun arc with a moon track, moon phase, and a readings row that includes
  feels-like, dew point, VPD (kPa), 10-minute average wind, max daily gust and Beaufort.
- Wind gauge with a blue arrow for the current direction and a grey arrow for the
  10-minute average direction.
- Rain section for a tipping gauge, a piezo gauge or both, with daily, rate, event,
  hourly, weekly, monthly and yearly totals.
- Signal bars next to each reading. A battery icon appears only when a battery is low.
- Three layouts chosen from the screen's shape: landscape 1280 × 800, portrait
  800 × 1518 and phone 540 wide. Settings → Layout can force one of them.
- Three themes: Navy (console), Black (OLED / night) and Light.
- All times are always shown in the station's time zone, whatever the time zone of
  the viewing device. The zone is read from the WeeWX machine and can be set in
  `skin.conf` (`timezone`) if that comes out wrong.

### Charts
- Full-screen Day, Week, Month and Year charts drawn in the browser, following the
  theme and display units, with tooltips by mouse or touch.
- Temperature, humidity, VPD, wind, wind direction (Day tab), rain, barometer,
  solar radiation and UV.
- One rain chart with a separate column per gauge in each interval (tipping blue,
  piezo violet). The tooltip shows both values and the difference, and the legend
  shows the same period totals as the rain table.
- The Year tab shows the last 12 calendar months, or any calendar year in the skin's
  database (1 January to 31 December), chosen from a list. Each year is a separate
  file (`chart_2025.json` ...); past years are written once, the current year at most
  once an hour.

### Panels
- Windy radar centred on the station, in a panel over the dashboard or as a link.
- Sensors panel with battery and signal for every sensor that reports them.
- Settings: language, units, layout, theme, 12/24-hour clock, seconds, keep screen awake,
  full screen on touch, and data status.
- Every panel opens inside the dashboard, so the page never leaves full screen.

### Languages
- Everything on the dashboard can be shown in 30 languages, the same as weewx-divumwx:
  Arabic, Basque, Breton, Catalan, Chinese (simplified), Czech, Danish, Dutch, English,
  English (US), Finnish, French, German, Greek, Hindi, Hungarian, Icelandic, Italian,
  Japanese, Norwegian, Polish, Portuguese, Spanish, Swedish, Tamil, Thai, Turkish,
  Ukrainian, Urdu and Welsh.
- Chosen under Settings → Language, with a default set by `language` in `skin.conf`.
  The choice is shared with weewx-divumwx through the `dashboardLanguage` browser
  setting when both are served from the same web server.
- Day and month names follow the language. Where weewx-divumwx already translates
  the same text, its wording is used.
- Labels that would not fit their place in a language are made smaller, or put on two
  lines, to fit. In Arabic and Urdu each label reads right to left.

### Units
- Data feeds are always written in METRICWX and converted in the browser to the
  DivumWX presets: uk, us, metric, scandi, canada and icao.
- The unit choice is shared with weewx-divumwx through the `dashboardUnitSystem`
  browser setting when both are served from the same web server.

### Data
- The skin keeps its own database (`ecowitt_console_emulator.sdb`, binding
  `ecce_binding`) and never changes the station's main database.
- The skin's database is created only if missing, with the main database's schema
  plus extra Ecowitt fields. It is filled with history from the main database
  (400 days by default), then gaps are filled at each start and every new archive
  record is written to it.
- Piezo rain history is read from `p_rain` and from `hail`, so records copied from
  the main database keep their piezo rain.
- Live data every few seconds from the skin's own `live.json`, or from
  weewx-EcowittGateway's `ecwLoop.json` when that driver is installed and chosen.
- Missing driver fields are worked out from the archive: feels-like, 10-minute wind
  direction, max daily gust, VPD and period rain totals.

### Installation
- `weectl extension install` installer that asks which rain sensor(s) the station
  has and, only when weewx-EcowittGateway is installed, where live data comes from.
- Unattended installs with `--yes --rain=tipping|piezo|both --live=driver|skin`.
- weewx-EcowittGateway is optional; the skin works with any driver for an Ecowitt
  gateway or console.

[Unreleased]: https://github.com/Millardiang/weewx-ecowitt_console_emulator/compare/v1.1.1...HEAD
[1.1.1]: https://github.com/Millardiang/weewx-ecowitt_console_emulator/compare/v1.1.0...v1.1.1
[1.1.0]: https://github.com/Millardiang/weewx-ecowitt_console_emulator/compare/v1.0.0...v1.1.0
[1.0.0]: https://github.com/Millardiang/weewx-ecowitt_console_emulator/releases/tag/v1.0.0

---

Copyright (c) Ian Millard 2026. GNU General Public License v3 or later; see [LICENSE](LICENSE).
