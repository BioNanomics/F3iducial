# F3iducial

> FIRST Field Fiducial — Open-source field calibration, localization, and verification tooling for FRC and FTC.

F3iducial is a modular open-source robotics platform combining a compact swerve-drive robot, AprilTag vision, inertial localization, and a detachable probe module to measure and validate real-world field geometry against expected layouts.

**F3iducial is NOT a competition robot. It is a mobile field instrumentation platform.**

See [docs/use-cases.md](docs/use-cases.md) | [docs/architecture.md](docs/architecture.md) | [docs/probe-system.md](docs/probe-system.md) | [docs/contributing.md](docs/contributing.md)

---

## Why?

Every robotics team has experienced it: the field was assembled slightly wrong, a wall shifted, an AprilTag moved, autonomous worked at home but not at competition.

Modern FIRST robots depend heavily on accurate field geometry. A few centimeters of error can cause missed autonomous routines, inconsistent path following, inaccurate AprilTag localization, and endless debugging during events.

Today, field verification is mostly tape measures, eyeballing, and guesswork. F3iducial exists to change that.

---

## Core Workflow

1. Calibrate from a known AprilTag
2. Establish field-relative pose
3. Navigate to expected field elements
4. Probe or inspect those elements
5. Record offsets and deviations
6. Generate reports or correction data

---

## Planned Features

**Localization:** AprilTag localization, odometry fusion, IMU integration, pose estimation

**Inspection:** tag verification, field geometry checks, wall alignment validation, scoring element inspection

**Calibration:** robot-to-field alignment, autonomous offset generation, localization characterization

**Reporting:** deviation heatmaps, inspection logs, field error reports, calibration exports

---

## Hardware

F3iducial supports both FTC-sized and FRC-sized swerve platforms. See [docs/architecture.md](docs/architecture.md) for full mechanical details.

- [FTC Drive Base Reference Video](https://youtu.be/aETaRclTDDo?si=y34b_Y77TWr-n3QT)

---

## Project Status

Early concept / proof-of-concept. Currently exploring detachable probe module, swerve localization, AprilTag alignment workflows, field inspection architecture, and calibration data models.

---

## License

TBD — potential directions: Apache 2.0, MIT, CERN-OHL-S

---

## The Goal

Reliable autonomous behavior starts with reliable field geometry. F3iducial helps teams understand the field they are actually playing on.
