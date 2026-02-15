# CYD Olympic Hockey Scoreboard (Canada Men)

ESP32-2432S028 (CYD) scoreboard for **Milano Cortina 2026 men's hockey**.
<img width="1390" height="740" alt="MenGameDay" src="https://github.com/user-attachments/assets/18246849-23fa-4610-9b5d-81a0d01776e3" /><img width="567" height="403" alt="Live" src="https://github.com/user-attachments/assets/829a70c0-8469-44c8-ad5b-37ab9681227f" />


## What it does

- Uses ESPN Olympic men's hockey JSON feed
- Focuses on `CAN` (Team Canada men)
- Auto-selects event priority:
  - in-progress Canada game
  - else next scheduled Canada game (countdown)
  - else most recent completed Canada game
- Screens:
  - `NEXT_GAME` (merged no-game + pre-game)
  - `LIVE`
  - `INTERMISSION`
  - `FINAL`
  - `GOAL` (when detectable from summary plays)
  - `LAST_GAME`
  - `STANDINGS` (group tables)
- Builds group standings from completed Preliminary Round games
- Loads country flags from SPIFFS (`/flags/...`), with runtime URL cache fallback
- Optional anthem playback at puck drop transition (`pre -> in`)

## Locked build environment

Use this exact env (pinned platform/libs):

```powershell
pio run -e esp32-cyd-sdfix
```

Upload firmware:

```powershell
pio run -e esp32-cyd-sdfix -t upload
```

Upload SPIFFS assets:

```powershell
pio run -e esp32-cyd-sdfix -t uploadfs
```

## Config

Edit `include/config.h`:

- Wi-Fi credentials
- `FOCUS_TEAM_ABBR` (default `CAN`)
- `TZ_INFO` for local countdown display
- `ANTHEM_DAC_PIN` (default `25`)

## Flags in SPIFFS

Preferred paths:

- `/flags/56/CAN.png`
- `/flags/64/CAN.png`
- `/flags/96/CAN.png`
- optional canonical fallback `/flags/CAN.png`

Generate/download flags from ESPN:

```powershell
python tools/fetch_flags.py
```

Then upload SPIFFS:

```powershell
pio run -e esp32-cyd-sdfix -t uploadfs
```

## Data source

Primary schedule/scoreboard endpoint:

- `https://site.api.espn.com/apis/site/v2/sports/hockey/olympics-mens-ice-hockey/scoreboard?dates=YYYYMMDD-YYYYMMDD`

Optional detail endpoint used for stats/goal detection:

- `https://site.api.espn.com/apis/site/v2/sports/hockey/olympics-mens-ice-hockey/summary?event=<eventId>`

## Audio

See `README_AUDIO.md` for WAV format and upload instructions.

## Support

If you enjoy what I’m making and want to support more late-night builds, experiments, and ideas turning into reality, it's genuinely appreciated.

- https://buymeacoffee.com/zerocypherxiii

