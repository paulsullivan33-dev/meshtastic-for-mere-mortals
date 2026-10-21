# Meshtastic for Mere Mortals

A phone in your pocket is a remarkable
radio that stops working exactly where the
interesting places are: past the last cell
tower, in the valley, inside the metal
barn. **Meshtastic** is the open-source
answer a large hobby has built for that
gap — small, cheap LoRa radios that pass
short text messages hand to hand, node to
node, with no network, no account, and no
subscription, until the message arrives.
This guide explains how the mesh works,
what the hardware is, which settings
actually matter (region above all, because
it is also the law), and how far the whole
thing honestly reaches — which is a
question about hills and antennas, not
about radios, and is treated that way here.

The lab is unusual, and the guide is upfront
about it: there were no radios on the build
bench. Instead, the Meshtastic firmware
itself ran as its own Linux daemon
(`meshtasticd`), twice, as two nodes of an
invented farm — **Farmhouse** (`!52282ebc`)
and **Barn** (`!200bf9ba`) — driven by the
official Python command line over their TCP
interfaces. Everything a client can do to a
node was exercised for real: identity,
region, configuration export, sending, and
the acknowledgement machinery reporting a
failure honestly. What could not be faked
was the air: the two simulated radios never
heard each other in the sandbox, so nothing
in this guide claims a delivery it didn't
see. Phone apps, flashing, antennas in the
field, and MQTT are described from the
project's documentation and labelled as
such; `criteria.md` keeps the full ledger
of what was run and what was only read.

## What this guide covers

- [**What Meshtastic is**](what-meshtastic-is.md) —
  the mental model: nodes, flooding, channels;
  what it is not (LoRaWAN, ham radio, a
  privacy promise); the lab
- [**The devices**](the-devices.md) — the
  ESP32 and nRF52840 families, the named
  boards and finished units, and the one
  choice you can't undo: the band
- [**Your first node**](your-first-node.md) —
  antenna before power, flashing two ways,
  pairing, the region setting, naming
- [**Channels and messages**](channels-and-messages.md) —
  channels and keys, the public default,
  sharing by QR, direct messages, flooding,
  and the ack that proves arrival
- [**Range, antennas and placement**](range-antennas-and-placement.md) —
  no invented distances: link budget,
  height, line of sight, and what antennas
  can and can't do
- [**Roles and radio settings**](roles-and-radio-settings.md) —
  region tables, modem presets, hop limit 3,
  and the roles from CLIENT to ROUTER
- [**The Python command line**](the-python-cli.md) —
  installing the CLI, `--info`, `--nodes`,
  getting and setting, exporting a node's
  whole configuration (and its secret)
- [**The Linux daemon**](the-linux-daemon.md) —
  meshtasticd: the firmware as a program,
  its config and startup log, and the
  two-node lab — including its failure
- [**Privacy, encryption and the rules**](privacy-encryption-and-rules.md) —
  what the keys protect, what the mesh can
  see, MQTT's internet door, and the legal
  frame in brief
- [**Recipes**](recipes.md) — the two-node
  proof walk, a private group channel, the
  ridge relay, backups, and "is it me or
  is the mesh quiet?"
- [**The neighbours**](the-neighbours.md) —
  LoRaWAN, MeshCore, Reticulum, APRS,
  satellite messengers, goTenna: what each
  trades, and why none of them joins your
  mesh
- [**Troubleshooting**](troubleshooting.md) —
  the silent node, mismatched presets, the
  honest NAK, the CLI's noisy traceback,
  and Europe's self-enforcing quiet
- [**Glossary**](glossary.md) — the terms,
  collected in one place

## What it doesn't cover

Building radios from bare modules, antenna
design beyond choosing and placing one,
and Meshtastic's module zoo (telemetry,
store-and-forward, ATAK, canned messages)
beyond naming what exists. Running a
public gateway or a community mesh as an
organisation is a social project this guide
only touches. The daemon on real SPI radio
hardware is described, not built — no
module was available to the build. And
nothing here is legal advice: chapter 9
states the rules' shape and points at your
national band plan for the numbers.

## How to use this guide

Read chapter 1 for the model, chapter 2
before buying anything, and chapter 3 with
a new node in hand. Chapters 5 and 6 are
the reference to return to when a link
disappoints — in that order, placement
before settings. Chapters 7 and 8 are for
readers with a keyboard: the CLI chapter
works against any node you can plug in or
reach, and the daemon chapter rebuilds
this guide's whole lab on one computer, no
radio required. The transcripts the
chapters quote live in
`meshtastic-lab/transcripts/`; the lab's
own notes are `examples/lab-notes.txt`.
