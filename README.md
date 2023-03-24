# # A basic line follower

This code for a line follower is an improvement on my [previous line follower code](https://github.com/eccentricOrange/line-follower-std).

Just like that project, this robot has the following features:
* It would use 3 infrared sensors to detect the line. Each sensor outputs a Boolean value.
* It would follow [differential drive](https://en.wikipedia.org/wiki/Differential_wheeled_robot) principles for movement.
* The expected control hardware is an Arduino-compatible microcontroller.
* The expected motor driver is a something like an L293D or L298N motor driver.

However, it also implements a closed-loop control system to actuate the motors, which reduces the jerkiness of the robot's movement.

___

Note that this project is written for [Platform IO](https://platformio.org/). If you just want the code, it's in [`src/main.cpp`](src/main.cpp).

## Key logic
The crucial logic in this program is very simple. It can be worked by using a model or drawing of the robot, and a line on a piece of paper.

The robot has 3 sensors, and 2 motors (or another even number). The sensors are placed on the left, middle, and right of the robot. The sensors are placed in a line, and the robot is placed on the line.

For every possible sensor configuration, the robot will move in a certain way, and if you can determine which movement will bring teh centre sensor to the line, you can determine the logic.

## Motor control logic
This robot uses my own [Differential Drive library, DDBot](https://github.com/eccentricOrange/DDBot). The `ForwardDDBot` class from this library is a Forward Biased Differential Drive robot. This means that the robot will always move forward, and the direction of the robot is controlled by the speed of the motors.


The main loop here is structured to use feedback control. In each pass of the loop:
1.  Values from the IR sensors are recorded and stored in a variable.
2.  The sensor data is used to determine the direction the robot should proceed in.
3.  The speed of each motor is adjusted to turn the robot closer to the desired direction.

Conventionally, the direction of a **differential wheeled robot** is set by turning motors on or off in a specific direction. In this setup, they are set to move forward perpetually, and direction control is achieved by varying the speed of each motor instead. This allows for smoother turns and will (hopefully) help the robot to stay on its line more reliably.