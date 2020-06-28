# Generate VEX v5 controller button assignments by parsing PROS source code
This python script parses a "constants.h" file with button assignments of the form

    const auto INTAKE_BUTTON  = okapi::ControllerDigital::R1;
to generate a markdown file of button assignments.

The following is an example of documentation generated directly from source code.

---

# Key bindings for SMC 2019-2020 24in robot
![Controller](https://raw.githubusercontent.com/smcrobotics/competition_bot_15_inch/master/docs/controller.png)

## Button mappings
### Bumpers (1-4), Joysticks (5-6), Arrows (7-10), Letters (11-14)
1. R1: intake button 
2. L1: outtake button
3. R2: move intake to open
4. L2: move intake to closed
5. Left Joystick: left side drive motors
6. Right Joystick: right side drive motors
7. down: tray pos down
8. left: raise tray
9. up: tray pos up
10. right: lower tray
11. Y: toggle tray
12. X: toggle intake
13. A: place stack
14. B: drive brake toggle

---

# Motivation

This script is useful particularly when any of the following are true:

- the drivers are not involved in programming the robot
- there are multiple drivers for each robot
- two robots have different button assignments, and it is desired to have a readily available reference for button bindings for each robot (i.e. for VEX U teams)

Note that the script assumes a tank drive configuration, with the left joystick controlling the left side motors and the right joystick controlling the right side motors (see lines 54 and 55 in `generic_parse_key_bindings.md`). This can be easily modified to suit your driving setup.

For example, if you use arcade with the left joystick controlling turning the robot left and right and the right joystick controlling forward and backward motion, change line 54 to

    output_file.write("5. Left Joystick: turns robot left and right (x axis only)\n")

and change line 55 to

    output_file.write("6. Right Joystick: move robot forward and backward (y axis only)\n")

Similarly if you have a holonomic drive setup with the left joystick controlling rotation and the right joystick controlling translation, change line 54 to

    output_file.write("5. Left Joystick: controls robot rotation

and change line 55 to

    output_file.write("6. Right Joystick: controls robot translation")

# Setting up your project to use the parse\_key\_bindings script
- Create a directory called `docs` in your top level directory, and put the file `controller.png` in this directory.
- Create a file called `constants.h` and assign your button mappings inside of it (see [`constants_example.h`](https://github.com/nashirj/create-vex-controller-documentation/blob/master/constants_example.h)).
- Add the lines

        builddocs: path/to/constants.h
            python generic_parse_key_bindings.py
to your `Makefile` which is in the top level directory of your project folder (see [`Makefile_example`](https://github.com/nashirj/create-vex-controller-documentation/blob/master/Makefile_example.h)).
- Update `parse_key_bindings.py` with the requested information

# Usage
To generate the documentation, type `prosv5 make builddocs` at the root directory of your project. If you want the script to be run everytime you compile the project, you can create aliases in your `.bashrc` (or equivalently `.zshrc`, etc) of the form

    alias pm='prosv5 make builddocs'
    alias pmu='prosv5 make builddocs && prosv5 upload'
    alias pmut='prosv5 make builddocs && prosv5 upload && prosv5 terminal'

# Example of project that uses this script
See [this github repository](https://github.com/smcrobotics/competition_bot_24_inch) for an example of all the above used in a PROS project.
