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
robot. This was retuned frequently using an autonomous procedure. 

**Odometry** — Wheel-encoder-based dead reckoning tracking the robot's pose
on the field. Used a setup with a tracking wheel attached to an encoder. 

**Localization** — A particle filter using Monte Carlo localization, fusing
odometry with distance sensor and IMU readings to correct drift against known
field geometry.

Result: scoring runs repeatable to roughly an inch, and reduced routine
runtime.

## Attribution

355U was a team effort. I co-founded the team and led software; Oliver and Adhrit worked on building and driving. The team shared a laptop for all
development, so commit history in the linked repositories is attributed to a
single account rather than to individual contributors. The localization and
motion control work described above is mine.

## Source by season

| Season | Repository | Notes |
|---|---|---|
| Over Under (2024) | https://github.com/OliverCieslak/355U | introduce PID | 
| High Stakes (2025) | https://github.com/OliverCieslak/355U_HighStakes | introduce particle filter |
| Push Back (2026) | https://github.com/OliverCieslak/355U_PushBack | Switch to separate odometry wheel |

Built with [VEXcode V5 / PROS — confirm which] in C++.
