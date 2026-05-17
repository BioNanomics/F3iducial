# Probe System

## Overview

The probe is designed for robustness, repeatability, and field safety — not precision metrology.

Target accuracy: ±1–2 cm repeatability.

## Probe Tips

- Soft Delrin tip
- Rubber contact tip
- Electrical contact probe
- Interchangeable field-safe tips

## Motion

The probe is **fixed in the horizontal plane**. It does not extend, retract, or articulate.

- **Z-axis only**: the sole powered probe motion is a worm-gear vertical lift used to align tip height with the target.
- **Contact is made by driving the chassis** slowly toward the surface, not by moving the probe.
- **Manual XY adjustment** is done off-robot at mount time; there is no in-tour XY motion of the probe.

## Safety

- Low-speed calibration mode
- Front whisker safety bar
- Immediate stop on contact
- **Stop and report on any error** — no automatic retract, recovery, or repositioning. A human operator decides the next step.
- Software speed limiting
- Manual override mode

## Modularity

The probe module is detachable and self-describing. A module may contain: camera calibration, probe geometry, fixed mount offsets, module ID, hardware revision.
