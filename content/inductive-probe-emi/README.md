# Inductive probe EMI

I have two printers: one generates a relatively normal bed map, while the other doesn't. The inductive sensor sometimes triggers almost "randomly." The bed map is significantly distorted, and it can be measured at the cost of very large point repetitions.

I've noticed that the situation changes significantly during head maintenance, and that the problem depends on the location of the sensor cable.

The original cable is unshielded and without twisted pair. However, the extruder heater is always running, holding the PWM at 140 degrees (to stabilize for piezoelectric sensor measurements).

Disabling the heater during mesh construction stopped the random sensor activations and reduced its reading repeatability to 0.05 mm. Apparently, the inductive sensor cable is picking up interference from the heater's PWM controller.

This problem was on the original unit and on the Phaetus Conch.

## Possible solutions

1. Route the sensor cable away from the heater cables. It should also be secured to prevent vibration during printing.
2. Use a different type of sensor.
3. Turn off the extruder heater while the sensor is operating.

### Disable heater while meshing

1. Backup `gcode_macro.cfg`
2. Edit `gcode_macro.cfg`
3. Find `[gcode_macro G29]`
4. Insert new lines before (disable heater) and after (restore temp) calling meshing:
```
        M104 S0 # new line added - disable extruder heater
        BED_MESH_CALIBRATE PROFILE=kamp
        M109 S104 # new line added - restore heater to 140c
```
```
            M104 S0 # new line added - disable extruder heater
            _BED_MESH_CALIBRATE PROFILE=default
            M109 S104 # new line added - restore heater to 140c
```
