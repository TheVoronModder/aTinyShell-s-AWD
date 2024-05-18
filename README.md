# aTinyShell-s-AWD
My journey with aTinyShellScript's AWD kit

## This guide covers the use of an Octopus v1.1 Control Board (MCU) and help you with Sensorless Homing.
-------------------------------

I am assuming you are running a tool head with CanBus or USB capability, this is crucial because we do not want to use the bridged driver for 2 motors.

---------------------
### Getting Started.

Let's start with wiring things up....

You can do this however you choose to do it, I ran my wires on the outside of the extrusions with a printed cover.

--------------------------------
Octopus Wiring....

I have an LDO 2.4 300 build. I used the LDO wiring guide hence my pinout and motor location of the octopus control board:

![image](https://github.com/TheVoronModder/aTinyShell-s-AWD/assets/142328467/c5440bc7-c6ff-4fba-a3ae-9f4b450e9edb)

You need to swap the GREEN and BLACK wires on your motors (specific to LDO brand motors)

If you do not have LDO motors and or wiring just swap 2 (outside) cables and your good to go.


![swap cable](https://github.com/TheVoronModder/aTinyShell-s-AWD/assets/142328467/56ae12f9-3976-4231-b588-4029ec1663a7)

---------------------------
## Printer.cfg configuration:

```bash
####################################################
#              Core X Stepper Settings               #
####################################################
[stepper_x]
step_pin: PF13
dir_pin: PF12
enable_pin: !PF14
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:400
endstop_pin: tmc2209_stepper_x:virtual_endstop #X_STOP
position_min: 0
position_endstop: 300
position_max: 300
homing_speed: 100 #50
homing_retract_dist: 0
homing_positive_dir: True

[tmc2209 stepper_x]
uart_pin: PC4
interpolate: False
run_current: 1.0
sense_resistor: 0.110
#stealthchop_threshold: 0 #always on, 0 is off
diag_pin: PG6 # use the same pin that was previously the endstop_pin!
driver_SGTHRS: 160

[stepper_x1]
step_pin: PE2
dir_pin: !PE3
enable_pin: !PD4
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:400

[tmc2209 stepper_x1]
uart_pin: PE1
interpolate: False
run_current: 1.0
sense_resistor: 0.110
#stealthchop_threshold: 0 #always on, 0 is off

####################################################
#              Core Y Stepper Settings               #
####################################################
[stepper_y]
step_pin: PG0
dir_pin: PG1
enable_pin: !PF15
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:400
endstop_pin: tmc2209_stepper_y:virtual_endstop #Y_STOP
position_min: 0
position_endstop: 300
position_max: 300
homing_speed: 100 #50
homing_retract_dist: 0
homing_positive_dir: true

[tmc2209 stepper_y]
uart_pin: PD11
interpolate: False
run_current: 1.0
sense_resistor: 0.110
#stealthchop_threshold: 0
diag_pin: PG9     # use the same pin that was previously the endstop_pin!
driver_SGTHRS: 195 # 255 is most sensitive value, 0 is least sensitive

[stepper_y1]
step_pin: PE6
dir_pin: !PA14
enable_pin: !PE0
rotation_distance: 40
microsteps: 32
full_steps_per_rotation:400

[tmc2209 stepper_y1]
uart_pin: PD3
interpolate: False
run_current: 1.0
sense_resistor: 0.110
```

## Sensorless Homing Setup

Now that you have your motors wired and printer.cfg configured lets see if you can home x

Just realize that your toolhead might CRASH or not make it. - Thats OKAY! were going to fine-tune that sensorless homing. 

In the Klipper Console (Mainsail or Fluidd)

put this command in the terminal and hit enter

```bash
SET_TMC_FIELD FIELD=SGTHRS STEPPER=stepper_x VALUE=255
```

You will need to tell your printer to home x each time you issue the below commands..

Subtract 50 until the toolhead moves all the way to the right...

```bash
SET_TMC_FILED FIELD=SGTHRS STEPPER=stepper_x VALUE=205
```

Keep adjusting the value until your toolhead makes it all the way RIGHT.

Then we need to fine-tune with by going + or - by 5 until things go all the way but do not CRASH.

Make sure you update your printer.cfg 

This is the line we want to mod: **driver_SGTHRS: 160**

save config. 

Do the same process above for Y










