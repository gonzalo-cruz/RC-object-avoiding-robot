# Autonomous Obstacle-Avoiding Robot (STM32)

![C](https://img.shields.io/badge/Language-C-00599C)
![Platform](https://img.shields.io/badge/Platform-STM32%20Nucleo--L152RE-blue)
![IDE](https://img.shields.io/badge/IDE-STM32CubeIDE-lightgrey)
![Status](https://img.shields.io/badge/Status-Prototype%20Completed-success)

> **A dual-mode robotic vehicle (Autonomous & RC) featuring real-time obstacle avoidance and Bluetooth telemetry, developed in C using STM32 HAL.**

## Project Overview
This project implements a comprehensive embedded control system for a 2WD robotic chassis. The system operates in two distinct modes: **Manual Control** via Bluetooth (UART) and **Autonomous Navigation** using ultrasonic feedback.

The core challenge was to manage hardware constraints in real-time, utilizing hardware timers for PWM motor control and Input Capture for distance sensing without blocking the CPU, ensuring immediate reaction times (<10cm stop distance).

[**📄 View Full Technical Report (PDF)**](project_report.pdf)

## System Architecture

### Hardware Components
* **Microcontroller:** STM32 Nucleo-L152RE (ARM Cortex-M3)
* **Actuators:** 2x DC Motors driven by L298N H-Bridge.
* **Sensors:** HC-SR04 Ultrasonic Sensor (Echo/Trigger).
* **Comms:** HC-05 Bluetooth Module (UART).
* **Feedback:** Piezoelectric Buzzer (PWM audio feedback).

### Firmware Logic & Peripherals
The firmware is built on the **STM32 HAL API** utilizing an interrupt-driven architecture to handle asynchronous events:

* **TIM2 (Input Capture):** Measures the pulse width of the HC-SR04 Echo signal to calculate distance with microsecond precision.
* **TIM3 (PWM Output):** Generates Pulse Width Modulation signals to control motor speed (Duty Cycle) and buzzer frequency.
* **USART2 (Interrupt Mode):** Handles incoming Bluetooth commands asynchronously via `HAL_UART_RxCpltCallback` to switch modes (Auto/Manual) instantly.
* **GPIO:** Management of direction pins and status LEDs.

## Logic Flow & State Machine

The robot operates on a finite state machine loop inside `main.c`:

1.  **Sensing:** The Ultrasonic sensor triggers a pulse every 60ms.
2.  **Decision (Auto Mode):**
    * `Distance > 20cm`: Full Speed Forward.
    * `10cm < Distance < 20cm`: Decelerate (PWM reduction).
    * `Distance < 10cm`: **Hard Stop** & Audio Warning.
3.  **Command Handling:** Bluetooth interrupts update the state variable (Forward, Backward, Left, Right, Auto).

*(Place the Flowchart image from your PDF here, e.g., Figure 6)*

## 🔌 Pinout Configuration

| Component | Pin | Peripheral Function |
| :--- | :--- | :--- |
| **Left Motor** | PA6 | TIM3_CH1 (PWM) |
| **Right Motor** | PA7 | TIM3_CH2 (PWM) |
| **Ultrasonic Trigger** | PB9 | GPIO Output |
| **Ultrasonic Echo** | PA0 | TIM2_CH1 (Input Capture) |
| **Bluetooth TX/RX** | PA2 / PA3 | USART2 (Serial) |
| **Buzzer** | PA5 | GPIO / PWM |

## Build & Flash

1.  **Clone the repo:**
    ```bash
    git clone [https://github.com/gonzalo-cruz/autonomous-robot-stm32.git](https://github.com/gonzalo-cruz/autonomous-robot-stm32.git)
    ```
2.  **Import to IDE:**
    Open **STM32CubeIDE** and import the project folder.
3.  **Hardware Setup:**
    Connect the L298N driver and Sensors according to the Pinout table above.
4.  **Flash:**
    Connect the Nucleo board via USB ST-Link and click **Run/Debug**.

## Engineering Decisions

### Why Interrupts over Polling?
Initially, UART polling was considered, but it caused the robot to be unresponsive to "Stop" commands while it was calculating distance loops. I migrated to **UART Interrupts (`RXNE`)** to ensure that safety commands (STOP) are processed immediately, regardless of the main loop's state.

### Pulse Width Modulation (PWM) for Speed Control
Instead of simple On/Off logic, I implemented **PWM on TIM3**. This allowed for a "Soft Braking" feature—when an obstacle is detected between 10-20cm, the robot linearly decreases the Duty Cycle, preventing abrupt stops that could damage the chassis gearboxes.

## 👤 Author
* **Gonzalo Cruz Gómez**

* **Alonso Madroñal de Mesa**
---
*Developed for the Microprocessor-Based Digital Systems Laboratory.*
