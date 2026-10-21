# Channels and Messages

**Scope:** how talk is organised — channels
as named, keyed spaces; the default channel
and why it is public; sharing a channel by
link or QR code; direct messages; and what
happens to a message as the mesh carries it,
including the acknowledgement that proves
arrival — and the one that proves it didn't.

## A channel is a name and a key

A node holds up to **eight channels**, in
slots 0 to 7. Each channel is a name plus a
**PSK** — a pre-shared key, a secret byte
string every member must hold. Channel
traffic is encrypted with that key (AES;
the project's radio-settings page notes the
key can be 128- or 256-bit, or encryption
can be off entirely in the licensed mode
chapter 9 describes).

**Channel 0, the primary channel**, is
special: its name also helps choose which
exact frequency the node uses — the firmware
hashes the primary channel's name into a
frequency slot when you don't set one. In
the US region with the default settings that
lands on slot 20 of 104, 906.875 MHz; our
lab node's startup log shows precisely that
(and chapter 8 quotes it). Rename the
primary channel and you may quietly change
frequency as well as name.

## The default channel is a public square

Out of the box, channel 0 is named for the
default modem preset — **LongFast** — and
its key is the well-known default, a single
byte written `AQ==` in the tools. That key
is printed in the project's documentation
and known to every Meshtastic user on
earth. Treat the default channel exactly
like a public square: anyone in radio reach
with a node can read and join it. That's a
feature — it's how ad-hoc meshes form at
events and how you discover neighbours —
but nothing said there is private, and
chapter 9 returns to it.

Privacy lives in extra channels: add one in
slots 1–7 with a fresh random key (the
command line spells it `--ch-set psk random`
when editing channels), and only holders of
that key can read it.

## Sharing a channel: links and QR codes

Channels travel between nodes as a **channel
URL** — one link (or QR code, same content)
carrying the channel set and the LoRa
settings that go with it. The lab node's own
link, from its configuration export, looks
like this (truncated):

```
channel_url: https://meshtastic.org/e/#CgcSAQE6AggNEgoIAUADSAFQHmgB
```

Scanning someone's QR in the app offers to
add or replace channels with theirs. Two
consequences worth knowing before you scan
strangers' codes at a meetup: replacing
means your old channel set goes away, and a
shared link can carry radio settings as well
as channels, so a node that "joined a group"
may also have changed how it talks. The
command line applies a link with
`meshtastic --ch-set-url URL`, and prints one
for sharing with `--qr`.

## Direct messages

A **direct message** goes to one node, named
by its node ID — the `!` plus eight hex
digits you'll see everywhere in the tools,
like the lab's `!52282ebc`. On current
firmware, direct messages are protected by
the two nodes' own public keys (the lab
node reports `"hasPKC": true` in its info):
each node generates a key pair, publishes
the public half in its identity, and keeps
the private half secret. Nobody else on the
channel can read a direct message's contents,
though they can see that *someone* is
talking — chapter 9 again.

## How a message travels

Meshtastic **floods**. You transmit once.
Every node that hears your packet
rebroadcasts it — at most once each, and
only until its **hop limit** runs out. The
sender sets the limit (default 3); each
relay spends one hop. There is no routing
table and no chosen path: the message spreads
like a rumour, and duplicates die quietly
because each node remembers what it already
relayed.

Sending from the command line shows the
essentials in two lines:

```
$ meshtastic --host localhost --sendtext "Testing from the farmhouse"
Connected to radio
Sending text message Testing from the farmhouse to ^all on channelIndex:0
```

`^all` is the broadcast address; `channelIndex:0`
is the primary channel. Against a real mesh
that text would now be spreading. Against
our lab it went nowhere — no other node
could hear it — which made the lab the
perfect place to capture the honesty
machinery. A direct message can ask for an
**acknowledgement**:

```
$ meshtastic --host localhost --sendtext "Barn, do you copy?" --dest "!200bf9ba" --ack
Connected to radio
Sending text message Barn, do you copy? to !200bf9ba on channelIndex:0
Waiting for an acknowledgment from remote node (this could take a while)
Received a NAK, error reason: MAX_RETRANSMIT
```

Check: an ack means a specific node took
responsibility for your message. A **NAK**
with `MAX_RETRANSMIT` means the sender
retried until it gave up — the destination
was off, out of range, or (our case) didn't
exist on any radio the sender could reach.
The apps show the same truth with a tick or
a warning icon. Flooding has no memory: a
node that was asleep when you sent will not
receive the message later, unless a
store-and-forward node was listening — an
extra role the documentation describes and
this guide does not test.

## Size and speed, honestly

At the default preset the theoretical data
rate is about **1.07 kbps** (chapter 6's
table) — a text message is a noticeable
moment on the air, not an instant. Keep
messages short; the apps stop you at a few
hundred characters, and a mesh carrying
essays at 1 kbps under a 10% duty cycle
(chapter 6) is a mesh asking its members to
wait. This is a postcard medium. Write
postcards.
