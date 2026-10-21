# Your First Node

**Scope:** from a new device in a box to a
named node on your region's settings:
antenna discipline, flashing the firmware
(or confirming it's already there), pairing
with the phone app, the one setting that is
not optional, and naming. This chapter is
described from the project's documentation
rather than captured — no radio hardware
was available to the build; `criteria.md`
says so plainly.

## Rule zero: antenna before power

A LoRa radio transmitting into no antenna
can damage its own output stage — the power
has nowhere to go. So the order is fixed:
**antenna on first, then power**, every
time, including "just for a second" tests
on the bench. Finger-tight is enough on the
small screw connectors. If your device has
a spring antenna soldered on, it's already
handled; if it has a connector, it isn't.

## Flashing, or finding you don't need to

Many finished devices (Seeed and RAK units
typically) arrive with Meshtastic already
flashed; bare boards often arrive with older
firmware or something else entirely. The
project's web flasher (flasher.meshtastic.org,
in Chrome or Edge) writes current firmware
over USB. The two chip families differ here
(chapter 2):

- **ESP32 devices** flash over a USB serial
  connection. Some boards must be coaxed
  into flash mode by holding a BOOT button
  while plugging in, and Windows machines
  may need a USB-serial driver before the
  port appears at all.
- **nRF52840 devices** flash by file copy:
  double-tap the reset button and the board
  mounts as a USB drive; drop the firmware
  file (a `.uf2`) onto it and it reboots
  into Meshtastic.

Check: after flashing, the device's screen
(or its Bluetooth name, on a screenless
board) says Meshtastic, and the phone app
can see it in the device picker. If the app
sees nothing, the usual cause is a
charge-only USB cable during flashing, or
Bluetooth off on the phone — not a dead
board.

## Pairing

Install the Meshtastic app from the Play
Store or App Store, turn Bluetooth on, and
add the device it finds. Pairing asks for a
code: shown on the node's screen if it has
one; screenless boards use the default the
documentation prints for that device. Nodes
with Wi-Fi can also be reached over the
network by address — that's how the command
line in chapter 7 talks to the lab nodes.

Platform differences worth knowing: the
Android and Apple apps expose the same
settings but arrange them differently —
Radio Configuration on one is a Settings
tree on the other — and the Apple app also
runs on macOS. Features land on Android
first more often than the project would
like; if a setting in this guide isn't
where the chapter implies, search the app's
settings screen for its name before
concluding it's missing.

## The one setting that is not optional

Every new node starts with its **region
unset**, and an unset node will not transmit
at all — the project's LoRa configuration
page says it in so many words: the screen
shows a warning and no packets leave until
a region is chosen. Region is not a
preference; it tells the radio which slice
of spectrum it may use, at what power
ceiling, and under what duty-cycle limit
(chapter 6 has the table). Pick the region
you are physically in. Our lab did it from
the command line:

```
$ meshtastic --host localhost --set lora.region US
Connected to radio
Set lora.region to US
Writing modified preferences to device
Writing lora configuration to device
```

Check: reading the setting back after the
change (the node restarts itself when radio
settings move) shows it in force:

```
$ meshtastic --host localhost --export-config | grep -A6 "  lora:"
  lora:
    hopLimit: 3
    region: US
```

## Naming

Two names, both set in the app's user
settings or from the command line: a **long
name** (the friendly one, shown in lists)
and a **short name** of at most four
characters, which is what tiny device
screens and message headers squeeze in.
The lab named its nodes after their jobs:

```
$ meshtastic --host localhost --set-owner "Farmhouse" --set-owner-short "FARM"
Connected to radio
Setting device owner to Farmhouse and short name to FARM
```

Name yours for where the node lives or what
it does — "Gate," "Barn," "Ridge." In a big
public mesh, a name that says where the
node is helps everyone; in a private one,
it helps future you.

## Leave the rest alone

Region set, names set: stop. The defaults
you're most likely to be advised to change
on a forum — modem preset (LongFast), role
(Client), hop limit (3) — are correct for
essentially every first node, and chapter 6
explains what each actually does. A new node
with defaults joins the public mesh around
it, if any exists, on the default channel
(chapter 4). Spend your second evening on
placement (chapter 5), not settings.
