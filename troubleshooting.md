# Troubleshooting

**Scope:** failures in the order you're
likely to meet them, each as symptom →
cause → fix. Items the lab produced are
marked with their transcript; the rest come
from the project's documentation and are
worded as what they are.

## The node never transmits

**Symptom:** the screen shows a warning, or
the node listens but nothing it sends is
ever heard.

**Cause:** region is still `UNSET`. The
project's documentation is blunt: unset,
the node "will display a message and not
transmit any packets."

**Fix:** set your region (chapter 3), then
read it back — `--get lora.region` should
no longer say `0`.

## Two nodes, side by side, total silence

**Symptom:** two powered nodes on the same
bench hear nothing of each other.

**Causes, in the order to check them:**

1. **Antenna off** — the bench classic.
   Chapter 3's rule zero; also stop
   transmitting into the missing antenna.
2. **Preset mismatch** — one node was
   "improved" to a different preset
   (chapter 6). Both must match.
3. **Region mismatch** — nodes in different
   regions choose different frequencies.
4. **Band mismatch** — an 868 MHz device
   and a 915 MHz device (chapter 2). No
   setting fixes this; it is two different
   radios.

**Fix:** read back region and preset on
both (`--get lora.region`,
`--get lora.modem_preset`) and compare the
numbers before touching anything else.

## Sent, but no acknowledgement

**Symptom:** a direct message with `--ack`
ends with:

```
Received a NAK, error reason: MAX_RETRANSMIT
```

**Cause:** the destination never answered —
off, out of range, or unreachable through
the relays that exist. In the lab it meant
"no radio path exists at all" (transcript
t15).

**Fix:** this is the mesh reporting
honestly, not an error to suppress. Close
the distance, fix placement (chapter 5), or
accept store-and-forward's absence: a
message to a node that isn't listening is a
message that didn't arrive.

## The CLI prints a Broken pipe traceback, then works

**Symptom:**

```
ERROR file:tcp_interface.py _writeBytes line:137 Socket send error, reconnecting: [Errno 32] Broken pipe
```

followed by `Connected to radio` and a
correct result.

**Cause:** a disconnect race between CLI
2.7.11's heartbeat thread and the 2.5.19
daemon, seen repeatedly in the lab
(transcripts t04, t06, t07) — the previous
connection's socket closes as a new command
starts.

**Fix:** none needed when the result lines
are right. Verify by the read-back, as
always. If a command's *output* is missing
rather than noisy, that's a different
problem; re-run it alone.

## The app (or CLI) connects, then gets kicked off

**Symptom:** a second client connecting
drops the first.

**Cause:** one client at a time per node.
The daemon's log states it outright:
`[ApiServer] Force close previous TCP
connection`.

**Fix:** expect it. Do configuration from
one client at a time; the app and the CLI
are two doors into one room.

## --get prints numbers, not names

**Symptom:** `lora.region: 0` instead of
`lora.region: US`.

**Cause:** the CLI prints the raw enum
value. `0` means the *first* value of each
list: `CLIENT` for role, `UNSET` for
region, `LONG_FAST` for preset.

**Fix:** read names from `--export-config`
or the app; use `--get` for scripting,
where numbers are the point.

## The export has no region line

**Symptom:** `--export-config` output shows
a `lora:` section with hop limit and power
but no `region:`.

**Cause:** exports omit settings still at
their proto defaults — and a fresh node's
region *is* the default, `UNSET`. After the
lab set `US`, the line appeared
(transcripts t05 vs t09).

**Fix:** none. An absent region line *is*
the information: the node hasn't been told
where it lives.

## The second daemon has no web interface

**Symptom:** a second `meshtasticd` on one
host runs fine on its API port, but its web
server failed at startup.

**Cause:** both instances claim HTTPS port
443; the first wins (node B's log:
`Error starting Web Server framework`).

**Fix:** ignore it, or set a different
`Webserver: Port` for the second instance.
The API port — the one the CLI uses — is
the `-p` flag and never collided.

## A node went quiet mid-conversation (Europe)

**Symptom:** an `EU_868` or `EU_433` node
stops transmitting for a while, then
recovers on its own.

**Cause:** the 10% duty cycle, reached
(chapter 6). The firmware enforces it by
refusing to transmit until the rolling hour
allows.

**Fix:** wait, and afterwards: slower
presets and frequent position broadcasts
are what spent the budget. This is the
regulation working as designed, not a
fault.

## The node list disagrees with itself

**Symptom:** node A lists nodes B has never
heard of; a node you know exists is missing
from yours.

**Cause:** there is no central registry.
Each node lists **what it has heard**,
for as long as it remembers it — lists are
evidence, not truth, and distance writes
them.

**Fix:** read `LastHeard` before believing
any row. A missing node is an unheard node;
go where its packets can reach you, or wait
for a relay to move them.
