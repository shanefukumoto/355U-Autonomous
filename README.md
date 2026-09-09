# VEX Robotics 355U — Autonomous Control Stack

Autonomous software for the competition robots of VEX Robotics team 355U,
which I co-founded and programmed from 2023 to 2026. Over three seasons the
team qualified for the Illinois state championship three times and the VEX
Robotics World Championship twice.

## The problem

VEX matches open with a 15-second autonomous period where the robot scores
with no driver input. Wheel odometry alone accumulates enough error over
those 15 seconds that scoring positions drift out of tolerance, so routines
that worked in practice fail in matches. Most of this code exists to make the
robot's estimate of its own position hold up for the full period.

## What's in the stack

**Motion control** — PID controllers for drive and turn motion, tuned per
robot. [One sentence on how you handled tuning across robot revisions.]

**Odometry** — Wheel-encoder-based dead reckoning tracking the robot's pose
on the field. [Note your tracking wheel setup if you had one.]

**Localization** — A particle filter using Monte Carlo localization, fusing
odometry with distance sensor and IMU readings to correct drift against known
field geometry.

Result: scoring runs repeatable to roughly an inch, and reduced routine
runtime.

## Source by season

| Season | Repository | Notes |
|---|---|---|
| [2023–24 game name] | [link] | [what was new that season] |
| [2024–25 game name] | [link] | |
| [2025–26 game name] | [link] | |

Built with [VEXcode V5 / PROS — confirm which] in C++.

## Attribution

355U was a team effort. I co-founded the team and led software; [teammate
names] worked on [build/design/driving]. The team shared a laptop for most
development, so commit history in the linked repositories is attributed to a
single account rather than to individual contributors. The localization and
motion control work described above is mine.
