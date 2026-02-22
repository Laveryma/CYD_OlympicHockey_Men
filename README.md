# CYD Olympic Hockey Scoreboard (Men)

ESP32-2432S028 (CYD) scoreboard for **Milano Cortina 2026 men's hockey**. Current default Canada, but easily switch to any favourite team.
<img width="1390" height="740" alt="MenGameDay" src="https://github.com/user-attachments/assets/18246849-23fa-4610-9b5d-81a0d01776e3" /><img width="567" height="403" alt="Live" src="https://github.com/user-attachments/assets/829a70c0-8469-44c8-ad5b-37ab9681227f" />


## What it does

- Uses ESPN Olympic men's hockey JSON feed
- Default set to docuses on `CAN` (Team Canada men) <---easily changed to any participating nation, in include/config.h
- Auto-selects event priority:
  - in-progress Canada (or user defined nation) game
  - else next scheduled Canada (or user defined nation) game (countdown)
  - else most recent completed Canada (or user defined nation) game
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

- Update Wi-Fi credentials
- `FOCUS_TEAM_ABBR` (default `CAN`, or update to favorite country NOC code, e.g. CAN, USA, NOR, CZE, etc.)
- `TZ_INFO` for local countdown display
- `ANTHEM_DAC_PIN` (default `25`)

## Flags in SPIFFS

Preferred paths:

- `/flags/56/CAN.png`
- `/flags/64/CAN.png`
- `/flags/96/CAN.png`
- optional canonical fallback `/flags/CAN.png`
*If favourite nation flag is not listed in 'data/flags/', run the included fetch flags tool (tools/fetch_flags.py) or find your own flag image, resize to 56px, 64px, 96px, then save to appropriate data/flags folder as <NOC>.png

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

If you enjoy what I’m making and want to support more late-night builds, experiments, and random ideas turning into reality, it's genuinely appreciated.

- https://buymeacoffee.com/zerocypherxiii





