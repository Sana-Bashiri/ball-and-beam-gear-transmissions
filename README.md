# Gear Transmission Design for a Ball-and-Beam System

A MATLAB/Simulink project exploring spur and worm transmissions for beam actuation in a ball-and-beam mechanism. The repository contains multibody models, CAD geometry, initialization code, recorded gear animations, and an academic English report.

The study examines transmission ratios, mechanical geometry, actuation methods, and feedback implementation. The available results comprise analytical calculations and recorded geometric demonstrations; quantitative closed-loop performance has not yet been established from exported simulation data.

## Project contents

```text
code/
├── models/
│   ├── spur_beam_baseline.slx
│   ├── spur_ball_beam_pid.slx
│   └── worm_ball_beam_pid.slx
├── scripts/
│   └── initialize_spur_parameters.m
├── geometry/
│   ├── spur_20_80/
│   ├── spur_10_50/
│   └── worm/
└── libraries/
    └── Multibody_Parts/
        ├── Multibody_Parts_Lib.slx
        ├── slblocks.m
        ├── Extrusions/
        └── Images/
report/
├── README.md
└── gear_transmission_ball_beam_report.pdf
results/
├── spur_gear_animation.mp4
└── compound_gear_animation.mp4
```

The source models, MATLAB script, geometry assets, supporting library, and videos were copied without changing their contents. Publication filenames and folder locations were reorganized. Internal library filenames were retained to preserve existing links and mask dependencies.

## Model configurations

| Model | Description | Main characteristics |
|---|---|---|
| [Spur beam baseline](../code/models/spur_beam_baseline.slx) | Spur-driven beam and linkage mechanism | 20/80-tooth geometry, 4:1 reduction, prescribed joint motion, 20 s stop time |
| [Spur ball-and-beam model](../code/models/spur_ball_beam_pid.slx) | Ball, beam, contact, and two PID blocks | 10/50-tooth geometry, geometric reduction of 5:1, motion-actuated input joint, 10 s stop time |
| [Worm ball-and-beam model](../code/models/worm_ball_beam_pid.slx) | Worm-driven mechanism with ball contact and two PID blocks | Right-hand worm constraint, external torque actuation, 30 s stop time |

The spur constraint blocks use pitch-circle radii as their active specification. Their workspace variables must therefore match the selected CAD geometry.

## Analytical results

The following values are ideal predictions at an illustrative input speed of **30 rpm**:

| Transmission | Reduction ratio | Calculated output-speed magnitude |
|---|---:|---:|
| Spur pair, 20/80 teeth | 4.00:1 | 7.50 rpm |
| Spur pair, 10/50 teeth | 5.00:1 | 6.00 rpm |
| Saved worm constraint | Approximately 25.63:1 | Approximately 1.17 rpm |

For a spur pair, the ideal reduction is `i = z_output / z_input`, and the output-speed magnitude is `n_output = n_input / i`.

The worm value is calculated from the saved pitch radii and lead angle:

```text
worm pitch radius = 8.5 mm
gear pitch radius = 16 mm
worm lead angle   = 4.2 degrees

i = gear_radius / (worm_radius × tan(lead_angle))
  ≈ 25.63
```

These calculations do not establish measured efficiency, practical torque capacity, positioning accuracy, or dynamic control performance.

## Recorded results

- [Spur gear animation](../results/spur_gear_animation.mp4): approximately 13.33 s, showing an externally meshing 20/80-tooth pair.
- [Compound gear animation](../results/compound_gear_animation.mp4): approximately 14.87 s, showing a compound arrangement with an internal ring and smaller gears.

The videos document geometric motion. They do not provide calibrated torque measurements or ball-position response histories. Their labeled geometry should be evaluated separately from the CAD assets used by the multibody models.

## Software requirements

- MATLAB and Simulink
- Simscape
- Simscape Multibody
- The supplied `Multibody_Parts` supporting library

The supplied models include R2024a/R2024b save formats. Compatibility and execution of the reorganized package have not been validated across MATLAB releases.

## Opening the baseline model

Open MATLAB in the repository root, where `code`, `report`, and `results` are located, and run:

```matlab
projectRoot = pwd;

addpath(fullfile(projectRoot, 'code', 'scripts'));
addpath(genpath(fullfile(projectRoot, 'code', 'libraries', ...
    'Multibody_Parts')));

run(fullfile(projectRoot, 'code', 'scripts', ...
    'initialize_spur_parameters.m'));

open_system(fullfile(projectRoot, 'code', 'models', ...
    'spur_beam_baseline.slx'));
```

Before simulation, update the File Solid blocks to use the geometry files in this repository. The saved models retain absolute CAD paths from their original development locations; renaming the files does not update those settings.

The baseline initialization defines pitch radii of **2 cm** and **8 cm** for the 20/80-tooth pair. It is not a verified initializer for the controlled 10/50-tooth model. That model requires pitch radii of **1.5 cm** and **7.5 cm**, together with consistent linkage variables and frame placement.

## Current validation status

- The controlled variants route a ball translational sensing signal through a radians-to-degrees conversion in their secondary feedback paths. The physical quantity, units, and intended loop structure require review.
- Beam dimensions, densities, and actuation methods differ between models. A performance comparison requires consistent load and actuator definitions.
- Prescribed joint motion does not reproduce a servo torque limit or torque-speed characteristic.
- A worm kinematic constraint and viscous terms alone do not establish self-locking or transmission efficiency.
- Exported histories of ball position, beam angle, torque, and tracking error are not included. Settling time, overshoot, steady-state error, and stability margins are therefore not presented as verified outcomes.

## Technical report

Read the [English technical report](gear_transmission_ball_beam_report.pdf) for the mechanical equations, model parameters, PID coefficients, analytical results, figures, and verification findings.

The report uses a black-and-white academic layout, with color confined to figures.

## Supporting library

The supplied library contains MathWorks copyright notices. Those notices are preserved in the packaged files. The library model, extrusion functions, and image filenames should remain unchanged unless their dependent model links and mask code are updated accordingly.
