# RC-object-avoiding-robot
Lab project for microprocessor-based digital systems, developed in a Nucleo-L152RE in C languaje with the STM32 IDE.
The Lab project consists in the development of a robot with an obstacle detection mechanism. The functionalities that are sought are the following:

  - Movement forward and backward in a straight line
  - Speed selection
  - Turn to both sides
  - Measurement of the distance to an obstacle when the robot is moving forward
  - Stop the movement of the robot when an obstacle is detected at less than 10 cm
  - Reduce the speed of the robot progressively when an obstacle is detected at less than 20 cm
  - Generate acoustic signals depending on the proximity of the obstacle
  - Communication with a device (PC or mobile phone), which includes receiving commands
  - Bluetooth communication with a mobile phone that controls the robot movement
  
All goals for the project were met and the robot is capable of moving both in RC mode controlled by the phone and in self driving mode. 

As for the components used: 
  - Development board NUCLEO-L152RE 
  - AptoFun 2WD Motor Smart Car Chassis for Arduino- with 2 Gear Motor and Battery Box
  - ARCELI 5 PCS L298N  motor driver
  - ELEGOO HC-SR04 ultrasonic sensor
  - Passive Buzzer
  - AZDelivery HC-05 HC-06 Bluetooth Wireless, RF Transceptor Module, RS232 Serial TTL Bluetooth
  - 10k potentiometer
  - For the bluetooth control of the robot I used the 'Serial Bluetooth Terminal' app only available on Android

The pins used for each peripheral were the following: 

![image](https://user-images.githubusercontent.com/75577062/235641612-61ee64b7-379e-4464-baad-91b4353d0e8d.png)

All the rest of the documentation is in the Report document, where a block diagram, flowcharts and description of the peripherals can be found
