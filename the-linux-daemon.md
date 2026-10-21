# The Linux Daemon

**Scope:** `meshtasticd` — the firmware
itself, compiled to run as an ordinary Linux
program (the build is called Portduino) —
where it comes from, how it's configured,
what its startup log proves, and the
two-instance lab this whole guide rests on,
including the part that didn't work.

## Why a daemon exists

The same firmware that runs on a $25 radio
also builds for Linux, for two honest
reasons: a computer with a real LoRa module
on its GPIO pins makes a fine fixed node
(a Raspberry Pi with a LoRa HAT is the
usual shape), and developers need nodes
they can run without a desk full of radios.
Run it with no radio hardware configured
and it falls back to a **simulated radio** —
which is what our lab used, and the source
of both its value and its limit.

## Getting it

The daemon ships with the firmware releases
on the project's GitHub: our copy came from
the `meshtasticd_2.5.19.f9876cf_amd64.deb`
in release **v2.5.19.f9876cf** — the newest
release still carrying the Linux package as
a download at build time; the project's
documentation now points to package
repositories for current builds
(meshtastic.org/docs/meshtasticd/). The
binary's own version string is modest:

```
$ meshtasticd --version
portduino 0.1.0

$ meshtasticd --help
Usage: meshtasticd [OPTION...]
  -c, --config CONFIG    Use a non-default config file
  -d, --fsdir FS_DIR     Specify a directory for the file system
  -e, --erase            Erase the file system on startup
  -h, --hwid HWID        The mac address is derived from this number
  -p, --port PORT        The TCP port to use for the phone API
  -V, --version          Print version information
```

(help text trimmed to the option block; the
full capture is transcript t02.) The real
firmware version — 2.5.19.f9876cf — shows up
where it matters, in the node's metadata
(chapter 7). Four flags run the whole lab:
`-c` the config, `-d` the state directory,
`-h` a hardware id that seeds the node's
identity, `-p` the API port.

## Configuration: a radio by omission

The daemon's YAML config names the hardware
hung off the host: GPIO, I2C, display, and,
under `Lora:`, a radio module. The shipped
file lists real modules (sx1262, sx1268,
sx1280, RF95, and others) as commented
examples. The lab's config, kept in
`examples/meshtasticd-config.yaml`, defines
**no module at all** — and that omission is
the whole trick. State lives in the `-d`
directory as protobuf files (`prefs/db.proto`
and friends), created on first boot and
reloaded on every later one.

## The first boot, in its own words

Excerpts from node A's startup log
(transcript t17), in order:

```
MAC ADDRESS: 80:00:00:00:00:01
INFO  | 22:27:19 0 Install default LocalConfig
INFO  | 22:27:19 0 Generate new PKI keys
INFO  | 22:27:19 0 Set radio: region=UNSET, name=LongFast, config=0, ch=19, power=30
INFO  | 22:27:19 0 frequency: 906.875000
INFO  | 22:27:19 0 Use SIMULATED radio!
INFO  | 22:27:19 0 Web Server framework started on port: 443
INFO  | 22:27:19 0 API server listen on TCP port 4403
```

Check against chapter 6: region UNSET at
birth, LongFast preset, and 906.875 MHz —
exactly the frequency the project's
radio-settings page computes for the US
default slot. The daemon also starts an
HTTPS web server; ours answers on 443 but
serves `File not found` at `/` — the web
client's files are a separate piece the
standalone package didn't bundle
(transcript t16).

Thirty seconds after boot the node
introduces itself to the (empty) mesh:

```
INFO  | 22:27:49 30 [NodeInfo] Send our nodeinfo to mesh (wantReplies=1)
INFO  | 22:27:49 30 [NodeInfo] Send owner !52282ebc/Meshtastic 2ebc/2ebc
```

## Two instances: what the lab proved

Node B ran with `-h 2 -p 4404` and its own
state directory, and was configured exactly
as chapter 3 describes (owner Barn, region
US). Then both sat for over a minute, and
each was asked for its node list. Node A:

```
│   1 │ Farmhouse │ !52282ebc │ FARM  │ PORTDUINO  │ ... │ 0 │ * │ 2026-10-21 22:31:15 │ now     │
```

Node B's table was its mirror image: one
row, itself. **Neither ever heard the
other.** No sockets join the simulated
radios — the sandbox host showed no UDP
listeners at all — and the delivery tests
of chapter 4 (a broadcast sent while B
listened; a direct message that ended in a
NAK) confirm it from both ends. In this
firmware version, in this environment, the
simulated radio is a loopback: a node
talking the full protocol to itself and to
whatever client connects. Everything the
CLI did *to* each node — identity, region,
config export, send queueing, retry and
NAK logic — is genuine firmware behaviour,
verified. Everything *between* nodes is
not demonstrated here, and this guide
marks it documented-only wherever it
appears.

Two smaller collisions worth knowing if
you run your own pair: both daemons try to
serve the web UI on 443, and only the first
gets it (B's log: `Error starting Web
Server framework`); and each daemon serves
one API client at a time (chapter 7).

## On real hardware

Bolt a supported LoRa module to a Pi, name
it under `Lora:` in the config, and the
same daemon becomes a fixed node with a
real radio — the path the project documents
for routers and gateways. That step was not
run here (no module was available to the
build) and is listed as documented-only in
`criteria.md`. The simulated radio is a
test bench, not a substitute: it verifies
everything except the air.
