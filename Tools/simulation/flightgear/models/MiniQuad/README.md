MiniQuad FlightGear model placeholder
====================================

This folder is a template placeholder for a FlightGear multirotor aircraft
named `MiniQuad`. The `sitl_run.sh` script will copy the contents of this
folder into the `flightgear_bridge/models/` submodule before launching the
bridge if the folder is present.

To add a working FlightGear multirotor model, place the aircraft directory
structure here. A typical structure looks like:

MiniQuad/
  aircraft/
    MiniQuad/
      README
      MiniQuad.xml
      models/
        MiniQuad/  (3D model files, e.g., .ac/.obj/.vtl etc.)

Notes and tips:
- The internal FlightGear aircraft name (e.g., `MiniQuad`) must match the
  `FgModel` value in `Tools/simulation/flightgear/models/miniquad.json`.
- If you can produce a simple OBJ/AC3D model (a central body and four arms),
  FlightGear can use it; you'll need to author the aircraft XML (or use the
  model packaging conventions). See FlightGear docs for aircraft packaging.
- Alternatively, create an archive (zip) of the `MiniQuad` folder and set the
  `Url` field in `Tools/simulation/flightgear/models/miniquad.json` so
  `FG_run.py` can download and install it automatically.

If you want, I can scaffold a simple example `MiniQuad` aircraft XML that
references a placeholder model. However, creating actual 3D geometry is
best done in Blender or a modeler and exported to a format FlightGear
accepts.
