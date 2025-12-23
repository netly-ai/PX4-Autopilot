MiniQuad placeholder aircraft
=============================

This folder contains a minimal placeholder aircraft for FlightGear named
`MiniQuad`. It includes a very small OBJ mesh and a minimal `MiniQuad.xml`
descriptor. It is provided only for basic visualization and testing of the
PX4 FlightGear bridge; it does not include flight dynamics (JSBSim/YASim)
or realistic controls.

To use it:
1. Run the PX4 FlightGear launcher from the repo root (the script will
   copy this folder into the `flightgear_bridge/models/` directory):

   ```bash
   ./Tools/simulation/flightgear/sitl_run.sh \
     /home/trr/dev/PX4-Autopilot/build/px4_sitl_nolockstep/bin/px4 \
     miniquad \
     /home/trr/dev/PX4-Autopilot \
     /home/trr/dev/PX4-Autopilot/build/px4_sitl_nolockstep
   ```

2. Ensure `fgfs` (FlightGear) is installed and available on your PATH.
3. Expect a very simple blocky model as the aircraft visual.

If you want a higher-fidelity visual, replace `models/MiniQuad.obj` with a
model exported from Blender and augment `MiniQuad.xml` with proper
flight dynamics configuration.
