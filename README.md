# Saathi

*saathi* — Nepali for companion.

A robot hand whose only job is to hold a trapped person's hand and talk to them,
in their own language, with no network, until the diggers reach them.

It does not lift. It does not pull. It does not extract.

---

## Why

On 26 August 2026 a 600 m section of glacier and bedrock sheared off at 5,200 m
above the Nepal–Tibet border and fell 1,200 metres into the Lhende valley. It
dammed the river, the dam burst, and the surge ran down the Bhote Koshi and the
Trishuli. 669 people are confirmed dead, around 2,900 are still missing, and the
Red Cross puts the affected population at 93,000. Forty-two kilometres of road
and every bridge on that corridor are gone.

Extraction from collapsed structures takes hours. People trapped in rubble die of
shock, hypothermia and despair as much as of injury, which is why the standing
practice for urban search and rescue teams is to keep someone talking to the
casualty continuously for the whole dig.

Right now that job requires a human being to lie in a void in an unstable
structure for six hours, in a valley where two barrier lakes are still filling
upstream and one of them already broke its banks and suspended the rescue.

Saathi does that job instead.

## What it actually does

One motion and one behaviour.

1. A rescue team pushes the arm into a void too small for a person.
2. It closes its gripper slowly until it meets resistance, then **stops**. It
   never commands a position past the point of contact.
3. It holds at that opening and speaks, on a loop, in Nepali.
4. If the person pulls their hand away, it notices and opens.

The force limit is the whole safety argument, so it is the part with a test
behind it. Squeezing a hand is a failure, not a success.

## Why touch and not vision

A camera cannot see into a void, cannot see around the bend of a collapsed
passage, and does nothing in the dark or in silt. Position control cannot tell
you that you have found a hand. Only load can.

The gripper motors already report load; the value is simply not exposed by
default. `Present_Load` sits at register 60 and is signed with the direction flag
on bit 10, which is why `read_load()` masks with `0x3FF` — without that mask a
joint at rest reads 1000 instead of 0.

## Why the voice runs on the device

There is no working connectivity in that valley. A cloud voice assistant is not
a degraded option there, it is a non-functional one. Phrases are short, because
someone in pain and frightened cannot follow sentences.

| Nepali (romanised)        | English                      |
| ------------------------- | ---------------------------- |
| ma yahaa chu              | I am here.                   |
| uddhaar aayirakheko chha  | Rescue is coming.            |
| mero haat chhodnuhos na   | Do not let go of my hand.    |
| saas pheri rahanuhos      | Keep breathing.              |
| timi eklai chhainau       | You are not alone.           |

## Run it

Nothing here needs a robot to run.

```bash
python3 -m saathi.hold --self-check     # assertions on the hold logic
python3 -m saathi.hold --dry-run        # full loop against a simulated hand
python3 -m saathi.voice                 # phrase loop
```

On hardware, with an SO-101 follower:

```bash
python3 -m saathi.hold --port /dev/tty.usbmodemXXXX --id saathi --seconds 30
```

## Honest state of this

Built and working: the force-limited hold, the withdrawal detection, the phrase
loop, and the tests behind them.

Not built: the tracked chassis, autonomous approach, locating a hand rather than
having one placed in the gripper, and offline Nepali speech synthesis (the demo
plays pre-recorded audio; the architecture for on-device generation is described
above but not implemented).

The arms available at this hackathon are also far too heavy and too fragile for a
collapsed structure. The behaviour is the contribution; the platform is not.

## Track

Track 2, Action. Plus the voice challenge.
