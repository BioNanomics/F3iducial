# F3iducial Development Guidelines

F3iducial is a FIRST field calibration and verification platform — a mobile instrumentation robot, not a competition bot. All decisions should prioritize measurement accuracy, safety, and reproducibility.

---

## Safety First

Hardware can injure people and damage equipment.

- **Default to low speed**: All motion defaults must be slow until explicitly overridden by calibrated, validated state
- **Hardware interlocks before software**: Don't rely solely on software limits — design for hardware safety stops
- **Fail safe — stop and report**: On any fault, communication loss, or unexpected state, stop **all** motion immediately and report the reason. Do not attempt automatic recovery, repositioning, or retraction; wait for a human operator.
- **Probe is fixed**: The probe does not extend, retract, or articulate. Its only powered motion is **Z-axis lift** (worm-gear). Contact is made by driving the chassis, not by moving the probe horizontally. Never write logic that assumes the probe can be "pulled back" from a surface.
- **Test in simulation before hardware**: Validate logic in sim before deploying to physical hardware
- **Safety features are not optional**: Whisker bars, speed limiters, and emergency stops must not be bypassed

---

## Coordinate Systems and Units

Coordinate confusion is a measurement error.

- **Document the frame**: Every pose, offset, and vector must be labeled with its reference frame (field-relative, robot-relative, tag-relative, probe-relative)
- **Use SI units internally**: Meters and radians everywhere. Document explicit conversions at system boundaries (e.g., WPILib `Units.inchesToMeters()`)
- **Positive direction conventions**: Follow WPILib conventions — X forward, Y left, Z up, CCW positive rotation
- **Never mix frames silently**: If a function takes a field-relative pose, assert or document it; don't assume

---

## Code Quality

### DRY (Don't Repeat Yourself)
- Extract reusable geometry, calibration math, and sensor utilities into shared modules
- Field layout data belongs in one place — other code references it, never duplicates it

### KISS (Keep It Simple)
- Prefer simple, well-understood algorithms over clever ones for localization and geometry
- If a calculation is non-obvious, explain the geometric or physical reasoning briefly
- Small, focused functions — one responsibility per function

### Minimal Changes (PR Philosophy)
- **Smallest viable change**: Make the minimum change that fully solves the problem
- **No sweeping refactors** without explicit request
- **Isolated changes**: If a fix grows complex, extract it into a new module rather than touching many files

---

## Measurement and Calibration

Accuracy matters. This tool exists to find centimeter-level errors.

- **Track accuracy targets**: Target ±1–2 cm repeatability; document when a method's precision is worse
- **Validate measurements**: Cross-check tag-based localization against odometry; reject outliers with documented thresholds
- **Record provenance**: All measurements should carry timestamps, robot pose at time of measurement, and method used
- **Expected vs. actual**: Always compare against the canonical field layout model — deviations are the output, not errors

---

## Hardware/Software Interface

- **Abstract hardware early**: Define interfaces for drive, probe, and vision so logic can be tested in simulation
- **Self-describing modules**: Probe modules carry their own geometry config (camera calibration, arm offsets, module ID)
- **Never hardcode field dimensions**: Load from data files; support multiple field variants and years

---

## Vision and AprilTags

- **Report confidence**: Vision measurements must carry an ambiguity or confidence metric
- **Validate tag identity**: Confirm tag ID matches expected field position before using for localization
- **Multi-tag fusion**: When multiple tags are visible, fuse estimates rather than arbitrarily selecting one
- **Camera calibration is required**: No vision measurements without verified intrinsic calibration loaded

---

## Folder Philosophy

- **Clear purpose**: Every folder should have a main thing that anchors its contents
- **No junk drawers**: Don't leave loose files without context or explanation
- **Explain relationships**: If it's not obvious how files fit together, add a README
- **Immediate clarity**: Opening a folder should make its organizing principle clear at a glance

---

## Dead Code Management

- **Immediate removal**: Delete unused code when identified
- **Historical preservation**: Move significant removed code to `.attic/` with a comment explaining why
- **No accumulation**: Don't let dead code build up in active paths

---

## Documentation

- **Mermaid diagrams** over ASCII art for workflows, architecture, and data flows
- **Memorable node names** in diagrams (e.g., `PoseEstimator`, `ProbeArm`, `TagDatabase` not `A`, `B`, `C`)
- Use appropriate diagram types:
  - `graph LR` / `graph TB` for architecture
  - `sequenceDiagram` for sensor pipelines and communication
  - `flowchart TD` for calibration and inspection workflows
- Keep docs DRY — reference other docs rather than duplicating content

---

## GitHub Actions

- **Script-first**: Workflows call scripts that can run locally
- **Local parity**: CI and local dev run the same commands
- **Thin wrappers**: Complex logic belongs in scripts, not workflow YAML

---

## Code Quality Checklist

- [ ] **Safety**: Are all motion defaults safe? Do faults stop motion?
- [ ] **Frames**: Is every pose/vector labeled with its reference frame?
- [ ] **Units**: Are SI units used internally? Are conversions explicit?
- [ ] **DRY**: No duplicated geometry, calibration math, or field data?
- [ ] **KISS**: Simplest correct algorithm used?
- [ ] **Minimal**: Smallest viable change for this PR?
- [ ] **Dead Code**: Removed or archived?
- [ ] **Docs**: Non-obvious WHY explained? Diagrams updated if architecture changed?
- [ ] **Tests**: Simulation or unit tests cover the new logic?
