# The Python Command Line

**Scope:** the `meshtastic` command — the
project's Python client — installed, connected
to a node over USB or the network, and used
for the jobs this guide has already shown:
reading identity and settings, changing them,
exporting a whole configuration, sending
text. Every output below is a capture from
the lab nodes of chapter 8.

## Installing

The CLI is a Python package, installed with
pip. A virtual environment keeps it out of
your system's way (and off its package
manager's nerves):

```
$ python3 -m venv ~/meshtastic-cli
$ ~/meshtastic-cli/bin/pip install meshtastic
$ ~/meshtastic-cli/bin/meshtastic --version
2.7.11
```

Check: the version prints bare — `2.7.11`
here. Windows and macOS readers: the same
pip install works; serial port names differ
(`/dev/ttyUSB0` and kin on Linux,
`/dev/cu.*` on macOS, `COM3`-style on
Windows), and chapter 3's driver note
applies to ESP32 boards.

## Connecting

Three ways in, one flag each:

- **USB serial** is the default: plug the
  node in and run `meshtastic --info`. Add
  `--port /dev/ttyUSB0` to name the port
  when several devices compete.
- **TCP** with `--host`: a node on Wi-Fi,
  or the daemon of chapter 8, listens on
  port **4403** by default. `--host localhost`
  and `--host localhost:4404` are how this
  guide drove lab nodes A and B.
- Everything else in `--help` — 344 lines of
  it, captured as transcript t01 — is
  variations on those two doors.

One wart, recorded because you'll meet it:
a node serves **one client at a time**. A
second connection force-closes the first
(the daemon's log says so, in those words),
so a phone app and the CLI will take turns
kicking each other off the same node.

## Reading a node

`--info` is the node's whole story in one
dump — identity, firmware, the node list,
and every preference. The head of node A's
first capture:

```
$ meshtastic --host localhost --info
Connected to radio

Owner: Meshtastic 2ebc (2ebc)
My info: { "myNodeNum": 1378365116, "minAppVersion": 30200, "pioEnv": "native", "rebootCount": 0, "deviceId": "", "firmwareEdition": "VANILLA", "nodedbCount": 0 }
Metadata: { "firmwareVersion": "2.5.19.f9876cf", "deviceStateVersion": 23, "hasWifi": true, "positionFlags": 811, "hwModel": "PORTDUINO", "hasPKC": true, ... "role": "CLIENT", ... }
```

Read it as: the node is `!52282ebc` (the hex
of that node number), a PORTDUINO running
firmware 2.5.19, role CLIENT, public-key
crypto available. `--nodes` shows the same
mesh as a table — one row per known node,
with signal, battery, and last-heard
columns; chapter 8 shows both lab nodes'
tables, one row each, which is the honest
state of a two-node lab whose radios
couldn't meet.

Single settings read with `--get`, which
prints raw enum numbers (chapter 6 decodes
the three that matter): `device.role: 0` is
CLIENT, `lora.region: 0` is UNSET,
`lora.hop_limit: 3` is itself.

## Changing things, and proving it

`--set` writes a setting; owner names have
their own flags. Both were run against the
lab in chapter 3, and the proof pattern is
worth copying: **change, wait for the
node's restart, read back.** Radio changes
restart the node (the daemon log records
"Reboot in 7 seconds"), so a read issued
too quickly talks to a node mid-restart —
give it a few seconds, then `--info` again
and look for your change in the Owner line
or the preferences. A setting you haven't
read back is a setting you merely
*requested*.

## The whole configuration, and its secret

`--export-config` prints the node's
configuration as YAML (chapter 3 quotes its
LoRa section). It is the node's complete
backup — and it contains a secret:

```
  security:
    privateKey: base64:<redacted — see criteria.md, Capture hygiene>
    publicKey: base64:Ou0xBQbpBiQrX+pAE1j1MsRmKSYGZY4NC6pTR/CHeXY=
```

That private key *is* the node's identity
for direct messages (chapter 9). The
transcript in this repo redacts the lab
node's key; your exports deserve the same
respect — treat the file like a password,
because anyone holding it can impersonate
the node. The public key, by contrast, is
meant to be public; it appears in the
node's own info dump.

## Sending text

`--sendtext`, `--dest`, and `--ack` are
covered where they belong, in chapter 4,
with both the broadcast and the failed
direct message quoted in full. The one
CLI-specific note: `--listen` turns the
command into a receiver, printing packets
as they arrive — against the lab it printed
nothing for 25 seconds while node A sent,
which is how the delivery failure was
*proven* rather than assumed (transcript
t14).

## The traceback you may see

On several lab runs, the CLI printed a
`Broken pipe` traceback from its heartbeat
thread — `Socket send error, reconnecting:
[Errno 32] Broken pipe` — and then completed
the command correctly. It is a disconnect
race between this CLI (2.7.11) and the
older daemon firmware (2.5.19), noisy and
harmless *in this pairing*; chapter 12
lists it so you don't chase it. Judge a
command by its result lines and read-backs,
not by the absence of noise on the way.
