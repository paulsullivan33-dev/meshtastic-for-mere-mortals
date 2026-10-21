# Writing Criteria

Every guide in this repo is written to the same
standard. If you're adding or editing a guide —
human or AI — hold it to these.

1. **Plain words, real ideas.** Simple language,
   never dumbed-down concepts. Explain jargon on
   first use; collect terms in the glossary.
2. **No hype.** No marketing adjectives, no
   "revolutionary," no promises about what
   something will do for the reader.
3. **Honest about limits.** Say what the thing
   *can't* do. State uncertainty instead of
   smoothing it over. Validation notes include
   what remains unverified.
4. **No invented numbers.** Benchmarks, defaults,
   and specs are either measured or qualified with
   "depends on your setup." If it wasn't tested,
   say so.
5. **Checkable.** Commands are meant to be run,
   with "Check:" blocks showing what success looks
   like. Sources are linked, not asserted.
6. **Respect the reader's setup.** Call out
   Windows/Mac/Linux differences. Don't assume a
   GPU, a budget, or prior experience.
7. **Safety isn't skipped.** Real risks get
   stated plainly, not buried.
8. **Scoped.** Each guide says what it covers and
   points elsewhere for the rest. No guide
   pretends to be the whole subject.
9. **Structure over prose.** Tables, before/after
   examples, symptom → fix. Scannable beats essay.

## Verification notes for this guide

Standard 3 and 4 applied to this guide,
specifically. There was **no radio hardware**
on the build bench. What ran was software:
the official Python CLI, **meshtastic 2.7.11**
(pip, in a venv), and the Meshtastic firmware
built as a Linux daemon — **meshtasticd,
firmware 2.5.19.f9876cf** (the binary reports
`portduino 0.1.0` for itself) — from the
official `meshtasticd_2.5.19.f9876cf_amd64.deb`
in the firmware project's GitHub releases,
extracted (not installed) into the build
directory and run unprivileged. Every quoted
CLI output in the chapters is a capture from
those two programs; working transcripts are
kept with the build
(`meshtastic-lab/transcripts/`, t00–t17).

**The lab.** Two daemon instances on one
host, each with its own config file and state
directory, no LoRa module configured — so
each fell back to the firmware's simulated
radio:

- *Node A:* `-h 1 -p 4403`, node
  `!52282ebc`, named **Farmhouse / FARM**.
- *Node B:* `-h 2 -p 4404`, node
  `!200bf9ba`, named **Barn / BARN**.

Node A's config is kept verbatim at
`examples/meshtasticd-config.yaml`. Both
daemons were driven over localhost TCP by
the CLI. The lab's central negative result:
**the two nodes never heard each other.**
After both were configured and left running,
each node's list contained only itself
(t11, t12); a broadcast sent from A while B
listened arrived nowhere (t13, t14); a
direct message with `--ack` ended in
`Received a NAK, error reason:
MAX_RETRANSMIT` (t15). The host had no UDP
listeners at all during the tests (`ss`
checked), i.e. this firmware's simulated
radio opens no socket a second instance
could share, at least in this sandbox.
Accordingly: everything *to* a node is
verified; everything *between* nodes is
documented-only in this guide, and the
chapters say so where it matters rather
than performing a delivery that didn't
happen.

**Verified by running, per chapter:**

- *Ch. 1:* the framing facts are conceptual;
  the version and identity figures come from
  t00, t02, t03.
- *Ch. 3:* `--set lora.region US` with its
  four output lines (t08) and the read-back
  showing `region: US` in the export (t09);
  `--set-owner "Farmhouse"` / `"Barn"` with
  read-backs in `--info` (t07, t09, t10).
  Flashing and app pairing are not in this
  list — see Documented-only.
- *Ch. 4:* the `channel_url` from A's
  export (t05); broadcast `--sendtext` with
  its two output lines (t13); the direct
  `--sendtext --dest … --ack` sequence
  ending in the NAK (t15); `--listen` on B
  printing nothing for 25 s during A's send
  (t14). The frequency claim (US default
  slot 20, 906.875 MHz) is from A's startup
  log (t17) and matches the project's
  radio-settings page.
- *Ch. 6:* `--get device.role` (0),
  `--get lora.region` (0 before, and the
  named read-back in t09 after),
  `--get lora.modem_preset` (0),
  `--get lora.hop_limit` (3) — all t06.
  The region and preset tables are the
  project's published ones (sources below),
  not measurements.
- *Ch. 7:* `--version` (t00); the 344-line
  `--help` surface (t01); `--info` against
  a live node (t03), including the
  `"hasPKC": true` and firmware metadata;
  `--nodes` tables (t04, t11, t12);
  `--export-config` in full (t05);
  `--get`/`--set` round trips (t06–t10);
  the `Broken pipe` heartbeat traceback,
  observed across t04, t06 and t07, always
  with the command completing correctly;
  the daemon log's `Force close previous
  TCP connection`, the one-client rule.
- *Ch. 8:* `meshtasticd --version` and
  `--help` (t02); the startup log (t17):
  default-config installation, PKI key
  generation, `Set radio: region=UNSET,
  name=LongFast`, `frequency: 906.875000`,
  `Use SIMULATED radio!`, API on 4403;
  state persistence across the region-set
  reboot (`Loaded /prefs/db.proto
  successfully`, node list intact); the
  second instance's webserver failure on
  the already-bound port 443 (B's log, in
  t17); the HTTPS server answering on 443
  with `404 File not found` at `/` — the
  web client's files are not in the
  standalone package (t16).
- *Ch. 9:* the key pair's presence in the
  export's `security:` section and the
  public key in `--info` (t03, t05); the
  MQTT module defaults printed by the same
  export (`mqtt.meshtastic.org`, root
  `msh`, username `meshdev`). Nothing was
  uplinked.
- *Ch. 10:* the recipes assemble only the
  commands above; the one exception is
  flagged in its own text — the CLI's
  channel-editing flags (`--ch-add`,
  `--ch-set psk random`) are quoted from
  `--help` (t01), not run.
- *Ch. 12:* the troubleshooting entries
  drawn from the lab cite their transcripts
  in place (t04–t09, t15, t17, and B's
  startup log); the remainder are marked
  by their wording as documented behaviour.

**Sources for the documented material,**
consulted 2026-10-21: the project's LoRa
configuration page (region table, duty-cycle
rule, the unset-region warning, preset
legality) —
https://meshtastic.org/docs/configuration/radio/lora/ —
and its radio-settings overview (preset
data rates and link budgets, the 906.875 MHz
slot calculation, channel cryptography
notes) —
https://meshtastic.org/docs/overview/radio-settings/.
Link budgets and data rates are the
project's *computed* figures (Semtech
calculator, stated transmit power and a
0 dB antenna) and are presented as
comparisons, never as measured range.

**Capture hygiene (declared trims):** the
lab node's **private key is redacted** in
t05, replaced in place by a marker; the key
was generated for this lab, belongs to no
real deployment, and is redacted anyway
because the guide teaches that exports are
secrets. The MQTT password line in t05
shows the CLI's own `<redacted>`. In
chapter 7, the `--info` JSON is trimmed
with `...` ellipses inside the braces; in
chapter 8, the `--nodes` table is trimmed
to the self row with a `...` where columns
were elided — full tables are t11/t12.
t17 is labelled excerpts of the daemon
logs. t09 and t10 were captured through
shell pipelines (`--info | head`,
`--export-config | grep`); the pipelines
are printed in the transcripts themselves.
One line in t15 — `(exit 0 — timed out
waiting for an ack that cannot come)` — is
the builder's annotation, in the file as
captured. No output was otherwise edited,
reordered, or cleaned up; the `Broken pipe`
tracebacks were left in t04, t06 and t07
because chapter 12 teaches from them.

**Build-machine weather, stated once:** the
sandbox restricts some network operations;
loopback TCP (all the lab used) worked, and
no UDP socket existed on the host during
the delivery tests. The daemon binary
needed libraries the sandbox lacked —
libgpiod, libyaml-cpp, libi2c, libulfius,
liborcania, libyder, libmicrohttpd — fetched
as distribution packages, extracted beside
the binary, and supplied via
`LD_LIBRARY_PATH`; the daemon itself is
otherwise the project's unmodified release
binary. Daemon clocks in the logs are the
sandbox's (CDT, 2026-10-21).

**Documented-only (not run), and why:**

- *Phone apps (Android, iOS, macOS):* no
  phone or radio was available to the
  build. App behaviour, settings layout,
  and pairing are described from the
  project's documentation; platform
  differences are stated where the docs
  state them.
- *Flashing real devices:* no hardware.
  The web flasher, the ESP32 serial route,
  and the nRF52840 UF2 route are described
  from the project's getting-started
  documentation; nothing about a flash is
  quoted as output.
- *Node-to-node delivery, relaying, and
  traceroute:* attempted in the lab and
  **not achieved** — the simulated radios
  did not interconnect (see The lab).
  Flooding behaviour, hop mechanics, and
  delivery expectations are described from
  the documentation; the only delivery
  output quoted anywhere is the failure.
- *Real RF behaviour and range:* no radios,
  no measurements. The guide quotes no
  range figure at all except the project's
  own deliberately vague "several
  kilometers," and presents its preset
  table as computed comparison data.
- *The daemon on real radio hardware*
  (a Pi with an SPI LoRa module): no
  module available. The config that names
  one is quoted from the shipped defaults;
  the path is described, not walked.
- *MQTT gatewaying and public maps:*
  nothing was uplinked; the lab had no
  reason to publish and the sandbox no
  business doing so. Behaviour is from the
  module's documented defaults, which the
  export did verify (above).
- *Channel editing by CLI:* the flags are
  quoted from `--help` (t01). Creating the
  guide's private channel on the daemon
  would have proven a menu, not a mesh;
  the app path is the one chapter 10
  walks, and it is documented-only there
  too.
- *The neighbouring systems of chapter 11:*
  described from their own projects'
  public documentation; none installed.

Specimen discipline, for the record: the
farm, its buildings, and the hikers are
invented; the lab nodes' names and IDs are
their own, created for this guide; the one
real secret the lab generated (node A's
private key) is redacted in the transcript
and was never used anywhere else. No
command in this guide was aimed at any
system outside the lab directory and its
two daemon processes.
