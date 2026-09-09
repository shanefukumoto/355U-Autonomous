# VEX Robotics 355U — Autonomous Localization & Control

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

**Motion control and odometry** — Drive and turn PID and wheel odometry come
from [LemLib](https://github.com/LemLib/LemLib). My work here was
configuration and tuning, retuned frequently against an autonomous test
routine. In the 2025–26 season we added a dedicated tracking wheel on its own
encoder to improve odometry accuracy.

**Localization** — A particle filter using Monte Carlo localization, written
for this robot. It fuses LemLib's odometry with distance sensor and IMU
readings to correct drift against known field geometry.

**Real-time optimization** — The V5 brain runs a fixed control loop, so the
filter has a hard per-update time budget. Profiling showed the sensor update
dominating, so I replaced `std::exp` with a fast approximation
(`utils::fastExp`) and the trig calls with `fastSin`/`fastCos`:

| | Motion update | Sensor update | Total |
|---|---|---|---|
| Baseline (300 particles) | 3.59 ms | 13.65 ms | 18.38 ms |
| After fast math (300 particles) | 2.57 ms | 8.49 ms | 11.34 ms |
| After `fastNorm` (1000 particles) | 3.82 ms | 4.15 ms | 8.64 ms |
| Final (500 particles) | 1.92 ms | 2.07 ms | 4.32 ms |

Net effect: the filter got faster while the particle count went up more than
3x, which meant a better position estimate inside the same loop budget.

Result: scoring runs repeatable to roughly an inch, and reduced routine
runtime.

## Source by season

| Season | Repository | Notes |
|---|---|---|
| Over Under (2023–24) | [355U](https://github.com/OliverCieslak/355U) | First season; drive PID |
| High Stakes (2024–25) | [355U_HighStakes](https://github.com/OliverCieslak/355U_HighStakes) | Migrated from VEXcode V5 to PROS; adopted LemLib for motion control |
| Push Back (2025–26) | [355U_PushBack](https://github.com/OliverCieslak/355U_PushBack) | Dedicated tracking wheel; filter optimization |

Written in C++. The first season used VEXcode V5 with the JAR Template; we
migrated to PROS for the 2024–25 and 2025–26 seasons.

## Attribution

355U was a team effort. I co-founded the team and led software; Oliver and
Adhrit worked on building and driving. The team shared a laptop for all
development, so commit history in the linked repositories is attributed to a
single account rather than to individual contributors. The particle filter,
its optimization, and the control tuning described above are mine.
