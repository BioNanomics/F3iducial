# Architecture

## Drive Base

F3iducial supports FTC-sized and FRC-sized swerve platforms.

Typical features: 4 swerve modules, IMU (Pigeon2 or equivalent), low-speed calibration mode, safety perimeter sensors, companion compute.

**Reference:** [FTC Drive Base Reference Video](https://youtu.be/aETaRclTDDo?si=y34b_Y77TWr-n3QT)

## Detachable Probe Module

The probe module mounts externally where bumpers would normally attach.

- Detachable quick-mount interface
- **Fixed horizontal overhang** — no extension or retraction in tour
- Manual XY adjustment (set at mount time, not driven)
- **Motorized Z-only movement** (worm-gear lift) — the only powered probe axis
- Camera mounted directly on the module
- Horizontal side probe (not downward)
- Contact with a target is made by **driving the chassis**, not by moving the probe

The probe is intentionally simple and robust — no articulated arm, no inverse kinematics, no multi-axis manipulation, and no automatic retract on error (the robot stops and reports instead).

## Vision System

F3iducial uses AprilTags for localization, field alignment, pose correction, and geometry verification.

Supported stacks: PhotonVision, Limelight, OpenCV AprilTag, custom pipelines.

## Design Principles

Simple mechanisms · High rigidity · Repeatable geometry · Low-speed operation · Minimal moving axes · Modular hardware · Field-safe probing · Serviceable at events
