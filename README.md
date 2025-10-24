Advanced PID Line Follower Robot (LFR)

🎯 Overview

This repository contains the complete code for a competition-grade Line Follower Robot (LFR) built for the EEE Day 2025 competition at RUET. The robot is designed for high speed and stability, capable of navigating complex tracks with S-curves, sharp turns, and potential line breaks.

🏆 Our team, [Your Team Name], secured 9th place out of [Total #] teams in the competition.

✨ Key Features

8-Sensor Digital Array: Uses an 8-sensor digital IR array for high-precision line position tracking.

Advanced PID Control: Implements a high-frequency PID control loop (500Hz) for highly responsive adjustments.

Dynamic PID Gains: The PID (Kp, Kd) gains are dynamically adjusted based on the detected path type, increasing stability on straights and agility in curves.

Path State Machine: A sophisticated state machine analyzes path history to classify the robot's current state (e.g., PATH_STRAIGHT, PATH_SHARP_CURVE, PATH_S_CURVE, PATH_LOST).

Smart Line Recovery: If the line is lost, the robot uses its error history to predict the most likely direction to find the line and executes a multi-stage recovery maneuver.

Real-time Debug Display: A 16x2 I2C LCD provides real-time feedback on the current path state, sensor values, and PID gains.

🛠️ Tech Stack & Components

Arduino Uno

L293D Motor Driver IC (the 16-pin chip)

8-Sensor Array (using pins A0-A5, 11, and 12)

3S LiPo Battery (for power)

2 Micrometal gear motor

1 360 wheel.

Chasis board. 

Software: C++ (Arduino IDE)

📸 Demo & Schematics

[Video have been attached as files.](https://youtu.be/_y0VWtV_XBU)


🔧 How It Works

This robot's logic is far more advanced than a simple PID controller. It operates using a predictive path-following algorithm.

Read Sensors: The 8 digital sensors are read, and their state is used to calculate a weighted position error. The weights (sensorWeights) determine the line's position relative to the center (0).

Analyze Path (State Machine): The analyzePath() function continuously updates a history of recent error values. It uses this history to analyze the path's curvature, acceleration, and smoothness.

Determine State: Based on this analysis, the determinePathState() function updates the robot's current state (e.g., PATH_STRAIGHT, PATH_SHARP_CURVE).

Adjust PID Gains: The adjustPIDConstants() function selects different KpDynamic and KdDynamic values based on the current state.

Straights: Lower Kp, moderate Kd for stability.

Sharp Curves: High Kp, very high Kd to prevent overshoot.

S-Curves: High Kp, extreme Kd to dampen oscillations.

Calculate Motor Speed: The calculatePIDCorrection() function computes the final PID value. This correction is applied to a baseSpeed (which is also adjusted by the path state) to get the final leftSpeed and rightSpeed.

Handle Line Loss: If all sensors read LOW (!onLine), the handleLineRecovery() function is triggered. It uses the errorHistory to make an intelligent guess about which way to turn and performs an aggressive pivot to find the line again.
