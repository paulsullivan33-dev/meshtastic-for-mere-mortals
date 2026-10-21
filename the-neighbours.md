# The Neighbours

**Scope:** the technologies Meshtastic is
confused with, and the ones it competes
with for the same pocket — what each
actually is, and the trade it makes. None of
these were run for this guide; each is
described from its own project's public
documentation, and none is taught here.

## LoRaWAN, The Things Network, Helium

The confusion to kill first (chapter 1
killed it; here it gets a page). **LoRaWAN**
is a protocol for *sensors*: a device talks
to a gateway, the gateway talks to the
internet, the internet talks back. **The
Things Network** is a community LoRaWAN
network; **Helium** is a commercial one.
Same radio technology, opposite shape: no
gateway and no internet means no LoRaWAN,
while Meshtastic is at its best exactly
then. If your goal is a soil sensor
reporting to a dashboard, you want this
neighbour. If your goal is texting a human
in a dead zone, you don't.

## MeshCore

A younger sibling: firmware for much the
same LoRa hardware, built around companion
radios paired to a phone app, with its own
protocol and its own community. It is
**not** interoperable with Meshtastic —
same air, different language — and choosing
between them is mostly choosing which local
community and which design trade-offs you
prefer. If everyone near you runs one,
that *is* the deciding fact; a mesh of one
is a walkie-talkie with nobody listening.

## Reticulum

A software network stack rather than a
radio project: it builds encrypted,
self-configuring networks over whatever
bearers exist — including LoRa, via its
RNode radios — and identifies users by
cryptographic keys rather than node
numbers. More general, more demanding, and
the pick of people who want the network
itself to be the private thing. It does not
interoperate with Meshtastic either.

## APRS

Amateur radio's packet network: licensed
operators beaconing position and short
messages on shared frequencies, received by
digipeaters and internet gateways, with a
public map as the living centre of the
hobby. Meshtastic's position-sharing mesh
will feel familiar to anyone who has used
it — the differences are the licence
(APRS requires one, and forbids encryption)
and the culture (callsigns, not handles).
If you hold a licence, the two coexist
happily in one bag.

## Satellite messengers and phones

Garmin inReach and its kin, and the
satellite features now built into phones,
are not meshes at all: each message rents a
moment on someone else's satellite fleet.
They reach places no ridge relay can —
mid-ocean — at subscription prices, with
per-message limits, and they work when
everyone you know owns nothing at all.
For a solo traveller beyond all relays,
this neighbour isn't a rival so much as
the honest alternative.

## goTenna

The commercial answer to the same problem:
closed, phone-paired mesh messengers sold
as products, aimed largely at teams and
agencies. It works, it's supported like a
product, and the network is whoever bought
the same product. Meshtastic's counter-offer
is commodity hardware, open firmware, and a
public default channel that strangers can
already hear.

| Neighbour | Shape | Needs internet | Licence | Talks to Meshtastic |
| --- | --- | --- | --- | --- |
| LoRaWAN / TTN | Star via gateways | Yes, at the gateway | No | No |
| MeshCore | Mesh | No | No | No |
| Reticulum | Software stack, many bearers | No | No | No |
| APRS | Mesh + gateways | No (gateways add it) | **Yes** | No |
| Satellite | Star via satellites | Is the internet | No | No |
| goTenna | Mesh | No | No | No |

Check: the last column is the lesson. These
are neighbouring answers to neighbouring
problems; not one of them joins your mesh.
Pick by the problem and the local crowd,
not by the radio — the radio is the one
part they mostly share.
