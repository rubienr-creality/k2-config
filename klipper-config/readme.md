# Configs

This folder contains my current (modified) Klipper printer configs.

Fixes:

 1. renames fan and fan switch in "Fans & Outputs" UI
   - Chamber Fan -> Chamber Heater Fan
   - Power -> Chamber Heater Power
 2. tunes z-tilt
   - increases required accuracy
   - decreases required `horizontal_move_z` clearance
 3. decreaes 2nd x/y homing speed
 4. slightly faster meshing
   - increases x/y move speed
   - decreases z-clearance `horizontal_move_z`
 5. sets stepper timeout from `99999999s` to `10m`
