## Autonomous Mobile Robot & Active Fire Suppression System

**An Arduino-based robotics system engineered for autonomous navigation, dynamic obstacle avoidance, and real-time fire localization and suppression.**

## Project Overview

This project focuses on the development of an Autonomous Mobile Robot (AMR) capable of operating without human intervention. Utilizing an Arduino Uno as the core processing unit, the system integrates spatial perception and environmental hazard detection to dynamically navigate complex spaces while actively mitigating fire hazards.

## The Real-World Problem

Autonomous robotic navigation is critical in hazardous, fire-prone environments where human deployment carries severe risks. Traditional obstacle-avoidance mechanisms often fail to adapt to dynamic environments and lack the concurrent processing required to respond to sudden, unpredictable anomalies like spontaneous fire spots.

## Hardware Architecture

The system architecture was designed with a strict decoupling of logical control and mechanical execution to ensure system stability:

  * **Drive System:** 4-wheel drive chassis powered by DC gear motors and managed via a dedicated motor driver.
  * **Power Management:** To prevent power starvation during high-torque movements, the power supply was split: a 5.1V (10,000 mAh) power bank strictly powers the Arduino logic, while dual lithium batteries provide isolated power to the mechanical drive system.
  * **Spatial Perception:** An ultrasonic sensor mounted on a servo motor enables a 180° continuous spatial sweep for dynamic pathfinding.
  * **Suppression Actuator:** A high-speed propeller system that activates specifically to suppress detected flames.

## Software & Control Logic

The system processes continuous data streams via a concurrent execution loop in Embedded C, monitoring for both navigational hazards and fire anomalies simultaneously.

  * **Dynamic Pathfinding:** Upon detecting an obstacle within the threshold, the robot executes a safe-motion sequence: it reverses, scans the left and right vectors, evaluates the deepest clearance, and reroutes.
  * **Hazard Override:** The flame sensor runs on a continuous polling mechanism. If an infrared (IR) fire anomaly is detected, it triggers a system override—instantly halting the drive motors and activating the suppression propeller.

## Engineering Insights & Troubleshooting

Deploying hardware in the real world introduced constraints that required iterative engineering solutions:

  * **Sensor Latency Mitigation:** The initial obstacle threshold of 15 cm resulted in physical collisions due to the ultrasonic sensor's computation delay. I mitigated this by increasing the detection threshold to 25 cm, effectively absorbing the latency and eliminating collisions.
  * **Dead-End Logic Refactoring:** Early testing showed the robot forcing itself into narrow corners. The pathfinding logic was refactored to prioritize a "reverse-before-turn" algorithm, ensuring the robot creates physical clearance before attempting a new trajectory.
  * **IR Sensor Vulnerabilities:** Real-world testing exposed the IR flame sensor's susceptibility to false positives, specifically from direct sunlight and air conditioning remotes. While the logic executed perfectly (halting the motors upon signal reception), this highlighted a critical need for future iterations to implement multi-sensor validation (e.g., combining IR with thermal or vision sensors) and digital signal filtering.

## Tech Stack

  * **Microcontroller:** Arduino Uno
  * **Language:** Embedded C (Arduino IDE)
  * **Sensors & Actuators:** Ultrasonic Sensor, Flame/IR Sensor, Servo Motor, DC Motors, L298N Motor Driver, Propeller
