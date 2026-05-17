# Field Configuration

Vendored, read-only copies of the field metadata F3iducial uses to know **where every AprilTag is supposed to be** and **how big the field is**. These files are the canonical "expected" geometry; the robot's inspection tour compares its measurements against them.

## Layout

```
fields/
├── refinery-forcefield/   # Field-image calibration (corners, size) — for visualization
│   ├── manifest.json
│   ├── 2023-chargedup.json
│   ├── 2024-crescendo.json
│   ├── 2025-reefscape.json
│   └── 2026-rebuilt.json
└── apriltag/              # WPILib AprilTag layouts (per-tag pose, ID, size)
    ├── 2023-chargedup.json
    ├── 2024-crescendo.json
    ├── 2025-reefscape-welded.json
    ├── 2025-reefscape-andymark.json
    ├── 2026-rebuilt-welded.json
    └── 2026-rebuilt-andymark.json
```

## Provenance

| Subdirectory | Upstream source | What it provides |
|---|---|---|
| `refinery-forcefield/` | [BioNanomics/refinery-forcefield](https://github.com/BioNanomics/refinery-forcefield) — `editor/fields/` | Field overall dimensions, image-to-field calibration corners. Sourced upstream from [WPILib field images](https://github.com/wpilibsuite/allwpilib/tree/main/fieldImages). |
| `apriltag/` | [WPILib `allwpilib`](https://github.com/wpilibsuite/allwpilib/tree/main/apriltag/src/main/native/resources/org/wpilib/vision/apriltag) | Per-tag 3D pose (translation in meters, rotation as quaternion) in the WPILib blue-origin coordinate system. |

Files are vendored as-is. **Do not hand-edit.** When a new season's data is published, re-pull from upstream rather than patching.

## 2025 / 2026 welded vs. andymark

Starting in 2025, FIRST publishes two physical tag-placement variants per season:

- **`-welded.json`** — official welded field, slightly different tag positions than the AndyMark field.
- **`-andymark.json`** — AndyMark-built field used at most events.

F3iducial selects the correct variant at tour time based on the event configuration. Both are vendored so the same robot can validate either physical field.

## Coordinate System

All AprilTag poses use the **WPILib blue-origin** convention (same as PathPlanner and Choreo):

| Axis | Direction |
|---|---|
| Origin | Blue alliance corner (bottom-left viewed from above) |
| +X | Toward red alliance wall (down-field) |
| +Y | Toward far side wall (cross-field) |
| +Z | Up |
| Rotation | Counter-clockwise positive, 0° = toward +X |
| Units | Meters, radians |

The robot must never silently mix this frame with a robot- or tag-relative frame. See [.github/copilot-instructions.md](../.github/copilot-instructions.md) → *Coordinate Systems and Units*.

## Runtime Dependency

At build time, the robot code consumes **[refinery-forcefield](https://github.com/BioNanomics/refinery-forcefield)** as a library — it owns field-image calibration and the editor for placing potential-field charges. F3iducial reuses the same field metadata so visualizations, force-fields, and tag layouts all agree.

### Gradle (once robot code lands)

```gradle
// settings.gradle
dependencyResolutionManagement {
    repositories {
        maven { url 'https://jitpack.io' }
    }
}

// build.gradle
dependencies {
    implementation 'com.github.BioNanomics:refinery-forcefield:v0.1.0'
}
```

For local development against an in-tree checkout:

```gradle
// settings.gradle
includeBuild '../refinery-forcefield'

// build.gradle
dependencies {
    implementation 'com.bionanomics.refinery:refinery-forcefield'
}
```

The vendored copies in this directory exist so the F3iducial repo is **self-contained for inspection/reporting** even when the robot Gradle build isn't available (e.g., generating field reports from a laptop, running unit tests in CI, or scripting from Python).

## Refresh

To pull the latest upstream copies:

```sh
# Field-image calibration (from refinery-forcefield)
BASE="https://raw.githubusercontent.com/BioNanomics/refinery-forcefield/main/editor/fields"
for f in manifest.json 2023-chargedup.json 2024-crescendo.json 2025-reefscape.json 2026-rebuilt.json; do
  curl -sfSL -o "fields/refinery-forcefield/$f" "$BASE/$f"
done

# AprilTag layouts (from WPILib)
BASE="https://raw.githubusercontent.com/wpilibsuite/allwpilib/main/apriltag/src/main/native/resources/org/wpilib/vision/apriltag"
for f in 2023-chargedup.json 2024-crescendo.json \
         2025-reefscape-welded.json 2025-reefscape-andymark.json \
         2026-rebuilt-welded.json 2026-rebuilt-andymark.json; do
  curl -sfSL -o "fields/apriltag/$f" "$BASE/$f"
done
```

## Related Docs

- Camera/probe geometry and tag-height envelope: [../README.md](../README.md#camera--probe-geometry)
- Architecture and self-describing probe modules: [../docs/architecture.md](../docs/architecture.md)
