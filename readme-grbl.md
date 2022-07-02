# Notes GRBL G-Codes and modifications

## Modifications

### Commands

Since it is cumbersome to send non-printable characters, 
some command tokens were modified.

| Modified command token | Default command token | Command                      |
|:----------------------:|:---------------------:|:-----------------------------|
|           Q            |         0x18          | reset                        |
|           °            |         0x90          | feed override reset: 100%    |
|           :            |         0x91          | feed override increase: +10% |
|           ;            |         0x92          | feed override decrease: -10% |

- Supported tokens: A B C D E H O Q U V W ; : / ( ´ § ° µ €
- Unsupported tokens: , . _ { } \ < > | ) = [ ] ` ^ " % & * # @
- Used tokens: F G I J K L M N P R S T X Y Z $ 

### Spindle as servo PWM

The servo signal resolution is reduced to 15 steps which are about 66 spindle-steps.

**Examples:**

| CMD   | Servo Output             | Notes                                   |
|:------|:-------------------------|:----------------------------------------|
| S0    | signal disabled          | servo remains inactive at last position |
| S1    | minimum angular position | 1st servo-steps bin                     |
| S66   | minimum angular position | 1st servo-steps bin                     |
| S67   | 2nd angular step         | 2nd servo-steps bin                     |
| S133  | 2nd angular step         | 2nd servo-steps bin                     |
| S134  | 3rd angular step         | 3rd servo-steps bin                     |
| S934  | maximum angular position | last servo-steps bin                    |
| S1000 | maximum angular position | last servo-steps bin                    |
| S1001 | maximum angular position | last servo-steps bin                    |

## GRBL supported C-codes

| Code                                                                            | Description                                                                          | Modal group              |
|:--------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------|:-------------------------|
| [G10](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g10-l1)        | set tool table / set coordiante system                                               | AXIS_COMMAND_NON_MODAL   |
| [G28](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g28-g28.1)     | go to position                                                                       | AXIS_COMMAND_NON_MODAL   |
| [G28.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g28-g28.1)   | set position                                                                         | AXIS_COMMAND_NON_MODAL   |
| [G30](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g30-g30.1)     | go to position                                                                       | AXIS_COMMAND_NON_MODAL   |
| [G30.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g30-g30.1)   | set position                                                                         | AXIS_COMMAND_NON_MODAL   |
| [G92](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92)           | set coordinate system offset                                                         | AXIS_COMMAND_NON_MODAL   |
| [G92.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92.1-g92.2) | turn off offset                                                                      | AXIS_COMMAND_NON_MODAL   |
| [G4](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g4)             | dwell                                                                                | MODAL_GROUP_G0           |
| [G53](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g53)           | move in machine coordinates                                                          | MODAL_GROUP_G0           |
| [G0](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g0)             | rapid move                                                                           | AXIS_COMMAND_MOTION_MODE |
| [G1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g1)             | linear move                                                                          | AXIS_COMMAND_MOTION_MODE |
| [G2](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g2-g3)          | clockwise arc move                                                                   | AXIS_COMMAND_MOTION_MODE |
| [G3](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g2-g3)          | counterclockwise arc move                                                            | AXIS_COMMAND_MOTION_MODE |
| [G38](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)           | straight probe                                                                       | AXIS_COMMAND_MOTION_MODE |
| [G38.2](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | straight probe toward workpiece, stop on contact, signal error if failure            | AXIS_COMMAND_MOTION_MODE |
| [G38.3](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | straight probe toward workpiece, stop on contact                                     | AXIS_COMMAND_MOTION_MODE |
| [G38.4](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | straight probe away from workpiece, stop on loss of contact, signal error if failure | AXIS_COMMAND_MOTION_MODE |
| [G38.5](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | straight probe away from workpiece, stop on loss of contact                          | AXIS_COMMAND_MOTION_MODE |
| [G80](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g80)           | cancel canned cycle                                                                  | MODAL_GROUP_G1           |
| [G17](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | xy plane select (default)                                                            | MODAL_GROUP_G2           |
| [G18](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | zx plane select                                                                      | MODAL_GROUP_G2           |
| [G19](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | yz plane select                                                                      | MODAL_GROUP_G2           |
| [G90](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90-g91)       | G90 absolute distance mode                                                           | MODAL_GROUP_G3           |
| [G91](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90-g91)       | incremental distance mode                                                            | MODAL_GROUP_G3           |
| [G93](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g93-g94-g95)   | inverse time feed rate mode                                                          | MODAL_GROUP_G5           |
| [G94](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g93-g94-g95)   | units per minue feed rate mode                                                       | MODAL_GROUP_G5           |
| [G20](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g20-g21)       | unit: inch                                                                           | MODAL_GROUP_G6           |
| [G21](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g20-g21)       | unit: Millimeter                                                                     | MODAL_GROUP_G6           |
| [G40](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g40)           | turh cuttercompensation off                                                          | MODAL_GROUP_G7           |
| [G43](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g43)           | tool length offset                                                                   | MODAL_GROUP_G8           |
| [G49](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g49)           | cancel tool length compensation                                                      | MODAL_GROUP_G8           |
| [G54](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 1                                                           | MODAL_GROUP_G12          |
| [G55](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 2                                                           | MODAL_GROUP_G12          |
| [G56](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 3                                                           | MODAL_GROUP_G12          |
| [G57](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 4                                                           | MODAL_GROUP_G12          |
| [G58](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 5                                                           | MODAL_GROUP_G12          |
| [G59](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | select coordinate system 6                                                           | MODAL_GROUP_G12          |
| [G61](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g61)           | exact path mode                                                                      | MODAL_GROUP_G13          |

See also:

- LinuxCNC [G-Codes reference](https://www.linuxcnc.org/docs/html/gcode/g-code.html)
- GRBL [`uint8_t gc_execute_line(char *line)` implementation](https://github.com/photogrammetry-scanner/grbl/blob/photogrammetry/grbl/gcode.c#L66)

## GRBL supported M-codes

| Code                                                                      | Description                                                         | Modal group    |
|:--------------------------------------------------------------------------|:--------------------------------------------------------------------|:---------------|
| [M0](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m0-m1)    | pause program                                                       | MODAL_GROUP_M4 |
| [M1](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m0-m1)    | pause program                                                       | MODAL_GROUP_M4 |
| [M2](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m2-m30)   | end program                                                         | MODAL_GROUP_M4 |
| [M30](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m2-m30)  | change pallet shuttles and end program                              | MODAL_GROUP_M4 |
| [M3](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | start spindle clockwise                                             | MODAL_GROUP_M7 |
| [M4](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | start spindle counterclockwise                                      | MODAL_GROUP_M7 |
| [M5](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | stop spindle                                                        | MODAL_GROUP_M7 |
| [M7](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | mist coolant on (requires ENABLE_M7)                                | MODAL_GROUP_M8 |
| [M8](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | flood coolant on                                                    | MODAL_GROUP_M8 |
| [M9](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | both coolants off                                                   | MODAL_GROUP_M8 |
| M56                                                                       | parking control override (requires ENABLE_PARKING_OVERRIDE_CONTROL) | MODAL_GROUP_M9 |

See also:

- LinuxCNC [M-Codes reference](https://www.linuxcnc.org/docs/html/gcode/m-code.html)
- GRBL [`uint8_t gc_execute_line(char *line)` implementation](https://github.com/photogrammetry-scanner/grbl/blob/photogrammetry/grbl/gcode.c#L66)

## GRBL assignment letters

Examples: 
- set spindle 0 speed to 100: `S100 $0`
- move X to position 100: `X100`

| Letter | Description   | Notes                  |
|:-------|:--------------|:-----------------------|
| I      | X-Axis        | move X to new position |
| J      | Y-Axis        | move Y to new position |
| K      | Z-Axis        | move Z to new position |
| X      | X-Axis        | move X to new position |
| Y      | Y-Axis        | move Y to new position |
| Z      | Z-Axis        | move Z to new position |
| L      |               |                        |
| N      |               |                        |
| P      |               |                        |
| R      |               |                        |
| S      | spindle speed | set new speed          |
| T      | tool number   | set current tool       |
