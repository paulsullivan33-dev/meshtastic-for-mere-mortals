# The Devices

**Scope:** the hardware families a node can
be — the two chip families underneath nearly
every device, the well-known boards and
finished units, and the differences that
actually matter when choosing: band, GPS,
screen, battery, and whether an antenna is
in the box. Prices are not quoted; they move
too much to print honestly.

## Every node is the same three parts

A LoRa radio chip, a small computer to run
the firmware, and an antenna. Devices differ
in which computer, what else is soldered on,
and how they're packaged — not in what the
mesh sees. Any node can talk to any other if
the radio settings match (chapter 6).

## The two chip families

Nearly everything on sale runs one of two
microcontrollers, and the split predicts the
device's character better than its brand:

| | ESP32 family | nRF52840 family |
| --- | --- | --- |
| Radios of its own | Wi-Fi and Bluetooth | Bluetooth only |
| Power appetite | Noticeably hungrier | Sips; best for solar and trackers |
| Typical devices | Heltec boards, T-Beam, T-Deck | T-Echo, WisBlock, T1000-E |
| Flashing | USB serial (chapter 3) | USB drive drag-and-drop (chapter 3) |

Neither is "better." An ESP32 node on a
windowsill with wall power is a fine
citizen. An nRF52 node is what you want
running for months off a small solar panel.

## Names you will meet

- **Heltec WiFi LoRa 32** (V3 and newer) —
  the common starter board: ESP32, a small
  screen, a battery connector. Bare board;
  you supply the case.
- **LILYGO T-Beam** — ESP32 with a GPS
  receiver and an 18650 battery holder. The
  classic handheld build.
- **LILYGO T-Echo** — nRF52840 with an e-ink
  screen and GPS in a finished case; the
  low-power pocket option.
- **RAK WisBlock** — a modular system: a
  RAK4631 core (nRF52840) plus a base board,
  sold as Meshtastic starter kits. The usual
  pick for fixed outdoor and solar nodes.
- **Seeed SenseCAP T1000-E** — a card-sized
  finished tracker, nRF52840 and GPS, sealed
  against weather. Made to be carried, not
  opened.
- **Seeed XIAO-based kits** — a tiny
  nRF52840 module plus a LoRa expansion
  board; the hobbyist's small-parts route.

The project keeps a supported-hardware list
in its documentation; treat any device not
on it as an experiment, however confidently
a marketplace listing uses the word
"Meshtastic."

## The choice that cannot be fixed later

**Band.** LoRa hardware is built for one
part of the spectrum. North American devices
are made for the 902–928 MHz band; European
devices for 868 MHz (or 433 MHz). A 915 MHz
node and an 868 MHz node cannot hear each
other, ever, and no setting changes it —
the difference is in the radio and its
antenna. Buy the band your region uses
(chapter 6's table), from a seller who
states it in plain text. Listings that say
"868/915" usually mean "we stock both; pick
at checkout" — pick deliberately.

## What else varies

- **GPS.** Present on trackers and most
  handhelds, absent on many fixed boards
  (or added as a module). A node without
  GPS can still be given a fixed position
  by hand, or share none.
- **Screen.** Handy for a handheld, useless
  weight on a roof. Several roles turn the
  screen off anyway (chapter 6).
- **Battery.** Finished units include one;
  bare boards have a connector and expect
  an 18650 or a LiPo pouch. Anything solar
  is a kit, not a product: panel, charge
  controller, battery, enclosure.
- **Antenna.** Some boards ship with a small
  spring or stub antenna; some expect you
  to screw on your own. Chapter 5 is mostly
  about this part, because it decides more
  of your range than the electronics do.

## Buying advice that fits in a paragraph

Buy two of the same device, in your band,
from the project's supported list. Two nodes
let you prove the mesh works across your
own ground — farmhouse to barn, house to
gate — before spending on a third for the
high spot. One node alone can only join
meshes other people built, and in a quiet
area it will sit in silence that tells you
nothing about whether it works.
