# ESPHome Dotmatrix Clock

![Matrix, ktorý sformuje čas](docs/dotmatrix-clock/22-matrix-3-cas.svg)

Hodiny na displeji 32 × 8 bodov zo štyroch modulov MAX7219, riadené ESP8266 (esp12e). Konfigurácia je v [`dotmatrix-clock.yaml`](dotmatrix-clock.yaml).

- **Zapojenie:** SPI CLK = GPIO14, MOSI = GPIO5, CS = GPIO0.
- **Čas:** SNTP (`pool.ntp.org`, Europe/Bratislava), synchronizácia každých 15 minút. Displej sa prekresľuje každých 50 ms, takže sekundy sedia s presnosťou na desiatky milisekúnd.
- **Písma:** `tom-thumb.bdf` (3×5, MIT) je v repe, Micro 5 si ESPHome stiahne z Google Fonts, `MobileFont.ttf` si treba stiahnuť (pozri nižšie).
- **Home Assistant:** pripája sa cez natívne API; štýly, minútka, kocky a dýchanie sú entity.

Štýl sa prepína entitou **Clock Style** v Home Assistante alebo vo webovom rozhraní displeja (port 80). Displej si posledný výber pamätá aj po výpadku prúdu.

## Štýly

| # | Štýl | Ukážka |
|---|---|---|
| 0 | **Tom Thumb HH:MM:SS** – číslice 3×5, všetky rovnako široké | ![](docs/dotmatrix-clock/00-tom-thumb.svg) |
| 1 | **Micro 5 HH:MM:SS** – užšie dvojbodky | ![](docs/dotmatrix-clock/01-micro5.svg) |
| 2 | **MobileFont + sekundový pásik** – pásik sa do 30. sekundy plní, potom zhasína zľava | ![](docs/dotmatrix-clock/02-pasik-hore.svg) |
| 3 | **MobileFont + sekundový pásik dole** | ![](docs/dotmatrix-clock/03-pasik-dole.svg) |
| 4 | **Pôvodné** – čiarka v hornom riadku sa posúva každé dve sekundy | ![](docs/dotmatrix-clock/04-povodne.svg) |
| 5 | **Binárne hodiny** – stĺpce 8-4-2-1 pre HH MM SS | ![](docs/dotmatrix-clock/05-binarne.svg) |
| 6 | **Rolujúce číslice** – meniaca sa číslica odroluje hore | ![](docs/dotmatrix-clock/06-rolujuce.svg) |
| 7 | **Čas a vonkajšia teplota** – 7 s čas, 3 s teplota z `weather.domov` | ![](docs/dotmatrix-clock/07-teplota.svg) |
| 8 | **Čas a deň v týždni** – dieliky po – ne, dnešok svieti celý | ![](docs/dotmatrix-clock/08-den-v-tyzdni.svg) |
| 9 | **Presýpacie sekundy** – každú sekundu spadne zrnko do kôpky | ![](docs/dotmatrix-clock/09-presypacie.svg) |
| 10 | **Čas, ktorý sa rozpadne** – od 50. sekundy sa číslice menia na Game of Life | ![](docs/dotmatrix-clock/10-rozpad.svg) |
| 11 | **Pac-Man zje minútu** – pri zmene minúty zje staré HH:MM | ![](docs/dotmatrix-clock/11-pacman.svg) |
| 12 | **Živé počasie** – animovaná ikona podľa `weather.domov` | ![](docs/dotmatrix-clock/12-pocasie.svg) |
| 13 | **Ručičkové hodinky** – ciferník 8×8 a sekundový bod po okraji | ![](docs/dotmatrix-clock/13-rucickove.svg) |
| 14 | **Čas vystúpi z dažďa** – body číslic sa chytajú z padajúcich kvapiek | ![](docs/dotmatrix-clock/14-dazd.svg) |
| 15 | **Deň v 256 bodoch** – každý bod je 5,6 minúty dňa | ![](docs/dotmatrix-clock/15-den-256.svg) |
| 16 | **Fáza Mesiaca** – počítaná priamo v displeji, s lunárnym kalendárom | ![](docs/dotmatrix-clock/16-mesiac.svg) |
| 17 | **Teplota za 24 hodín** – stĺpcový graf, vzorka každých 45 minút | ![](docs/dotmatrix-clock/17-graf-teploty.svg) |
| 18 | **Had okolo hodín** – had po okraji narastie každú sekundu | ![](docs/dotmatrix-clock/18-had.svg) |
| 19 | **Pong hrá o čas** – skóre vľavo sú hodiny, vpravo minúty | ![](docs/dotmatrix-clock/19-pong.svg) |
| 20 | **Warp do novej minúty** – hviezdy zrýchľujú až do skoku | ![](docs/dotmatrix-clock/20-warp.svg) |
| 21 | **Mesiac v stĺpcoch** – jeden stĺpec za deň, víkendy hore | ![](docs/dotmatrix-clock/21-kalendar.svg) |
| 22 | **Matrix** – dážď, z ktorého sa sformuje čas a znova sa rozsype | ![](docs/dotmatrix-clock/22-matrix-3-cas.svg) |

Graf teploty (17) žije len v pamäti displeja: po reštarte začína odznova a celý deň je vidieť po 24 hodinách.

## Matrix v 15-sekundovom cykle

| Fáza | Ukážka |
|---|---|
| 0–5 s: padá dážď | ![](docs/dotmatrix-clock/22-matrix-1-dazd.svg) |
| 5–8 s: kvapky dopadajú do číslic a prilepia sa | ![](docs/dotmatrix-clock/22-matrix-2-formovanie.svg) |
| 8–12 s: svieti čas | ![](docs/dotmatrix-clock/22-matrix-3-cas.svg) |
| 12–15 s: číslice padnú dole a stane sa z nich dážď | ![](docs/dotmatrix-clock/22-matrix-4-pad.svg) |

## Doplnky z Home Assistanta

Kým sú aktívne, prekryjú akýkoľvek zvolený štýl.

| Entita | Čo robí | Ukážka |
|---|---|---|
| **Minútka (min)**, **Spustiť minútku**, **Zrušiť minútku** | odpočet MM:SS so skracujúcim sa pásikom, na konci 5 s bliká | ![](docs/dotmatrix-clock/overlay-minutka.svg) |
| **Dýchanie 4-7-8** | 4 s nádych, 7 s zadržanie, 8 s výdych, štyri kolá | ![](docs/dotmatrix-clock/overlay-dychanie.svg) |
| **Hoď kockou** | dve kocky sa roztočia a 8 s ukazujú výsledok | ![](docs/dotmatrix-clock/overlay-kocky.svg) |
| **Restart** | reštart displeja | |

## Inštalácia

1. Skopíruj `secrets.example.yaml` na `secrets.yaml` a doplň názov a heslo WiFi.
2. Stiahni písmo **Mobile Font** z [anke-art.de](https://www.anke-art.de) (donation-ware, preto nie je v repe) a ulož ho ako `MobileFont.ttf` vedľa konfigurácie.
3. Teplota a počasie (štýly 7, 12 a 17) čítajú entitu `weather.domov` z Home Assistanta. Ak sa tvoja volá inak, zmeň `entity_id` v blokoch `sensor` a `text_sensor`.
4. Nahraj firmvér:

```sh
esphome run dotmatrix-clock.yaml
```

Ďalšie nahratia idú cez WiFi (`--device dotmatrix-clock.local`).

Obrázky v `docs/dotmatrix-clock/` sú vykreslené z rovnakých bitmáp písiem ako na displeji, v čase 14:37.

## Licencie

- Tom Thumb (`tom-thumb.bdf`) od Robeyho Pointera: MIT.
- Micro 5: SIL Open Font License, sťahuje sa z Google Fonts.
- Mobile Font: anke-art.de, donation-ware, nie je súčasťou repa.
