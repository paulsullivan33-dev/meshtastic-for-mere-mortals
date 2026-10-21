# What Meshtastic Is

**Scope:** the mental model — small radios
that pass short text messages hand to hand,
with no phone network, no internet, and no
account — plus what a node is, what a mesh
is, and the lab this guide's claims rest on.

## The one-paragraph version

Meshtastic is open-source firmware for small,
cheap radios built around a technology called
**LoRa** — a way of sending tiny amounts of
data a long way on very little power, in
licence-free radio bands. Each radio is a
**node**. Nodes listen for each other and
**rebroadcast** what they hear, so a message
can travel further than any single radio
could send it, hopping from node to node.
That web of nodes is the **mesh**. You type
on a phone app; the phone hands your words
to its radio over Bluetooth; the radio puts
them on the air; every node that hears them
passes them along until they arrive.

Nothing about it needs the internet, a SIM
card, or a subscription. Two hikers on
opposite sides of a ridge can swap short
texts if a third node on the ridge top can
hear them both. A farmhouse can reach its
barn, its gate, and its far field the same
way. That is the whole idea, and most of
this guide is the fine print: what "short"
and "a long way" honestly mean, and the
settings that decide whether two nodes can
hear each other at all.

## The phone is a remote control

A Meshtastic node is a complete radio on its
own — it can relay other people's messages
for days with no phone nearby. The phone app
(Android and iOS, chapter 3) is how you type,
read, and change settings. It connects over
Bluetooth, or over Wi-Fi or USB to nodes
that offer them. Lose the phone and the node
keeps relaying; the mesh does not notice.
This matters for how you plan: one node can
sit on a roof doing quiet work for a year
while the node in your pocket does the
talking.

Your node also keeps a **node list**: every
node it has heard, with names, signal
strength, and battery where known. The list
persists between reboots — the lab node in
chapter 8 reloaded its list from its own
files on restart.

## What a message is, and is not

Messages are short text — think text-message
length, not email. There are no voice calls,
no photos, no file transfers. A message goes
either to a **channel** (everyone who shares
that channel hears it, the way a party line
worked) or **directly** to one node. Nodes
can also broadcast their position if they
have GPS and you leave position sharing on,
which is why privacy gets its own chapter
(chapter 9) rather than a footnote.

Delivery is best-effort, and honest about
it. Send a direct message and ask for an
acknowledgement and the mesh will tell you
if none comes back — chapter 4 shows the
failure captured live, because our lab
produced a real one.

## What it is not

Three confusions to clear away early,
because the names sound alike:

- It is **not LoRaWAN**. LoRaWAN (and the
  networks built on it, like The Things
  Network or Helium) is a star: sensors talk
  to gateways, gateways talk to the internet.
  Meshtastic is a mesh of equals, and works
  with the internet switched off entirely.
  The project's own radio-settings page
  opens with exactly this warning.
- It is **not amateur (ham) radio**, though
  licensed hams use it too. It runs in
  licence-free bands by default. There is a
  licensed mode and amateur-band regions in
  the firmware, with different rules —
  chapter 9 states them plainly.
- It is **not a privacy guarantee**. Channel
  traffic is encrypted with a shared key,
  and the key on the default channel is
  public knowledge. Chapter 9 is blunt
  about what that does and doesn't protect.

## Versions and the lab

Meshtastic moves in three pieces, versioned
separately: the **firmware** on the radios,
the **phone apps**, and the **Python command
line** tool. This guide was built against
the command line at version **2.7.11** and
the firmware at **2.5.19.f9876cf**, running
not on a radio but as the project's Linux
daemon on an ordinary computer, twice, as
two pretend farm buildings — Farmhouse and
Barn. Chapter 8 explains that trick and
exactly how far it went: configuration,
identity, and message sending are real
firmware behaviour, exercised live.
Delivery between the two lab nodes never
happened — the simulated radios could not
hear each other in this sandbox — and this
guide says so wherever it matters instead
of pretending otherwise. `criteria.md`
keeps the full ledger.

Check: you should now be able to say what
carries a message when no single radio can
reach — every node in between, rebroadcasting.
If that picture is clear, chapter 2 names
the hardware that does it.
