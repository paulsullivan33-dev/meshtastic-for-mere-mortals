# Privacy, Encryption and the Rules

**Scope:** what the encryption actually
protects, what it doesn't, what a mesh can
see about you, the one setting that reaches
the internet, and the legal frame — stated
plainly and briefly, because region rules
vary and this chapter is not a lawyer.

## Two kinds of encryption, honestly labelled

**Channels** are encrypted with their PSK
(chapter 4) — AES, with 128- or 256-bit keys
per the project's radio-settings page.
Whoever holds the key reads the channel;
whoever doesn't, can't. That is exactly as
strong as your key hygiene and no stronger:
a key shared by screenshot with forty
people is a public channel with extra
steps. And the **default channel's key is
published in the documentation** — treat
everything on it as posted on a noticeboard
(chapter 4 said it; this is the chapter
that means it).

**Direct messages** use each node's own
key pair (the firmware calls it PKC;
chapter 7's info dump shows `"hasPKC":
true`). Your node's public key travels with
its identity; its **private key never
should** — and it sits in plain text inside
every `--export-config` you save (chapter 7
shows the line). Guard exports like
passwords, and don't paste them into bug
reports.

## What the mesh can see even so

Encryption covers message *contents* on a
keyed channel. It does not hide:

- **That you exist.** Node IDs, long and
  short names, and hardware models are
  broadcast in node info for anyone in
  range to collect.
- **Where you are, if you share position.**
  A GPS node with position sharing on
  broadcasts its location to the channel —
  that's the feature, and it's on the
  primary channel by default. The position
  settings let you turn broadcasting off or
  coarsen its precision; a node with no
  need to be found should.
- **That you're talking, and roughly how
  much.** Relayed packets are visible to
  every node that relays them; contents on
  a private channel stay sealed, traffic
  patterns don't.

## The internet door: MQTT

A node with internet can bridge channels to
an **MQTT** broker — the lab node's defaults
name the project's public broker,
`mqtt.meshtastic.org`, topic root `msh`,
username `meshdev`. A channel reaches the
internet only if someone enables **uplink**
on it, and comes back only with **downlink**
— but once uplinked, its traffic (positions
included) is republished to the broker and
typically onto public mesh maps for anyone
to browse. That is how community maps get
their dots, and it is a publishing decision,
not a networking detail. Our lab never
uplinked (the sandbox, and no reason to);
MQTT behaviour is documented-only in this
guide's ledger.

## The rules, briefly

- **Region is law-shaped.** The region
  setting (chapter 6) picks your band,
  power ceiling, and duty cycle to match
  licence-free rules where you are. It is
  your job to set it where you actually
  are, and the firmware's ceiling is not
  your national limit — the project's own
  page says to check your national band
  plan.
- **Duty cycle is a shared budget.** In the
  European 868 and 433 bands the limit is
  10% of airtime over a rolling hour, and
  the firmware enforces it by going quiet.
  Presets and chatty telemetry spend from
  the same purse.
- **Amateur allocations are a different
  world.** The firmware also offers
  ham-band regions and a licensed-operator
  mode. Those require an amateur licence,
  forbid encryption (that's why channels
  can turn it off), and require you to
  identify with your call sign. No licence,
  no exceptions, no "just testing."
- **Physical safety is short.** Antenna on
  before power (chapter 3). Lithium cells
  get the usual respect: no punctures, no
  charging unattended on the sofa, swollen
  battery means done.

Check: after this chapter you should be
able to say which of your channels a
stranger with a node can read (the default
one), what your exports contain (your
node's private key), and which single
setting publishes your mesh to the internet
(MQTT uplink). If any answer surprises you,
it was worth the chapter.
