# PX4 SITL Mini Quadrotor (DJI Mini 4 Pro Sized) - Summary of Changes

**Date:** December 23, 2025
**Branch:** dev-mini-4-pro-sim
**Objective:** Add compact multirotor airframe definitions for PX4 SITL simulation matching DJI Mini 4 Pro specifications

## Overview

This changeset adds two new airframe configurations to PX4-Autopilot for SITL (Software-In-The-Loop) simulation of compact multirotors with specifications matching the DJI Mini 4 Pro (~245mm wheelbase).

## Changes Made

### 1. New Airframe Files Created

#### A. `ROMFS/px4fmu_common/init.d-posix/airframes/10020_gazebo-classic_mini_quadrotor`
- **Autostart ID:** 10020
- **Simulator:** Gazebo-Classic
- **Type:** Quadrotor X configuration
- **Wheelbase:** ~245mm (DJI Mini 4 Pro spec)
- **Rotor Arm Length:** 0.0865m (122.5mm, half of wheelbase)
- **Configuration:**
  - 4 rotors in X-quad layout
  - Rotor positions:
    - Rotor 0 (FR): [0.0865, 0.0865] with Km=0.05
    - Rotor 1 (RL): [-0.0865, -0.0865] with Km=0.05
    - Rotor 2 (FL): [0.0865, -0.0865] with Km=-0.05
    - Rotor 3 (RR): [-0.0865, 0.0865] with Km=-0.05
  - PWM outputs mapped to motors 1-4
  - Uses default multicopter control defaults (rc.mc_defaults)

#### B. `ROMFS/px4fmu_common/init.d-posix/airframes/10021_jmavsim_mini_quadrotor`
- **Autostart ID:** 10021
- **Simulator:** jMAVSim
- **Type:** Quadrotor X configuration
- **Specifications:** Identical to Gazebo-Classic variant
- **Purpose:** Allows testing with jMAVSim simulator instead of Gazebo-Classic

### 2. Modified Files

#### `ROMFS/px4fmu_common/init.d-posix/airframes/CMakeLists.txt`
**Changes:**
- Added line 102: `10020_gazebo-classic_mini_quadrotor`
- Added line 103: `10021_jmavsim_mini_quadrotor`
- These entries register the new airframe files in the PX4 ROMFS build system

**Impact:** Both new airframes are now included in the px4_sitl_nolockstep build target

## Usage

### Starting PX4 SITL with Mini Quadrotor

#### Gazebo-Classic variant:
```bash
cd /home/trr/dev/PX4-Autopilot/build/px4_sitl_nolockstep
PX4_SIM_MODEL=gazebo-classic_mini_quadrotor ./bin/px4
```

#### jMAVSim variant:
```bash
cd /home/trr/dev/PX4-Autopilot/build/px4_sitl_nolockstep
PX4_SIM_MODEL=jmavsim_mini_quadrotor ./bin/px4
```

### Verification

When PX4 SITL starts with the new airframes, you should see:
```
INFO  [init] found model autostart file as SYS_AUTOSTART=10020
SYS_AUTOSTART: curr: 1034 -> new: 10020
```
(or `10021` for jMAVSim variant)

This confirms the mini quadrotor airframe has been successfully loaded.

## Technical Specifications

| Parameter | Value | Notes |
|-----------|-------|-------|
| **Wheelbase** | 245mm | DJI Mini 4 Pro spec |
| **Rotor Arm Length** | 86.5mm | Calculated from wheelbase |
| **Configuration** | X-Quad | 4 rotors, X arrangement |
| **Control Allocator** | Type 0 | Standard multirotor mixer |
| **Rotor Moment Coeff** | ±0.05 | Default values |

## Design Rationale

The mini quadrotor airframe definition was created to:
1. **Enable SITL testing** of algorithms suitable for small commercial drones
2. **Match real-world specs** using DJI Mini 4 Pro as reference (245mm wheelbase)
3. **Provide simulator options** with both Gazebo-Classic and jMAVSim support
4. **Maintain consistency** with PX4's airframe naming conventions (ID_simulator_model)

## Files Changed Summary

```
Modified:   ROMFS/px4fmu_common/init.d-posix/airframes/CMakeLists.txt (+2 lines)
Created:    ROMFS/px4fmu_common/init.d-posix/airframes/10020_gazebo-classic_mini_quadrotor
Created:    ROMFS/px4fmu_common/init.d-posix/airframes/10021_jmavsim_mini_quadrotor
```

**Total Impact:** 3 files modified/created, +45 lines added

## Testing

Both airframes have been verified to:
- ✓ Build successfully with `make px4_sitl_nolockstep`
- ✓ Load correctly when invoked via `PX4_SIM_MODEL` environment variable
- ✓ Generate correct autostart IDs (10020 and 10021)
- ✓ Initialize all PX4 subsystems (logger, MAVLink, control allocator)
- ✓ Properly configure 4-rotor motor outputs

## Future Work

- Integration with Gazebo-Classic 3D models for visual simulation
- Integration with jMAVSim 3D models for visual simulation
- Parameter tuning for flight controller gains specific to mini quadrotor
- Additional variants for other compact multirotor sizes/configurations

## Related Documentation

- PX4 Airframe Configuration: https://docs.px4.io/main/en/config/airframe.html
- PX4 Control Allocator: https://docs.px4.io/main/en/config/control_allocator.html
- SITL Simulation: https://docs.px4.io/main/en/simulation/
