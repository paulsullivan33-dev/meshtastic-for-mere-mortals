# Glossary

**Scope:** the terms, collected in one
place. Chapter references point at the
fullest explanation.

**Acknowledgement (ack)** — a routing reply
confirming a specific node received your
direct message; its absence comes back as a
NAK. Chapter 4.

**Antenna gain** — a measure of how an
antenna concentrates its pattern, not of
amplification. Higher gain flattens the
pattern toward the horizon. Chapter 5.

**Bandwidth** — the width of spectrum one
transmission occupies; wider is faster and
costs link budget. One of the three values
a modem preset bundles. Chapter 6.

**Channel** — a named messaging space with
its own encryption key; a node holds up to
eight. Chapter 4.

**Channel URL** — a link (or QR code)
carrying a channel set and its LoRa
settings, used to share channels between
nodes. Chapter 4.

**Client** — the default device role: a
node that talks and rebroadcasts for others
when usefully placed. Chapter 6.

**Coding rate** — how much redundancy is
added to resist noise; more is sturdier
and slower. Chapter 6.

**Direct message** — a message addressed to
one node ID rather than a channel, protected
by the two nodes' key pairs. Chapter 4.

**Duty cycle** — the share of time a node
may spend transmitting; capped at 10% in
the European licence-free bands the
firmware supports. Chapters 6 and 9.

**Firmware** — the software running on the
node itself; versioned separately from the
apps and the CLI. Chapter 1.

**Flooding** — Meshtastic's delivery method:
every node rebroadcasts each packet at most
once until its hop limit runs out. There is
no routing table. Chapter 4.

**Fresnel zone** — the plump region around
the straight path between two antennas that
should stay clear of obstacles for a strong
link. Chapter 5.

**Hop** — one relay of a packet. Each relay
spends one hop from the sender's hop limit.
Chapters 4 and 6.

**Hop limit** — how many relays a packet may
use; default 3, maximum 7. Chapter 6.

**Link budget** — in decibels, how much
signal loss a link can tolerate; the honest
way to compare presets and antennas.
Chapters 5 and 6.

**LoRa** — the low-power, long-range radio
technology underneath Meshtastic. Not the
same as LoRaWAN. Chapter 1.

**LoRaWAN** — a star-shaped network protocol
using the same radios, built for sensors
reporting through internet gateways.
Chapters 1 and 11.

**Mesh** — the web of nodes relaying for
each other; the thing the firmware exists
to make. Chapter 1.

**meshtasticd** — the firmware built as a
Linux daemon (the Portduino build), for
computers with LoRa modules and for labs
like this guide's. Chapter 8.

**Modem preset** — a named bundle of
spreading factor, bandwidth, and coding
rate. Nodes must share one to hear each
other. Chapter 6.

**MQTT** — a lightweight publish/subscribe
protocol; in Meshtastic, the bridge between
a mesh and the internet. Chapter 9.

**NAK** — the negative acknowledgement: the
mesh's report that a message went
unacknowledged, e.g. `MAX_RETRANSMIT`.
Chapter 4.

**Node** — one radio running the firmware,
whether handheld, on a roof, or (in the
lab) a daemon. Chapter 1.

**Node ID** — a node's permanent address,
written `!` plus eight hex digits, like
`!52282ebc`. Chapter 4.

**Node list** — the nodes one node has heard
and remembers; each node's list is its own.
Chapters 1 and 12.

**PKC** — public-key cryptography: the
per-node key pairs that protect direct
messages. The private half is a secret;
the public half travels with the node.
Chapters 4 and 9.

**Portduino** — the port of the firmware to
ordinary operating systems; `meshtasticd`
is its Linux daemon. Chapter 8.

**Preset** — see **Modem preset**.

**Primary channel** — channel 0; its name
also helps choose the node's exact
frequency. Chapter 4.

**PSK** — pre-shared key: the secret a
channel's members hold, used to encrypt
channel traffic. The default channel's PSK
is public knowledge. Chapter 4.

**Rebroadcast** — a node re-sending a packet
it heard, the act that makes a mesh.
Chapters 1 and 6.

**Region** — the firmware setting naming
your licence-free band, with its power
ceiling and duty cycle. Must be set before
a node will transmit. Chapters 3 and 6.

**Role** — a node's temperament: CLIENT,
ROUTER, TRACKER, and kin. Chapter 6.

**Router** — the infrastructure role: always
rebroadcasts promptly; for high, fixed,
well-powered spots. Chapter 6.

**Simulated radio** — the daemon's fallback
when no LoRa module is configured: the full
protocol with no air. Chapter 8.

**Spreading factor** — how drawn-out each
symbol is; higher is slower, hardier, and
doubles airtime per step. Chapter 6.

**Uplink / downlink** — the per-channel
switches deciding whether a node's MQTT
bridge publishes the channel to the
internet, and whether internet traffic
comes back. Chapter 9.
