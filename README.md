# ESPHome Dotmatrix Clock

![Matrix rain forming the time](docs/dotmatrix-clock/22-matrix-3-cas.svg)

A clock for a 32 × 8 dot-matrix display built from four MAX7219 modules, driven by an ESP8266 (esp12e). The whole configuration is in [`dotmatrix-clock.yaml`](dotmatrix-clock.yaml).

- **Wiring:** SPI CLK = GPIO14, MOSI = GPIO5, CS = GPIO0.
- **Time:** SNTP (`pool.ntp.org`, Europe/Bratislava), re-synced every 15 minutes. The panel redraws every 50 ms, so the seconds change within tens of milliseconds of the real second.
- **Fonts:** `tom-thumb.bdf` (3×5, MIT) is included, ESPHome downloads Micro 5 from Google Fonts, and `MobileFont.ttf` has to be downloaded separately (see Installation).
- **Home Assistant:** connects over the native API; styles, the timer, dice and breathing exercise are exposed as entities.

Pick a style with the **Clock Style** entity in Home Assistant or in the device's web interface (port 80). The last choice survives a power loss.

## Styles

| # | Style | Preview |
|---|---|---|
| 0 | **Tom Thumb HH:MM:SS** – 3×5 digits, all the same width | ![](docs/dotmatrix-clock/00-tom-thumb.svg) |
| 1 | **Micro 5 HH:MM:SS** – narrower colons | ![](docs/dotmatrix-clock/01-micro5.svg) |
| 2 | **Seconds Bar Top** – fills up until :30, then empties from the left | ![](docs/dotmatrix-clock/02-pasik-hore.svg) |
| 3 | **Seconds Bar Bottom** | ![](docs/dotmatrix-clock/03-pasik-dole.svg) |
| 4 | **Original** – a dash in the top row moves every two seconds | ![](docs/dotmatrix-clock/04-povodne.svg) |
| 5 | **Binary Clock** – 8-4-2-1 columns for HH MM SS | ![](docs/dotmatrix-clock/05-binarne.svg) |
| 6 | **Rolling Digits** – a changing digit rolls up like a mechanical counter | ![](docs/dotmatrix-clock/06-rolujuce.svg) |
| 7 | **Time and Outdoor Temperature** – 7 s time, 3 s temperature from `weather.domov` | ![](docs/dotmatrix-clock/07-teplota.svg) |
| 8 | **Time and Weekday** – Monday to Sunday markers, today fully lit | ![](docs/dotmatrix-clock/08-den-v-tyzdni.svg) |
| 9 | **Hourglass Seconds** – one grain of sand drops onto a pile every second | ![](docs/dotmatrix-clock/09-presypacie.svg) |
| 10 | **Time Falls Apart** – from :50 the digits turn into Conway's Game of Life | ![](docs/dotmatrix-clock/10-rozpad.svg) |
| 11 | **Pac-Man Eats the Minute** – on every new minute Pac-Man eats the old HH:MM | ![](docs/dotmatrix-clock/11-pacman.svg) |
| 12 | **Live Weather** – animated icon based on `weather.domov` | ![](docs/dotmatrix-clock/12-pocasie.svg) |
| 13 | **Analog Watch** – 8×8 dial with hands and a seconds dot running around the edge | ![](docs/dotmatrix-clock/13-rucickove.svg) |
| 14 | **Time Emerges from the Rain** – digit pixels stick as drops pass through them | ![](docs/dotmatrix-clock/14-dazd.svg) |
| 15 | **The Day in 256 Dots** – each dot is 5.6 minutes of the day | ![](docs/dotmatrix-clock/15-den-256.svg) |
| 16 | **Moon Phase** – computed on the device, with a lunar-cycle bar | ![](docs/dotmatrix-clock/16-mesiac.svg) |
| 17 | **Temperature, Last 24 Hours** – bar chart, one sample every 45 minutes | ![](docs/dotmatrix-clock/17-graf-teploty.svg) |
| 18 | **Snake Around the Clock** – a snake runs along the border and grows every second | ![](docs/dotmatrix-clock/18-had.svg) |
| 19 | **Pong Plays for Time** – left score is hours, right score is minutes | ![](docs/dotmatrix-clock/19-pong.svg) |
| 20 | **Warp to the Next Minute** – stars speed up until the jump | ![](docs/dotmatrix-clock/20-warp.svg) |
| 21 | **Month in Columns** – one column per day, weekends marked on top | ![](docs/dotmatrix-clock/21-kalendar.svg) |
| 22 | **Matrix** – digital rain that forms the time and then scatters again | ![](docs/dotmatrix-clock/22-matrix-3-cas.svg) |

The temperature chart (17) lives only in the device's memory: it starts over after a restart and shows a full day after 24 hours.

## Matrix in a 15-second cycle

| Phase | Preview |
|---|---|
| 0–5 s: rain falls | ![](docs/dotmatrix-clock/22-matrix-1-dazd.svg) |
| 5–8 s: drops land in the shape of the digits and stick | ![](docs/dotmatrix-clock/22-matrix-2-formovanie.svg) |
| 8–12 s: the time is shown | ![](docs/dotmatrix-clock/22-matrix-3-cas.svg) |
| 12–15 s: the digits fall off and turn back into rain | ![](docs/dotmatrix-clock/22-matrix-4-pad.svg) |

## Home Assistant extras

While active, these replace whichever style is selected.

| Entity | What it does | Preview |
|---|---|---|
| **Timer Minutes**, **Start Timer**, **Cancel Timer** | MM:SS countdown with a shrinking bar, blinks for 5 s at the end | ![](docs/dotmatrix-clock/overlay-minutka.svg) |
| **Breathing 4-7-8** | 4 s inhale, 7 s hold, 8 s exhale, four rounds | ![](docs/dotmatrix-clock/overlay-dychanie.svg) |
| **Roll Dice** | two dice tumble, then show the result for 8 s | ![](docs/dotmatrix-clock/overlay-kocky.svg) |
| **Restart** | restarts the device | |

## Installation

1. Copy `secrets.example.yaml` to `secrets.yaml` and fill in your Wi-Fi name and password.
2. Download **Mobile Font** from [anke-art.de](https://www.anke-art.de) (donation-ware, so it is not included) and save it as `MobileFont.ttf` next to the configuration.
3. Temperature and weather (styles 7, 12 and 17) read the `weather.domov` entity from Home Assistant. If yours is named differently, change `entity_id` in the `sensor` and `text_sensor` blocks.
4. Flash the firmware:

```sh
esphome run dotmatrix-clock.yaml
```

Later updates go over Wi-Fi (`--device dotmatrix-clock.local`).

The images in `docs/dotmatrix-clock/` are rendered from the same font bitmaps as the display, at 14:37.

## Licenses

- Tom Thumb (`tom-thumb.bdf`) by Robey Pointer: MIT.
- Micro 5: SIL Open Font License, downloaded from Google Fonts.
- Mobile Font: anke-art.de, donation-ware, not part of this repository.
