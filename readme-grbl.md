# Notes to slightly modified GRBL firmware

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

Supported tokens: A B C D E H O Q U V W ; : / ( ´ § ° µ €
Unsupported tokens: , . _ { } \ < > | ) = [ ] ` ^ " % & * # @
Used tokens: F G I J K L M N P R S T X Y Z $ 

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

| Code                                                                            | Modal group              | Description                                                                          |
|:--------------------------------------------------------------------------------|:-------------------------|:-------------------------------------------------------------------------------------|
| [G10](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g10-l1)        | AXIS_COMMAND_NON_MODAL   | set tool table / set coordiante system                                               |
| [G28](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g28-g28.1)     | AXIS_COMMAND_NON_MODAL   | go to position                                                                       |
| [G28.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g28-g28.1)   | AXIS_COMMAND_NON_MODAL   | set position                                                                         |
| [G30](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g30-g30.1)     | AXIS_COMMAND_NON_MODAL   | go to position                                                                       |
| [G30.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g30-g30.1)   | AXIS_COMMAND_NON_MODAL   | set position                                                                         |
| [G92](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92)           | AXIS_COMMAND_NON_MODAL   | set coordinate system offset                                                         |
| [G92.1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92.1-g92.2) | AXIS_COMMAND_NON_MODAL   | turn off offset                                                                      |
| [G4](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g4)             | MODAL_GROUP_G0           |                                                                                      |
| [G53](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g53)           | MODAL_GROUP_G0           |                                                                                      |
| [G0](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g0)             | AXIS_COMMAND_MOTION_MODE | rapid move                                                                           |
| [G1](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g1)             | AXIS_COMMAND_MOTION_MODE | linear move                                                                          |
| [G2](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g2-g3)          | AXIS_COMMAND_MOTION_MODE | clockwise arc move                                                                   |
| [G3](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g2-g3)          | AXIS_COMMAND_MOTION_MODE | counterclockwise arc move                                                            |
| [G38](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)           | AXIS_COMMAND_MOTION_MODE | straight probe                                                                       |
| [G38.2](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | AXIS_COMMAND_MOTION_MODE | straight probe toward workpiece, stop on contact, signal error if failure            |
| [G38.3](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | AXIS_COMMAND_MOTION_MODE | straight probe toward workpiece, stop on contact                                     |
| [G38.4](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | AXIS_COMMAND_MOTION_MODE | straight probe away from workpiece, stop on loss of contact, signal error if failure |
| [G38.5](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38)         | AXIS_COMMAND_MOTION_MODE | straight probe away from workpiece, stop on loss of contact                          |
| [G80](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g80)           | MODAL_GROUP_G1           | cancel canned cycle                                                                  |
| [G17](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | MODAL_GROUP_G2           | xy plane select (default)                                                            |
| [G18](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | MODAL_GROUP_G2           | zx plane select                                                                      |
| [G19](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1)     | MODAL_GROUP_G2           | yz plane select                                                                      |
| [G90](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90-g91)       | MODAL_GROUP_G3           | G90 absolute distance mode                                                           |
| [G91](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90-g91)       | MODAL_GROUP_G3           | incremental distance mode                                                            |
| [G93](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g93-g94-g95)   | MODAL_GROUP_G5           | inverse time feed rate mode                                                          |
| [G94](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g93-g94-g95)   | MODAL_GROUP_G5           | units per minue feed rate mode                                                       |
| [G20](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g20-g21)       | MODAL_GROUP_G6           | unit: inch                                                                           |
| [G21](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g20-g21)       | MODAL_GROUP_G6           | unit: Millimeter                                                                     |
| [G40](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g40)           | MODAL_GROUP_G7           | turh cuttercompensation off                                                          |
| [G43](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g43)           | MODAL_GROUP_G8           | tool length offset                                                                   |
| [G49](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g49)           | MODAL_GROUP_G8           | cancel tool length compensation                                                      |
| [G54](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 1                                                           |
| [G55](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 2                                                           |
| [G56](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 3                                                           |
| [G57](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 4                                                           |
| [G58](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 5                                                           |
| [G59](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3)     | MODAL_GROUP_G12          | select coordinate system 6                                                           |
| [G61](https://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g61)           | MODAL_GROUP_G13          | exact path mode                                                                      |

See also:

- LinuxCNC [G-Codes reference](https://www.linuxcnc.org/docs/html/gcode/g-code.html)
- GRBL [`uint8_t gc_execute_line(char *line)` implementation](https://github.com/photogrammetry-scanner/grbl/blob/photogrammetry/grbl/gcode.c#L66)

## GRBL supported M-codes

| Code                                                                      | Modal group    | Description                                                         |
|:--------------------------------------------------------------------------|:---------------|:--------------------------------------------------------------------|
| [M0](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m0-m1)    | MODAL_GROUP_M4 | pause program                                                       |
| [M1](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m0-m1)    | MODAL_GROUP_M4 | pause program                                                       |
| [M2](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m2-m30)   | MODAL_GROUP_M4 | end program                                                         |
| [M30](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m2-m30)  | MODAL_GROUP_M4 | change pallet shuttles and end program                              |
| [M3](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | MODAL_GROUP_M7 | start spindle clockwise                                             |
| [M4](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | MODAL_GROUP_M7 | start spindle counterclockwise                                      |
| [M5](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5) | MODAL_GROUP_M7 | stop spindle                                                        |
| [M7](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | MODAL_GROUP_M8 | mist coolant on (requires ENABLE_M7)                                |
| [M8](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | MODAL_GROUP_M8 | flood coolant on                                                    |
| [M9](https://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9) | MODAL_GROUP_M8 | both coolants off                                                   |
| M56                                                                       | MODAL_GROUP_M9 | parking control override (requires ENABLE_PARKING_OVERRIDE_CONTROL) |

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
