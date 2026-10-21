# Recipes

**Scope:** the jobs people actually set a
mesh up for, assembled from the earlier
chapters. Commands are the verified ones
from chapters 7 and 8 unless a step says
otherwise; app steps are described, per the
ledger in `criteria.md`.

## Two nodes, one farm (the proof walk)

Goal: prove your own ground before buying
a third node.

1. Set both nodes up per chapter 3: antenna
   on, region set, names set — "Farmhouse"
   and "Barn," say. Leave preset, role, and
   hops at defaults (chapter 6).
2. Put node B in the barn, node A in the
   farmhouse in its intended spot — antenna
   outside the wall if that's the plan
   (chapter 5).
3. Send a direct message with `--ack` from
   the app, or from the command line:

   ```
   $ meshtastic --sendtext "Barn, do you copy?" --dest "!200bf9ba" --ack
   ```

   (your destination ID will differ — read
   it from `--nodes`.) An ack is proof, not
   hope. Our lab's version of this command
   is transcript t15; it ends in a NAK
   because the lab has no air. Yours should
   end in silence and success.
4. No ack? Move placement before settings:
   raise A, move B out from behind the
   metal wall, retest (chapter 5). Only
   when placement is exhausted is a third
   node — on the high point between them,
   role `ROUTER` — the answer.

## A private channel for your group

Goal: a channel only your people can read,
next to the public one.

1. In the app, add a channel in slot 1 and
   let it generate a **random** key. (The
   command line spells the same job
   `--ch-add NAME` then `--ch-set psk random`
   — flags quoted from `--help`, transcript
   t01; the app path is the one this recipe
   walks.)
2. Share it by QR code or link (chapter 4)
   with each member's node, in person where
   you can. A channel link is a key: send
   it the way you'd send a password.
3. Keep channel 0 as it is. The default
   channel is your public square and your
   discovery tool; privacy lives in slot 1,
   not in locking the square.
4. Someone leaves the group? The key can't
   be un-shared. Make a new channel with a
   new key and move — the mesh equivalent of
   changing the locks.

## The ridge relay

Goal: extend a mesh over a barrier — the
hikers' version of the farm's third node.

1. Pick the spot by chapter 5's rules: the
   point that can *see* both sides. Test
   with a handset first; human-height at the
   summit predicts mast-height poorly, but
   failure at the summit is conclusive.
2. The relay node: role `ROUTER` (chapter
   6), position fixed, power sorted for
   unattended weeks — solar kits exist for
   the common boards; the node itself is
   the cheap part of this build.
3. Name it for the place ("Ridge"). Mark it
   a favourite in your app so its battery
   and last-heard are the ones you watch.
4. Resist a second relay until traffic
   proves the need: every always-on relay
   spends airtime on everything it hears
   (chapter 6's warning).

## Back up a node before you tinker

Goal: a restorable copy of a node's whole
configuration before experiments.

```
$ meshtastic --host localhost --export-config > farmhouse-config.yaml
```

Store the file like a password — it
contains the node's private key (chapters
7 and 9). Restoration is the reverse
direction (`--seturl`-style imports and
the app's config import exist; the export
direction is the half this guide verified).
Do one per node, dated, before any
afternoon of "just trying" a setting.

## Is it me, or is the mesh quiet?

Goal: tell a local fault from an empty
neighbourhood, in order.

1. `--nodes`: are the entries you expect
   there, and how old is **LastHeard**? A
   mesh you haven't heard in days may just
   be a mesh of strangers who went home.
2. Read back your own radio settings
   (chapter 6's three): region, preset,
   hops. A node deafened by its own preset
   change looks exactly like an empty mesh
   from inside.
3. Antenna connected, and the right one
   (chapter 3's rule zero, chapter 5's
   band warning).
4. Send to `^all` on channel 0 and watch
   `--listen`… on a *second* node if you
   have one. One node cannot hear itself
   prove anything.
