# Roles and Radio Settings

**Scope:** the settings that decide whether
nodes can hear each other and how they
behave once they can — region first, because
it is both a technical and a legal setting;
the modem presets; the hop limit; and the
device roles. Every default below was read
back from the lab node, and the tables are
from the project's own configuration pages.

## Region: the setting that is also the law

Region tells the firmware which slice of
spectrum it may use, at what power ceiling,
and under what duty cycle — the legal shape
of your country's licence-free band, baked
into one choice. The official table (from
the project's LoRa configuration page,
abridged):

| Region | Band (MHz) | Duty cycle | Power limit |
| --- | --- | --- | --- |
| `US` | 902.0 – 928.0 | 100% | 30 dBm |
| `EU_868` | 869.4 – 869.65 | 10% | 27 dBm |
| `EU_433` | 433.0 – 434.0 | 10% | 10 dBm |
| `ANZ` | 915.0 – 928.0 | 100% | 30 dBm |
| `JP` | 920.5 – 923.5 | 100% | 13 dBm |

The page carries the warning this guide
repeats in chapter 9: the power limit
shown is **the ceiling the firmware applies,
not your national limit** — some countries
allocate less than the full range shown, so
check your national band plan.

Two facts make region the first setting of
all. Unset, the node **will not transmit**
(chapter 3). And the European duty cycle is
real: in `EU_433` and `EU_868`, a node may
transmit at most 10% of the time, calculated
every minute over a rolling hour — reach
the limit and the firmware stops
transmitting until it is allowed again.
Chatty settings are not just rude in
Europe; they are self-defeating.

The lab read region back as a bare number
before and after setting it — the command
line prints enum values, while the apps and
the configuration export print names:

```
$ meshtastic --host localhost --get lora.region
Connected to radio
lora.region: 0
```

`0` is `UNSET`. After chapter 3's
`--set lora.region US`, the export shows
`region: US` by name (chapter 3 quotes it).

## Modem presets: one trade, made by everyone

A **preset** bundles three radio parameters
— spreading factor, bandwidth, coding rate —
into one name, and it is the mesh's single
most consequential choice, because **nodes
only hear each other when region and preset
match**. The project's table, abridged, with
its theoretical data rates and link budgets
(chapter 5 explains the budgets; both are
computed, not measured, and assume a plain
0 dB antenna):

| Preset | Data rate | Link budget |
| --- | --- | --- |
| Short Turbo | 21.88 kbps | 140 dB |
| Short Fast | 10.94 kbps | 143 dB |
| Medium Fast | 3.52 kbps | 148 dB |
| **Long Fast** (default) | 1.07 kbps | 153 dB |
| Long Moderate | 0.34 kbps | 156 dB |

Faster presets empty the air quickly — good
for a busy city mesh — and give up path
tolerance. Slower presets reach further on
worse paths and make everyone wait longer
per message, which under a 10% duty cycle
is a real budget being spent. The whole
mesh moves together or not at all: changing
your preset to something clever is the
standard way to go deaf in this hobby. Some
presets are not legal in some regions (the
500 kHz "Turbo" presets are not permitted
in `EU_868`); the firmware refuses or
replaces those, which is the correct
behaviour and not a bug to work around.

The lab's preset, read back:

```
$ meshtastic --host localhost --get lora.modem_preset
Connected to radio
lora.modem_preset: 0
```

`0` is `LONG_FAST`, the default — confirmed
in the node's startup log as `name=LongFast`
(chapter 8).

## Hop limit: 3, and the docs mean it

Each relay spends one hop; the sender's
limit (default **3**, maximum **7**) says how
far a flood may spread:

```
$ meshtastic --host localhost --get lora.hop_limit
Connected to radio
lora.hop_limit: 3
```

The project's configuration page annotates
its own default with unusual force:
"Default is 3 which should be fine for most
applications. *Really, 3 is fine.*" Higher
limits multiply every message's airtime by
the size of the crowd that can hear it — in
a dense mesh, raising hops is how a network
drowns in its own relays. Leave it at 3.

## Roles: what a node does with its day

A node's **role** sets its temperament. The
lab node read back `device.role: 0` —
`CLIENT`, the default. The roles a new user
will actually meet:

| Role | Behaviour | Use it for |
| --- | --- | --- |
| `CLIENT` | Talks, and rebroadcasts others' packets when no better-placed node has | Almost everything |
| `CLIENT_MUTE` | Talks, never rebroadcasts | A second node in the same house; dense areas |
| `CLIENT_HIDDEN` | Like mute, and stays out of node lists | Low-profile fixed nodes |
| `ROUTER` | Always rebroadcasts once, promptly; treated as infrastructure | The high, fixed, well-powered spot |
| `ROUTER_LATE` | Rebroadcasts only after others have had their chance | Filling the edge of a mesh |
| `TRACKER` | Prioritises position reports | Vehicles, assets, the dog |
| `SENSOR` | Prioritises telemetry reports | Weather and environment sensors |

Older guides love the `REPEATER` role; the
current documentation marks it deprecated —
use `ROUTER`. And note the social rule the
community repeats everywhere: a router in a
bad spot, or three routers where one would
do, makes the mesh *worse* (collisions,
duplicate relays). Infrastructure is a
decision for the whole mesh, not a treat
for one node. The farmhouse in our example
earns a `ROUTER` on the silo only after
chapter 5's tests show the house-to-barn
link needs it.

Check: with region matching, preset
matching, and hop limit 3, two nodes are
configured to hear each other. Everything
else in this chapter is optimisation; those
three are compatibility.
