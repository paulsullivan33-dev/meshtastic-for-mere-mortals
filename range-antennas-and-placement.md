# Range, Antennas and Placement

**Scope:** the honest version of "how far
does it go" — why no single number exists,
the parts that actually decide range
(height, line of sight, the antenna, the
preset), and how to spend effort where it
pays. This guide quotes **no** range record
and repeats **no** marketing distance,
because neither would help you plan.

## Why every range claim needs an asterisk

Range is not a property of the radio. It's
a property of the whole path: both antennas,
their heights, everything between them, and
the radio settings at both ends. The same
pair of nodes that swaps messages across a
valley from two hilltops can fail between
two streets in a city — buildings, wet
trees, and a band full of other people's
equipment eat signal that open air doesn't.
Online range claims are usually true
*somewhere*: a record set over water or from
a mountain says almost nothing about a farm
with a metal barn in the middle.

The project's own documentation describes
LoRa as reaching "several kilometers" —
deliberately vague, and the right register.
Plan in those terms: the question is never
"how far does it go" but "can *this* node
hear *that* node, from *these* two spots."

## The budget you actually spend: link budget

Engineers compare setups with a **link
budget**, in decibels (dB): how much signal
loss the link can tolerate before it fails.
The project's preset table (chapter 6)
publishes one per preset — LongFast, the
default, is rated 153 dB — computed at a
stated transmit power and a plain 0 dB
antenna, from the chip maker's own
calculator. Treat those numbers the way the
documentation presents them: as *relative*
comparisons. A slower preset buys a few dB
(more tolerance for a bad path) at the cost
of speed; a better antenna buys dB without
costing speed; height buys the most of all
and costs only effort.

Three rules of thumb from that same page,
worth more than any record:

- Each step up in spreading factor roughly
  **doubles airtime** and adds about
  **2.5 dB** of link budget.
- Each **doubling of bandwidth** costs
  almost **3 dB**.
- Presets are therefore a trade you make
  once, for the whole mesh — every node
  must share a preset to hear the others.

## Height and the fat cigar

Radio at these frequencies mostly wants
**line of sight**, and more than the thin
line you'd draw on a map: the signal
occupies a plump, cigar-shaped region
around the straight path (the Fresnel
zone), and obstacles poking into it —
rooftops, ridges, tree lines — weaken the
link even when the endpoints can literally
see each other. Practical consequences:

- **Height is the cheapest range.** A node
  on a roof, a mast, or a ridge beats the
  same node with a bigger antenna at ground
  level, almost every time. This is why
  meshes grow around one well-placed relay
  (chapter 6's Router role) instead of many
  strong handsets.
- **Indoors costs.** Walls, and especially
  metal — barn siding, vehicles, foil-backed
  insulation — can end a link that works
  fine from the yard. Put the antenna
  outside, or accept the tax.
- **Test from the real spots.** The two-node
  test from chapter 2 exists for this: carry
  one node to the gate, the far field, the
  other hiker's trailhead, and read the
  signal figures the app reports for what
  actually arrived.

## Antennas, without the folklore

- **The included antenna varies from fine
  to poor.** A stubby included antenna is
  the first suspect when two nodes that
  "should" reach don't. A proper antenna
  for *your band* (chapter 2 — an 868 MHz
  antenna on a 915 MHz node is a dummy load
  with good PR) is the standard first
  upgrade.
- **Gain is a shape, not a volume knob.** A
  higher-gain antenna doesn't amplify; it
  flattens the radiation pattern — more
  signal toward the horizon, less up and
  down. On flat ground that's a win. On a
  hilltop talking to a valley, a high-gain
  antenna can flatten your signal right
  over the heads of the nodes below.
- **Cable is loss.** Long thin coax between
  radio and antenna gives back much of what
  the antenna gained, especially at 900
  MHz. Keep the run short, or move the
  whole node to the antenna and run data
  and power instead.
- **Never transmit without one.** Chapter
  3's rule zero bears repeating here: the
  antenna goes on before the power, always.

## Placement for our two running examples

**The farm.** Farmhouse node inside a
window is a fair start; the same node with
its antenna outside the wall is better. If
the barn's metal skin blocks the house,
don't buy power — raise one node: a Router
on the silo, the windmill, or a mast where
it can see both buildings turns two failing
links into two easy ones.

**The hikers.** Ridges are assets and
valleys are shadows. Two hikers who lose
each other in a valley often both regain
the mesh at the next rise, because a
stranger's node on a summit — or their own
third node, left at camp on high ground —
is relaying. Expect dropouts; the medium
(chapter 4) does not store messages for
nodes that missed them.

Check: before buying anything for range,
you should be able to name which of the
three you'd change — preset, antenna, or
placement — and placement should be the
first word out of your mouth.
