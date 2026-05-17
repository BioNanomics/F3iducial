# Architecture

## Drive Base

F3iducial supports FTC-sized and FRC-sized swerve platforms.

Typical features: 4 swerve modules, IMU (Pigeon2 or equivalent), low-speed calibration mode, safety perimeter sensors, companion compute.

**Reference:** [FTC Drive Base Reference Video](https://youtu.be/aETaRclTDDo?si=y34b_Y77TWr-n3QT)

## Detachable Probe Arm

The probe module mounts externally where bumpers would normally attach.

- Detachable quick-mount interface
- Fixed horizontal overhang
- Manual XY adjustment
- Motorized Z-only movement (worm-gear lift)
- Camera mounted directly on arm
- Horizontal side probe (not downward)

The probe is intentionally simple and robust — no articulated arm, no inverse kinematics, no multi-axis manipulation.

## Vision System

F3iducial uses AprilTags for localization, field alignment, pose correction, and geometry verification.

Supported stacks: PhotonVision, Limelight, OpenCV AprilTag, custom pipelines.

## Design Principles

Simple mechanisms · High rigidity · Repeatable geometry · Low-speed operation · Minimal moving axes · Modular hardware · Field-safe probing · Serviceable at events
