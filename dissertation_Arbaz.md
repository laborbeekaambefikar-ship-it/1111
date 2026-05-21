---
title: "Development of a Smart Autonomous Guided Vehicle Based Warehouse System with Integrated Robotic Arm for Material Picking and Transfer"
author:
  - "Arbaz Rashid (22AEB485)"
  - "Sheikh Noorul A. Usmani (22AEB180)"
institute: "Zakir Hussain College of Engineering & Technology, Aligarh Muslim University"
supervisor: "Prof. Mohammad Muzammil"
date: "2025--2026"
geometry: "top=1in, bottom=1in, left=1in, right=1in"
papersize: a4
fontsize: 12pt
mainfont: "Times New Roman"
linestretch: 1.5
indent: false
numbersections: true
toc: true
header-includes:
  - \usepackage{parskip}
  - \setlength{\parskip}{12pt}
  - \setlength{\parindent}{0pt}
  - \usepackage{ragged2e}
  - \justifying
  - \usepackage{titlesec}
  - \titleformat{\section}{\bfseries\fontsize{14}{16}\selectfont}{\thesection}{1em}{}[\vspace{-4pt}]
  - \titlespacing*{\section}{0pt}{12pt}{12pt}
  - \titleformat{\subsection}{\bfseries\fontsize{12}{14}\selectfont}{\thesubsection}{1em}{}
  - \titlespacing*{\subsection}{0pt}{6pt}{6pt}
---

# Development of a Smart Autonomous Guided Vehicle Based

# Warehouse System with Integrated Robotic Arm

# for Material Picking and Transfer

## A Project Report Submitted in Partial Fulfillment of the Requirements

## for the Award of the Degree of

## Bachelor of Technology

## in

## Automobile Engineering (Electric & hybrid vehicles)

## Submitted by:

## SHEIKH NOORUL A. USMANI, 22AEB180

## ARBAZ RASHID, 22AEB485

## Under the Guidance of:

## Prof. MOHAMMAD MUZAMMIL

## Principal, Zakir Hussain College of Engineering & Technology

## Aligarh Muslim University

## 2025-26

## Declaration

I hereby declare that this project report entitled* “Development of a Smart Autonomous Guided* *Vehicle Based Warehouse System with Integrated Robotic Arm for Material Picking and Transfer”* is the result of my own work and investigation, carried out under the supervision of Prof. Mohammad Muzammil, Department of Mechanical Engineering, Zakir Hussain College of Engineering & Technology, Aligarh Muslim Univeristy. This report has not been submitted elsewhere for the award of any other degree or diploma. All sources of information and literature used have been duly acknowledged.

### Student Name:

### Faculty No.:

### Signature:

### Date:

## Certificate

This is to certify that the project report entitled* “Development of a Smart Autonomous Guided* *Vehicle Based Warehouse System with Integrated Robotic Arm for Material Picking and Transfer”* submitted by (Faculty No. ) is a bonafide record of work carried out under my direct supervision and guidance in partial fulfillment of the requirements for the degree of Bachelor of Technology in Mechanical Engineering during the academic year 2025–2026.

### Project Guide:

### Head of Department:

Designation: Designation: Date: Date:

### External Examiner:

# ACKNOWLEDGEMENT

### In the name of Allah, the Most Gracious, the Most Merciful.

First and foremost, the author expresses sincere gratitude to **Allah (SWT)** for His infinite mercy, countless blessings, guidance, and strength throughout this academic journey. By His grace, the author was able to overcome challenges and successfully complete this project. The author extends his heartfelt appreciation to his parents, **Mr. Rashid Akber** and

### Mrs. Kaniz Sakeena Rashid for their unwavering love, sacrifices, prayers, encouragement,

and constant support. Their guidance, patience, and belief have been a source of inspiration and strength throughout the completion of this work. Special thanks are extended to **Ms. Rida Fatima** for her patience, understanding, unwavering moral support, and encouragement. Her positive influence and constant motivation were invaluable during the course of this project. The author is deeply grateful to **Prof. Mohammad Muzammil** for his invaluable guidance, insightful suggestions, constructive feedback, and continuous encouragement. His mentorship and expertise played a crucial role in shaping and successfully completing this project. Sincere appreciation is also extended to **Mr. Sheikh Noorul**, project partner, for his cooperation, dedication, and collaborative efforts throughout the development of this project. His support, teamwork, and valuable contributions greatly facilitated the successful accomplishment of the project objectives. The author would also like to thank all **faculty members of the Department of Mechanical **

### Engineering for their guidance, encouragement, and support throughout this academic

endeavor. Finally, heartfelt thanks are extended to family members, friends, and well-wishers for their encouragement, understanding, and support, which provided constant motivation throughout this journey.

# Abstract

This project presents the design, implementation, and analysis of a smart Autonomous Guided Vehicle (AGV) based warehouse automation system integrated with a Universal Robots UR3 robotic arm for autonomous material picking and transfer. The system combines a differential- drive mobile platform (ATLAS AGV) with a 6-DOF collaborative manipulator to form a com- plete end-to-end warehouse automation solution capable of navigating to shelf locations, iden- tifying targets via RFID, executing pick-and-place operations, and returning to a home docking station. The ATLAS AGV employs an 8-channel infrared reﬂectance sensor array for line-following navigation at 0.4 m/s, achieving sub-5 mm tracking accuracy under a Proportional-Derivative (PD) control law. Navigation decision-making is governed by a 12-state Finite State Ma- chine (FSM) with RFID-based shelf conﬁrmation at 21 ﬂoor-embedded tag locations. The UR3 arm operates on a ﬁve-layer ROS 2 architecture spanning robot description, Gazebo Har- monic physics simulation, ros2_control real-time control, MoveIt 2 motion planning (OMPL and PILZ planners), and the MoveIt Task Constructor (MTC) compositional task framework with a Robotiq 2F-85 parallel gripper. The integrated system is implemented on ROS 2 comprising 11 packages and a PyQt5-based industrial control centre. Key results demonstrate 100% mission completion rate,* ±*3^◦^^^turn pre- cision, sub-5 mm line tracking accuracy, and complete autonomous operation without human intervention. A simulation-to-physical conversion pathway is documented, conﬁrming that 70% of the control codebase transfers directly to physical hardware. The combined minimum viable build is estimated at approximately $800 with a production-quality build at approxi- mately $1,250.

### Keywords: Autonomous Guided Vehicle, ROS 2, Differential Drive, Line Following, PID

Control, RFID, Pick-and-Place, MoveIt 2, Warehouse Automation, Industry 4.0.

# Notations, Symbols, and Abbreviations

## Symbols

### Symbol

### Description

### Unit

Linear velocity of robot centre m/s

$$*ω*$$
  *(Equation 1)*

Angular velocity of robot rad/s Left wheel linear velocity m/s *v**R* Right wheel linear velocity m/s

$$*ω**L*$$
  *(Equation 2)*

Left wheel angular velocity rad/s

$$*ω**R*$$
  *(Equation 3)*

Right wheel angular velocity rad/s *r* Wheel radius Wheel track width (separation) *R* Instantaneous turning radius

$$*θ*$$
  *(Equation 4)*

Robot heading angle rad *K**p* Proportional controller gain — *K**i* Integral controller gain — *K**d* Derivative controller gain — *e*(*t*) Control error signal — *u*(*t*) Controller output rad/s

$$*τ*$$
  *(Equation 5)*

Motor torque N*·*m *I**xx**, I**yy**, I**zz* Principal moments of inertia kg*·*m^2^ Robot mass kg *g* Gravitational acceleration m/s^2^ *µ* Coefﬁcient of friction — *F**N* Normal force N

$$*α*$$
  *(Equation 6)*

Angular acceleration rad/s^2^

$$*ζ*$$
  *(Equation 7)*

Damping ratio —

$$*ω**n*$$
  *(Equation 8)*

Natural frequency rad/s

$$*σ*$$
  *(Equation 9)*

Standard deviation varies

$$*∆**s*$$
  *(Equation 10)*

Incremental distance

$$*∆**θ*$$
  *(Equation 11)*

Incremental angle rad *w**i* Sensor position weight — *s**i* Binary sensor output *{*0*,* 1*}*

$$a, d, α*DH*, θ$$
  *(Equation 12)*

Denavit-Hartenberg parameters m, m, rad, rad

## Abbreviations

### Acronym

### Full Form

AGV Automated Guided Vehicle AMR Autonomous Mobile Robot CPR Counts Per Revolution DDS Data Distribution Service DH Denavit-Hartenberg DoF Degrees of Freedom EKF Extended Kalman Filter FSM Finite State Machine GPIO General Purpose Input/Output GUI Graphical User Interface HMI Human-Machine Interface I2C Inter-Integrated Circuit ICR Instantaneous Centre of Rotation IMU Inertial Measurement Unit IR Infrared LiDAR Light Detection and Ranging Large Language Model MTC MoveIt Task Constructor OMPL Open Motion Planning Library PD Proportional-Derivative PID Proportional-Integral-Derivative PWM Pulse Width Modulation QoS Quality of Service RFID Radio Frequency Identiﬁcation ROS2 Robot Operating System 2 RPM Revolutions Per Minute SAC Soft Actor-Critic SLAM Simultaneous Localization and Mapping SPI Serial Peripheral Interface TCP Tool Centre Point TF Transform Frame UART Universal Asynchronous Receiver-Transmitter URDF Uniﬁed Robot Description Format WMS Warehouse Management System YOLO You Only Look Once

# Contents

### Declaration

### Certiﬁcate

### Acknowledgement

### Abstract

### Notations, Symbols, and Abbreviations

### Introduction and Literature Review

1.1 Project Overview . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.2 Problem Statement . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.3 Objectives . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.4 Scope . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.5 Methodology . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.6 Literature Review . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1.6.1 Automated Guided Vehicles in Industry . . . . . . . . . . . . . . . . . 1.6.2 Differential Drive Kinematics . . . . . . . . . . . . . . . . . . . . . . 1.6.3 PID Control for Line Following . . . . . . . . . . . . . . . . . . . . . 1.6.4 ROS 2 as Robot Middleware . . . . . . . . . . . . . . . . . . . . . . . 1.6.5 RFID in Warehouse Automation . . . . . . . . . . . . . . . . . . . . . 1.6.6 Robotic Arm Manipulation with ROS 2 . . . . . . . . . . . . . . . . . 1.6.7 Industry 4.0 and Smart Warehousing . . . . . . . . . . . . . . . . . . .

### Problem Formulation

2.1 Core Problem Deﬁnition . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2.2 Technical Sub-Problems . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2.3 Design Constraints and Requirements . . . . . . . . . . . . . . . . . . . . . .

### Modelling, Solution Methodology, and System Design

3.1 Overall System Architecture . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.2 ROS 2 Package Structure . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.3 AGV Mechanical Design . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.3.1 Physical Parameters . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.3.2 Real-World Chassis . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.4 Differential Drive Kinematics . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.4.1 Forward Kinematics . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.4.2 Inverse Kinematics . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.4.3 Instantaneous Centre of Rotation . . . . . . . . . . . . . . . . . . . . . 3.4.4 Odometry Integration . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.4.5 Inertia Tensor . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.5 UR3 Robotic Arm Kinematic Model . . . . . . . . . . . . . . . . . . . . . . . 3.5.1 UR3 Denavit-Hartenberg Parameters . . . . . . . . . . . . . . . . . . 3.5.2 Robotiq 2F-85 Gripper . . . . . . . . . . . . . . . . . . . . . . . . . . 3.6 Sensor Systems . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.6.1 Line Following Sensor Array . . . . . . . . . . . . . . . . . . . . . . . 3.6.2 IMU Sensor . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.6.3 RFID Detection with Hysteresis . . . . . . . . . . . . . . . . . . . . . 3.7 Control Systems Design . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.7.1 Line Following PD Controller . . . . . . . . . . . . . . . . . . . . . . 3.7.2 Turn Controller . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.7.3 Motor Velocity PID . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.8 Mission State Machine . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.9 UR3 Arm Motion Planning . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.9.1 MoveIt 2 Architecture . . . . . . . . . . . . . . . . . . . . . . . . . . 3.9.2 Pick-and-Place Workﬂow . . . . . . . . . . . . . . . . . . . . . . . . 3.9.3 Planner Selection Rationale . . . . . . . . . . . . . . . . . . . . . . . 3.10 Hardware Architecture . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.10.1 Computing Platform . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.10.2 Power System . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.10.3 Simulation-to-Physical Component Conversion . . . . . . . . . . . . . 3.10.4 GPIO Pin Mapping . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.11 Software Deployment Architecture . . . . . . . . . . . . . . . . . . . . . . . . 3.11.1 Files Unchanged for Physical Deployment . . . . . . . . . . . . . . . . 3.11.2 New Hardware Interface Nodes . . . . . . . . . . . . . . . . . . . . . 3.12 GUI and Human-Machine Interface . . . . . . . . . . . . . . . . . . . . . . .

### Results and Discussions with Conclusions

4.1 System Performance Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.2 Mission Timing Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.3 Throughput Estimate . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.4 UR3 Arm Performance . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.5 Comparison with Commercial Systems . . . . . . . . . . . . . . . . . . . . . . 4.6 Challenges and Limitations . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.6.1 Current Limitations . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.6.2 Design Trade-offs . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.7 Future Scope . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.8 Conclusions . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.8.1 Achievements . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.8.2 Contributions . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.8.3 Final Assessment . . . . . . . . . . . . . . . . . . . . . . . . . . . . .

### A Complete State Machine Transition Table

### B

### Software Installation and Launch Guide

### C Viva Examination Questions Summary

# List of Tables

3.1 ROS 2 Package Structure . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.2 AGV Physical Parameters . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.3 UR3 Denavit-Hartenberg Parameters (Modiﬁed DH Convention) . . . . . . . . 3.4 Sensor Speciﬁcations . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.5 Mission State Machine Summary . . . . . . . . . . . . . . . . . . . . . . . . . 3.6 AGV Computing Platform Comparison . . . . . . . . . . . . . . . . . . . . . 3.7 AGV Power Budget . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 3.8 Simulation-to-Physical Component Conversion . . . . . . . . . . . . . . . . . 3.9 Raspberry Pi 4 GPIO Pin Mapping . . . . . . . . . . . . . . . . . . . . . . . . 3.10 Code Changes for Physical Deployment . . . . . . . . . . . . . . . . . . . . . 4.1 System Performance Metrics . . . . . . . . . . . . . . . . . . . . . . . . . . . 4.2 Mission Timing Analysis (Shelf S05) . . . . . . . . . . . . . . . . . . . . . . 4.3 Comparison with Commercial AGV Systems . . . . . . . . . . . . . . . . . . 4.4 Design Trade-offs Summary . . . . . . . . . . . . . . . . . . . . . . . . . . . A.1 Complete AGV FSM Transition Table . . . . . . . . . . . . . . . . . . . . . .

# List of Figures

# Introduction and Literature Review

## Project Overview

Modern warehouses demand high-throughput, error-free material handling with minimal hu- man intervention. Traditional warehouse operations, where workers spend 60–70% of their time on item retrieval walks, create signiﬁcant operational inefﬁciencies [1]. The physical toll of repetitive walking, bending, and lifting in conventional order-picking environments con- tributes to worker fatigue, musculoskeletal disorders, and elevated error rates that compound during extended shifts. According to the Warehousing Education and Research Council, labour costs constitute approximately 65% of total warehouse operating expenditure, with order pick- ing alone accounting for 55% of that labour allocation. These statistics underscore the pressing economic motivation for automated material handling systems. Autonomous Guided Vehicles (AGVs) address these inefﬁciencies through continuous, precise material transport, while integrated robotic arms extend AGV capabilities to the physical ma- nipulation of inventory items. The combination of guided mobile navigation with dexterous arm manipulation represents a complete material-handling automation solution aligned with the goals of Industry 4.0. Unlike standalone AGVs that merely transport entire shelving units (as in the Amazon Kiva paradigm) or stationary robotic arms that can only reach items within a ﬁxed workspace, an integrated mobile manipulator combines the reach advantages of mo- bility with the dexterity of a multi-degree-of-freedom arm. This synergy enables individual item-level picking from arbitrary warehouse locations—the so-called “last metre” problem that remains unsolved by conventional AGV-only or arm-only solutions. This project presents the development of the ATLAS Smart Warehouse AGV, a ROS 2-based autonomous mobile robot integrated with a Universal Robots UR3 6-DOF collaborative arm and a Robotiq 2F-85 parallel gripper. The system is capable of: receiving pick missions via a control GUI; navigating autonomously to target shelf locations using line-following and RFID identiﬁcation; executing structured pick-and-place operations using motion-planned arm trajec- tories; and returning to a home docking station. The entire system operates within a simulated Gazebo warehouse environment with a clearly documented pathway for physical hardware de- ployment. The engineering philosophy underlying this project is simulation-ﬁrst development with hardware- ready architecture. Rather than developing directly on physical hardware—where debugging is slow, component damage is expensive, and iteration cycles are measured in days—the entire control stack is developed and validated within high-ﬁdelity physics simulation. The archi- tectural discipline of separating control logic from hardware interface layers ensures that the transition from simulation to physical deployment requires only the replacement of sensor and actuator driver nodes, while all planning, control, and coordination logic remains unchanged. This approach reduces physical deployment risk, accelerates development velocity, and pro- duces a codebase whose control algorithms have been validated across thousands of simulated mission cycles before encountering real-world hardware. The ATLAS system name is an acronym for Autonomous Transport and Logistics Automation System, reﬂecting the project’s aspiration to address the full material handling pipeline from mission dispatch through navigation, manipulation, and return. The system architecture is deliberately modular: the AGV subsystem and the robotic arm subsystem are independently functional, yet they share a common ROS 2 communication backbone that enables seamless coordination during integrated missions.

## Problem Statement

Contemporary warehouses face critical operational challenges that arise from the fundamental mismatch between the speed and precision requirements of modern e-commerce fulﬁlment and the physical limitations of human order pickers. These challenges manifest across multiple dimensions:

### Labour Efﬁciency: Human labour accounts for a disproportionate share of material retrieval

time. A typical warehouse picker walks 12–15 kilometres per shift, with productive pick- ing (the actual retrieval of items from shelves) constituting only 30–40% of total work time. The remainder is consumed by walking between locations, searching for items, and handling paperwork. This ratio represents a fundamental inefﬁciency that automation can address by eliminating non-productive travel time entirely.

### Error Rates: Manual picking is error-prone, with industry-average pick accuracy rates of

99.0–99.5%. While this appears high in percentage terms, for a warehouse processing 10,000 picks per day, it translates to 50–100 errors daily—each requiring costly returns processing, customer service intervention, and potential loss of customer loyalty. Automated systems with sensor-veriﬁed picking routinely achieve 99.9%+ accuracy.

### Operational Continuity: Round-the-clock operation is constrained by worker availability,

shift change inefﬁciencies, and the physiological limits of human endurance. Automated sys- tems operate continuously with consistent performance, limited only by battery capacity and maintenance schedules.

### Safety: Human-forklift interaction zones pose safety hazards. The Occupational Safety and

Health Administration (OSHA) reports approximately 85 forklift-related fatalities and 34,900 serious injuries annually in the United States alone. Automated guided vehicles operating in segregated zones eliminate this hazard category entirely. **Scalability:** Human workforce scaling is linear and subject to labour market constraints, train- ing overhead, and management complexity. Automated ﬂeet scaling is near-linear with dimin- ishing marginal coordination overhead. Existing commercial AGV solutions, such as the Amazon Robotics Kiva and MiR 250 systems, carry unit costs of $25,000 to $30,000 and above, and are closed systems that do not permit research-level modiﬁcation [2]. These systems are optimised for speciﬁc warehouse conﬁgura- tions and do not readily adapt to academic research environments where algorithm experimen- tation, sensor fusion research, and control strategy comparison require full source-code access and hardware abstraction ﬂexibility. An open-source platform combining mobile AGV navigation with a collaborative robotic arm for integrated picking is therefore needed to bridge the gap between academic research and industrial deployment. This project addresses that need by combining a differential-drive AGV with a six-axis manipulator on the ROS 2 middleware framework. The target cost point of ap- proximately $1,250 for a production build represents a 95% cost reduction relative to commer- cial alternatives, while maintaining comparable functional capability for structured warehouse environments.

## Objectives

The speciﬁc objectives of this project are as follows: 1. Design and implement a differential-drive AGV platform with accurate kinematic mod- elling for warehouse navigation, including forward kinematics, inverse kinematics, in- stantaneous centre of rotation analysis, and odometry integration with second-order ac- curacy. 2. Implement line-following navigation using an 8-channel infrared reﬂectance sensor array and a PD controller, achieving sub-5 mm lateral tracking accuracy at a cruise velocity of 0.4 m/s with demonstrated stability through closed-loop pole analysis. 3. Implement RFID-based shelf identiﬁcation for position conﬁrmation at 21 warehouse locations, with hysteresis-based detection logic to prevent false triggers at tag boundaries. 4. Design and integrate a 12-state Finite State Machine for complete autonomous mission lifecycle management, encompassing idle waiting, spine navigation, turning, aisle nav- igation, shelf arrival, picking, pivoting, return navigation, docking, and emergency stop states. 5. Integrate a UR3 robotic arm with a Robotiq 2F-85 gripper for autonomous pick-and- place operations, including full Denavit-Hartenberg kinematic modelling of the 6-DOF serial chain. 6. Implement MoveIt 2-based motion planning using OMPL and PILZ planners within the MoveIt Task Constructor (MTC) framework, providing both probabilistically complete free-space planning and deterministic Cartesian interpolation for approach and retreat phases. 7. Develop a PyQt5 industrial control centre GUI for mission dispatch, real-time telemetry, and emergency controls, with thread-safe ROS 2 integration and 10 Hz update rates. 8. Demonstrate complete autonomous warehouse operation in a Gazebo simulation envi- ronment with 100% mission completion rate and full mission timing characterisation. 9. Document the simulation-to-physical deployment conversion pathway, identifying all hardware interface nodes requiring replacement and quantifying code reuse at 70%. Each objective is formulated as a measurable engineering deliverable with quantitative accep- tance criteria, enabling objective assessment of project success during validation testing.

## Scope

The project covers the complete robotics stack for a warehouse AGV with a robotic arm, span- ning mechanical design, sensor integration, control systems, motion planning, software archi- tecture, and human-machine interface design.

### AGV Subsystem Scope: The AGV subsystem scope includes mechanical design (URDF/X-

acro model with accurate mass, inertia, and collision properties), sensor systems (8-channel line sensor array, 9-axis IMU, 125 kHz RFID reader), PD-based line-following control with stability analysis, bang-bang turn control with IMU feedback, junction detection with tempo- ral ﬁltering, a 12-state mission Finite State Machine, velocity arbitration for safe multi-source velocity management, odometry integration, and Gazebo Classic 11 physics simulation with ground-truth comparison. **Arm Subsystem Scope:** The arm subsystem scope includes UR3 URDF modelling with accu- rate Denavit-Hartenberg parameters, Gazebo Harmonic physics simulation with realistic joint dynamics, ros2_control integration providing 500 Hz position command interfaces, MoveIt 2 motion planning with both OMPL (RRTConnect) and PILZ (LIN, PTP) planner plugins, MTC- based compositional task planning decomposing pick-and-place into nine sequential stages, Robotiq 2F-85 gripper integration with mimic joint coupling, and planning scene management with collision geometry.

### Integration Scope: The integrated scope includes a uniﬁed ROS 2 communication architecture

with custom message types, topic-based inter-subsystem coordination, a shared PyQt5 GUI providing mission dispatch and real-time telemetry for both subsystems, and a comprehensive hardware deployment guide documenting every simulation-to-physical conversion step.

### Exclusions: The following are explicitly outside the project scope: multi-robot ﬂeet coordina-

tion and trafﬁc management; dynamic obstacle detection and avoidance (the warehouse is as- sumed clear of unexpected obstacles); computer vision-based object detection (pick poses are pre-deﬁned); force/torque-based compliant grasping; and real-time SLAM-based navigation. These exclusions deﬁne clear boundaries while simultaneously identifying natural extension points for future research.

## Methodology

This project follows a simulation-ﬁrst development methodology structured in six phases, each building upon the validated outputs of the preceding phase:

### Phase 1 — Requirements Analysis: Deﬁning warehouse layout geometry (spine corridor, per-

pendicular aisles, shelf positions at known coordinates), arm workspace constraints (500 mm reach radius, table heights, object dimensions), mission types (single-item pick, multi-item sequential pick, return-to-home), and performance requirements (speed, accuracy, reliability targets). This phase produces a formal requirements speciﬁcation against which all subsequent design decisions are validated. **Phase 2 — System Architecture:** Designing the ROS 2 package structure (11 packages with clear dependency relationships), node communication graph (publishers, subscribers, services, actions), hardware interface abstraction strategy (separating control logic from sensor/actuator drivers), and computational resource allocation (AGV on Raspberry Pi 4, arm planning on oper- ator PC). This phase ensures that architectural decisions support both simulation development and physical deployment without structural refactoring.

### Phase 3 — Component Development: Implementing AGV navigation (line follower, turn

controller, junction detector, RFID reader), arm control (MoveIt 2 conﬁguration, MTC pipeline, gripper integration), and GUI nodes (PyQt5 control centre with dual-threaded architecture) independently. Each component is unit-tested in isolation before integration, following the principle of compositional correctness: if each component behaves correctly in isolation and the interfaces are well-deﬁned, the integrated system will behave correctly. **Phase 4 — Integration Testing:** Combining all nodes within the uniﬁed ROS 2 workspace and verifying inter-system communication. This phase validates that topic names, message types, Quality-of-Service policies, and timing assumptions are consistent across all packages. Race conditions, deadlocks, and timing violations are identiﬁed and resolved during this phase. **Phase 5 — System Validation:** Executing complete missions from pick dispatch to home return, measuring all performance metrics, and comparing against requirements. Validation testing includes nominal missions (expected behaviour), edge cases (boundary conditions), and stress testing (rapid sequential missions, emergency stop during motion, queue overﬂow).

### Phase 6 — Performance Analysis: Measuring timing (mission phase durations, controller

response times), accuracy (line tracking error, turn precision, RFID detection reliability, arm positioning accuracy), reliability (mission completion rate across multiple runs), and documen- tation of all results with engineering interpretation and comparison against industrial bench- marks. This six-phase methodology provides traceability from requirements through design to vali- dated results, ensuring that every design decision can be justiﬁed against a documented re- quirement and every performance claim is supported by measured data.

## Literature Review

### Automated Guided Vehicles in Industry

AGVs have evolved signiﬁcantly since their introduction in the 1950s, progressing through ﬁve distinct technological generations that mirror the broader evolution of automation technology. As documented by De Ryck et al. [1], AGV technology can be categorised as follows:

### First Generation (1950s–1970s): Buried wire induction systems pioneered by Barrett Elec-

tronics, where AGVs followed electromagnetic ﬁelds generated by wires embedded in the ware- house ﬂoor. These systems offered reliable guidance but required expensive and disruptive ﬂoor modiﬁcations. The guidance principle relied on detecting the magnetic ﬁeld gradient us- ing paired induction coils, with the vehicle steering to maintain equal ﬂux in both coils—a form of analog proportional control.

### Second Generation (1980s–1990s): Magnetic tape and painted line guidance systems devel-

oped by Daifuku and others, offering easier installation and reconﬁguration than buried wires. These systems reduced infrastructure cost while maintaining deterministic path following. The sensing modality shifted from electromagnetic induction to optical or magnetic detection of surface-mounted guidance media.

### Third Generation (2000s–2010s): Laser triangulation SLAM systems using retroreﬂective

targets mounted on warehouse walls (e.g., SICK NAV350). These systems eliminated the need for ﬂoor-mounted guidance media entirely, enabling ﬂexible path planning and dynamic rerouting. However, they introduced computational complexity and sensitivity to environmen- tal changes such as new equipment or rearranged inventory.

### Fourth Generation (2010s–2020s): Natural feature SLAM systems (MiR, OTTO Motors)

using LiDAR and camera-based simultaneous localisation and mapping without any artiﬁcial landmarks. These systems offer maximum ﬂexibility but require signiﬁcant computational resources (GPU-accelerated inference) and are sensitive to environmental dynamics.

### Fifth Generation (2020s–present): AI-based ﬂeet coordination systems (Amazon Robotics)

combining individual robot autonomy with centralised ﬂeet orchestration using reinforcement learning-based trafﬁc management, predictive maintenance, and demand-responsive dispatch- ing. The ATLAS project implements a second-to-third generation hybrid—line-following with RFID augmentation—which remains the dominant method in structured warehouse environments owing to its reliability, low cost, and deterministic behaviour. This choice is deliberate: for structured environments where paths are ﬁxed and safety certiﬁcation requires deterministic behaviour, line-following provides mathematically provable path adherence that SLAM-based systems cannot guarantee due to their probabilistic nature. Azadeh et al. [2] report that line and tape guidance systems account for approximately 60% of all installed AGV systems globally as of 2023, attributable to ﬁve key advantages: (1) de- terministic behaviour suitable for safety certiﬁcation under ISO 3691-4 [13]; (2) zero mapping requirements enabling immediate deployment without commissioning delays; (3) no sensor degradation over time (unlike LiDAR systems affected by dust accumulation or camera sys- tems affected by lighting changes); (4) low computational requirements permitting operation on embedded processors without GPU acceleration; and (5) predictable, repeatable paths suited to ﬁxed warehouse layouts where path optimality is a solved problem. The industrial relevance of AGV systems is quantiﬁed by market research indicating a global AGV market size of $3.2 billion in 2023, projected to reach $7.8 billion by 2030, driven by labour shortages, e-commerce growth, and the Industry 4.0 transformation of manufacturing and logistics. Within this market, warehouse and distribution centre applications constitute 42% of total AGV deployments, making warehouse automation the single largest application domain for guided vehicle technology.

### Differential Drive Kinematics

The differential drive mechanism is the most common mobile robot drive system due to its zero-turning-radius capability, simple mechanical construction, and straightforward kinematic model [3]. In a differential drive conﬁguration, two independently actuated wheels are mounted on a common axis, with one or more passive caster wheels providing stability. The robot’s lin- ear velocity is determined by the average of the two wheel velocities, while its angular velocity is determined by the difference between wheel velocities divided by the wheel separation dis- tance. Siegwart et al. [3] derive the forward kinematics from individual wheel velocities to robot body velocity and the inverse kinematics from desired robot velocity to wheel commands, both of which are implemented in this project. The mathematical elegance of the differential drive model lies in its linearity: the mapping from wheel velocities to body velocities is a simple linear transformation, and the inverse mapping is equally straightforward. This linearity sim- pliﬁes controller design, stability analysis, and real-time computation on resource-constrained embedded processors. Alternative drive conﬁgurations considered during the design phase include: (a) Ackermann steering (car-like), which provides higher straight-line stability but cannot achieve zero-radius turns, has a larger minimum turning circle, and introduces kinematic complexity through the steering mechanism; (b) omnidirectional (Mecanum or omni-wheels), which enables holo- nomic motion (independent control of x, y, and* θ*) but suffers from reduced traction, higher mechanical complexity, increased cost, and susceptibility to debris jamming; and (c) skid- steering (tank drive), which provides high traction and mechanical simplicity but generates signiﬁcant wheel scrub during turns, causing ﬂoor damage, odometry errors, and elevated en- ergy consumption. The differential drive was selected as the optimal compromise between manoeuvrability (zero-radius turns), simplicity (two motors, one caster), traction (full wheel- ground contact during turns), cost (minimum actuator count), and kinematic tractability (linear forward/inverse models). The kinematic constraints of a differential drive robot deﬁne a non-holonomic system: the robot cannot move instantaneously in the lateral direction. This non-holonomic constraint is expressed as ˙*x* sin* θ** −* ˙*y* cos* θ* = 0, which states that the velocity vector must always be aligned with the robot’s heading. While this constraint limits instantaneous motion capabilities, it does not limit reachability—the robot can reach any point and orientation in the plane through appropriate sequences of forward motion and rotation. This property is essential for warehouse navigation where arbitrary shelf positions must be reachable from a ﬁxed home location.

### PID Control for Line Following

PID control is established as the industry standard for line-following AGVs owing to its math- ematical simplicity, well-understood tuning procedures, and real-time capability [4]. The gen- eral PID control law:

$$∫*t*$$
  *(Equation 13)*

$$*e*(*τ*)*dτ* +*K**d*$$
  *(Equation 14)*

*de*(*t*) *dt* (1.1)

$$*u*(*t*) =*K**p* ·*e*(*t*) +*K**i*$$
  *(Equation 15)*

provides three corrective actions: proportional action (*K**p*) that responds to the current error magnitude, producing an output proportional to the instantaneous deviation from the desired trajectory; integral action (*K**i*) that accumulates historical error to eliminate steady-state offset; and derivative action (*K**d*) that responds to the rate of change of error, providing predictive damping that reduces overshoot and oscillation. The ATLAS system uses a PD controller (*K**I* = 0) as recommended for line-following applica- tions to eliminate integral windup issues at junction transitions [4]. The rationale for omitting the integral term is grounded in the speciﬁc characteristics of line-following control: (1) the controlled variable (lateral position error) is directly measured by the sensor array at every con- trol cycle, eliminating the steady-state errors that integral action is designed to correct; (2) at junction transitions, the line pattern changes abruptly (from a single line to a wide junction re- gion), causing the error signal to change discontinuously—an accumulated integral term would produce inappropriate corrective action during these transitions, potentially causing the robot to deviate from the correct path; (3) the 50 Hz control rate ensures that proportional and deriva- tive actions alone provide sufﬁcient tracking accuracy (sub-5 mm) for the required navigation precision. The Ziegler-Nichols tuning methodology, documented by Franklin et al. [10], provides system- atic gain selection. However, for line-following applications, manual tuning informed by the system’s physical parameters (wheel separation, sensor spacing, maximum velocity) typically produces superior results because the Ziegler-Nichols method assumes a speciﬁc plant model structure (integrator with delay) that does not precisely match the line-following dynamics.

### ROS 2 as Robot Middleware

ROS 2 was selected over ROS 1 based on the following advantages documented by Open Robotics [5]: (1) real-time capability through DDS-based communication that provides bounded latency guarantees essential for safety-critical control loops; (2) decentralisation eliminating the rosmaster single point of failure that plagued ROS 1 deployments—in ROS 2, node discovery is performed by the DDS discovery protocol without any central coordinator; (3) production- quality Quality-of-Service (QoS) policies enabling ﬁne-grained control over message reliabil- ity, durability, deadline, and liveliness; (4) multi-platform support including Linux, Windows, and macOS; and (5) Long-Term Support through 2027 for the Humble release, ensuring soft- ware stability for the project’s operational lifetime. The DDS (Data Distribution Service) communication layer speciﬁed by the Object Manage- ment Group [17] provides the fundamental publish-subscribe messaging infrastructure. DDS was designed for distributed real-time systems in aerospace and defence applications, bringing military-grade reliability to robotic middleware. Key DDS features leveraged by this project in- clude: automatic peer discovery (no conﬁguration ﬁles for node communication), conﬁgurable reliability (best-effort for high-rate sensor data, reliable for mission commands), and deadline monitoring (detecting stale sensor data that could indicate hardware failure). The ROS 2 computation graph model organises software into nodes (processes), topics (named publish-subscribe channels), services (synchronous request-response), and actions (asynchronous goal-feedback-result). This project uses all four communication patterns: topics for continuous sensor data streams (line sensor, IMU, odometry) and velocity commands; services for discrete conﬁguration queries; and actions for long-running operations (arm motion planning, mission execution). The choice of communication pattern for each data ﬂow is driven by the temporal characteristics of the data: periodic sensor readings use topics with appropriate QoS; discrete queries use services; and operations with progress feedback use actions. Alternative middleware frameworks considered include MQTT (lightweight but lacking type safety and QoS guarantees), ZeroMQ (fast but requiring manual serialisation and discovery), and custom TCP/UDP protocols (maximum control but prohibitive development effort). ROS 2 was selected because it provides type-safe message deﬁnitions, automatic serialisation, built-in discovery, conﬁgurable QoS, extensive tool support (rviz2, ros2bag, rqt), and a vast ecosystem of pre-built packages for robot control, motion planning, and simulation.

### RFID in Warehouse Automation

RFID provides position conﬁrmation in structured environments without line-of-sight require- ments, with read-while-moving capability, unique identiﬁcation per location, and passive tags requiring no power or maintenance [6]. The operating principle of passive RFID exploits electromagnetic induction: the reader generates an oscillating magnetic ﬁeld at the carrier frequency (125 kHz for LF systems); when a passive tag enters the ﬁeld, the tag’s antenna coil captures sufﬁcient energy to power its integrated circuit; the circuit then modulates the antenna’s impedance to transmit its stored identiﬁcation number back to the reader via load modulation. Finkenzeller [6] documents the 125 kHz passive RFID systems used in warehouse applications, forming the basis for the RDM6300 reader hardware selected for physical deployment. The 125 kHz frequency band (Low Frequency, LF) was selected over higher-frequency alternatives (13.56 MHz HF, 860–960 MHz UHF) for the following reasons: (1) LF signals are less af- fected by metal and liquid interference common in warehouse environments; (2) the short read range (5–10 cm) provides precise position conﬁrmation rather than the ambiguous zone-level detection of UHF systems; (3) LF passive tags are the lowest-cost RFID technology available ($0.10–0.30 per tag); and (4) the RDM6300 reader module provides a simple UART interface compatible with Raspberry Pi GPIO without additional interface hardware. The ATLAS system employs 21 RFID tags embedded in the warehouse ﬂoor: one home tag at the docking station and 20 shelf tags at intersection points. Each tag stores a unique 10- digit hexadecimal identiﬁer that maps to a speciﬁc warehouse location (shelf ID). The ﬂoor- embedding approach ensures that tags are not accidentally displaced and that read geometry is consistent (reader-to-tag distance is always the robot’s ground clearance, approximately 2– 3 cm). This contrasts with wall-mounted or shelf-mounted tag approaches where the read distance varies with robot position and orientation.

### Robotic Arm Manipulation with ROS 2

The MoveIt 2 motion planning framework is the standard ROS 2 interface for robotic arm ma- nipulation, providing a uniﬁed API for kinematic solvers, motion planners, collision checking, and trajectory execution. Corke [7] provides the mathematical foundations for serial manipula- tor kinematics including the Denavit-Hartenberg (DH) parameter convention used for the UR3 arm in this project. The DH convention provides a minimal parameterisation of the spatial re- lationship between consecutive joint frames using only four parameters per joint (*a*,* d*,* α*,* θ*), enabling systematic construction of the forward kinematic chain as a product of homogeneous transformation matrices. The Open Motion Planning Library (OMPL) implements sampling-based planners including RRTConnect, which serves as the primary planner for free-space arm motions [8]. Sampling- based planners operate by randomly sampling the joint conﬁguration space and connecting valid (collision-free) samples to build a graph or tree that connects the start conﬁguration to the goal. RRTConnect is particularly effective for robotic arm planning because: (1) it grows trees from both start and goal simultaneously, dramatically reducing planning time for the typical manipulation scenario where both start and goal are known; (2) it is probabilisti- cally complete—given sufﬁcient time, it is guaranteed to ﬁnd a solution if one exists; and (3) it handles high-dimensional spaces (6-DOF) efﬁciently without the curse of dimensionality that afﬂicts grid-based planners. The PILZ industrial motion planner provides deterministic Cartesian interpolation (LIN mo- tions) for approach and retreat phases of the pick-and-place cycle. Unlike sampling-based planners that produce stochastic paths varying between planning calls, PILZ generates identical trajectories for identical requests, a property essential for industrial certiﬁcation where repro- ducible behaviour must be demonstrated. PILZ LIN planning interpolates linearly in Cartesian space between two end-effector poses, computing inverse kinematics at each interpolation point to produce a joint trajectory that traces a straight line in task space. This is critical for approach and retreat motions where the gripper must move vertically relative to the object to avoid lateral forces during grasp engagement or release. The MoveIt Task Constructor (MTC) extends MoveIt 2 with compositional task planning— the ability to specify complex multi-stage manipulation tasks as sequences of primitive stages (move, pick, place, modify planning scene). MTC manages the inter-stage constraints that ensure kinematic consistency: the end state of each stage becomes the start state of the next, and the planner backtracks if any stage fails to ﬁnd a solution compatible with its neighbours. This compositional approach enables the speciﬁcation of the complete pick-and-place cycle (nine stages in the ATLAS implementation) as a single planning request that is globally optimised for kinematic feasibility.

### Industry 4.0 and Smart Warehousing

Wurman et al. [9] describe the architecture of the Amazon Kiva system as involving mobile robots, overhead pick stations, and centralised ﬂeet management. The Kiva system pioneered the “goods-to-person” paradigm where mobile robots transport entire shelving units to station- ary human pickers, inverting the traditional “person-to-goods” model. While revolutionary in eliminating picker walking time, this approach has a fundamental limitation: it requires human operators at pick stations to perform the actual item retrieval from the delivered shelf unit. The ATLAS project addresses a comparable functional scope at signiﬁcantly reduced cost using open-source middleware and commercial off-the-shelf hardware. The integration of a collabo- rative robotic arm additionally addresses the “last metre” problem in warehouse automation— the physical transfer of individual items from shelving—which Amazon Kiva and MiR systems do not address autonomously. By combining navigation and manipulation on a single platform, the ATLAS system achieves “person-out-of-the-loop” operation where the entire cycle from mission dispatch to item delivery requires no human intervention. The Industry 4.0 framework emphasises four design principles relevant to this project: (1) interconnection—the ability of machines, devices, and humans to communicate via standard- ised protocols (achieved through ROS 2/DDS); (2) information transparency—digital repre- sentation of the physical system enabling simulation and optimisation (achieved through the Gazebo digital twin); (3) technical assistance—systems that support human decision-making through information aggregation and visualisation (achieved through the PyQt5 control cen- tre); and (4) decentralised decisions—cyber-physical systems making autonomous decisions (achieved through the onboard FSM and motion planning stack). The convergence of affordable computing hardware (Raspberry Pi class), mature open-source robotics middleware (ROS 2), high-ﬁdelity physics simulation (Gazebo), and advanced motion planning frameworks (MoveIt 2) creates an unprecedented opportunity for academic institu- tions to develop and validate warehouse automation research at a fraction of the cost that was possible even ﬁve years ago. The ATLAS project leverages this convergence to demonstrate that the core functionality of systems costing $25,000+ can be replicated at approximately $1,250 using open-source tools and commodity hardware.

# Problem Formulation

## Core Problem Deﬁnition

The central engineering problem addressed by this project is the design of an integrated au- tonomous system capable of performing a complete warehouse pick-and-transfer cycle with- out human intervention. The system must: (i) navigate a known warehouse ﬂoor layout to a commanded shelf location using reliable, deterministic guidance; (ii) conﬁrm its arrival at the correct shelf via a secondary identiﬁcation mechanism; (iii) execute a structured pick operation at the shelf using a robotic arm with appropriate motion planning; (iv) return to a home docking station with the picked item; and (v) repeat the cycle in response to queued missions dispatched from an operator interface. This problem is inherently multi-disciplinary, requiring the simultaneous solution of challenges spanning mechanical engineering (chassis design, centre of gravity, wheel-ground interaction), electrical engineering (sensor interfacing, motor driving, power management), control engi- neering (PD line following, turn control, velocity PID), computer science (state machines, ROS 2 architecture, GUI development), and robotics (kinematics, motion planning, grasp ex- ecution). The integration of these disciplines into a coherent, functional system represents the primary engineering challenge beyond the solution of any individual sub-problem. The problem is further complicated by the dual-system nature of the platform: the AGV and robotic arm subsystems operate on fundamentally different timescales and computational paradigms. The AGV operates in continuous time with real-time control loops at 50 Hz, pro- cessing sensor data and generating velocity commands every 20 ms. The robotic arm operates in a plan-then-execute paradigm, where motion planning may require 0.5–2.0 s of computation followed by trajectory execution lasting 3–10 s. Coordinating these disparate temporal regimes within a uniﬁed mission lifecycle requires careful architectural design to prevent timing con- ﬂicts, race conditions, and unsafe command sequences. The physical constraints of the problem domain impose additional requirements: the ware- house ﬂoor is ﬂat and level (no ramps or elevation changes); the guidance paths are laid out as orthogonal grid lines (spine corridor with perpendicular aisles); shelf positions are at known, ﬁxed locations; and the environment is assumed free of unexpected dynamic obstacles. These constraints simplify the navigation problem while remaining representative of the majority of structured warehouse environments where AGVs are actually deployed.

## Technical Sub-Problems

The core problem decomposes into six coupled technical sub-problems, each requiring inde- pendent solution methodology while maintaining interface compatibility with the others: **Navigation Problem:** The AGV must follow a tape-based guide path with sub-5 mm lateral er- ror at 0.4 m/s, detect junction points for directional decisions, and execute precise 90^◦^^^and 180^◦^ turns. The navigation problem encompasses three distinct control regimes: (a) line-following during straight-path segments, requiring continuous closed-loop control with the PD law op- erating at 50 Hz; (b) junction detection, requiring pattern recognition on the sensor array to distinguish between a straight line (2–3 sensors active) and a junction (5+ sensors active); and (c) turn execution, requiring open-loop angular velocity command with IMU-based termination when the target heading is achieved within* ±*3^◦^. The physics underlying the navigation problem involves the interaction between the differential drive kinematics, the control law, and the sensor geometry. The 8-channel sensor array spans 66.5 mm (7 gaps* ×* 9.5 mm spacing), creating a measurement window within which the line position can be estimated. If the robot deviates beyond this window, the line is lost and recovery behaviour must be invoked. The control law must therefore maintain the lateral error within *±*33 mm at all times, providing a safety margin of approximately 28 mm beyond the 5 mm tracking accuracy target.

### Localisation Problem: Without SLAM, the system must determine its shelf position using

junction counting combined with RFID tag detection, with appropriate hysteresis to prevent false triggers. The localisation strategy is deliberately minimalist: the AGV maintains a junc- tion counter that increments at each detected intersection, and the target shelf is identiﬁed by its junction index along the spine and its RFID tag within the aisle. This approach elimi- nates the computational overhead, environmental sensitivity, and probabilistic uncertainty of SLAM-based localisation while providing deterministic position knowledge in a structured en- vironment. The hysteresis requirement arises from the physical reality of RFID detection: as the robot passes over a ﬂoor-embedded tag, the detection distance transitions smoothly from “out of range” to “in range” and back. Without hysteresis, the system would report multiple detections as the robot oscillates near the detection boundary. The hysteresis model uses a 0.5 m inner (detect) radius and a 0.8 m outer (rearm) radius, creating a 0.3 m dead band that prevents oscillatory detection.

### Mission Management Problem: A state machine must manage the complete lifecycle includ-

ing idle waiting, navigation, picking, returning, and docking, with emergency stop and reset capabilities at all times. The mission management problem is fundamentally a problem of sequential decision-making under deterministic conditions: at each state, the system knows ex- actly what it should do next based on the current state and the triggering event. The 12-state FSM provides complete coverage of all mission phases with well-deﬁned transition conditions and actions. The safety-critical aspect of mission management is the E-Stop override: regardless of the current state, an emergency stop command must immediately halt all motion within a bounded response time (target:* <* 100 ms). This requires that the velocity arbiter—the sole publisher of motor commands—monitors the E-Stop ﬂag at every control cycle and overrides any velocity command with zero when E-Stop is active. The priority architecture of the velocity arbiter is: (1) E-Stop override (highest priority, always zero); (2) turn velocity (during turning states); (3) navigation velocity (during line-following states); (4) dock velocity (during docking states); (5) zero (idle/stopped states).

### Manipulation Problem: The UR3 arm must plan and execute collision-free trajectories from

a home conﬁguration to grasp poses, execute a stable grasp using the Robotiq gripper, and transfer the object to a target location within the constraints of the arm workspace and obsta- cle geometry. The manipulation problem decomposes into: (a) motion planning—ﬁnding a collision-free joint trajectory from start to goal conﬁguration; (b) grasp planning—determining the gripper pose and aperture that will achieve a stable grasp on the target object; (c) trajectory execution—commanding the joint trajectory to the robot controller with appropriate velocity and acceleration proﬁles; and (d) planning scene management—maintaining an accurate repre- sentation of the environment (tables, shelves, objects) for collision checking. The kinematic constraints of the UR3 (500 mm reach, 6 joints, joint limits) deﬁne the reach- able workspace within which pick-and-place operations can be performed. Objects located outside this workspace are unreachable, and objects near workspace boundaries may have lim- ited approach directions due to joint limit constraints. The motion planning algorithm must ﬁnd collision-free paths within this constrained space, avoiding self-collision (arm hitting it- self), environment collision (arm hitting tables or shelves), and joint limit violations.

### System Integration Problem: The AGV navigation and arm manipulation sub-systems must

share a common ROS 2 communication architecture and a uniﬁed mission logic without intro- ducing race conditions or unsafe velocity commands. The integration problem is primarily an architectural challenge: deﬁning clean interfaces between subsystems, establishing communi- cation protocols, managing shared state (mission status, emergency stop ﬂags), and ensuring temporal coordination (the arm must not begin picking until the AGV has stopped and con- ﬁrmed its position; the AGV must not begin return navigation until the arm has completed its pick and returned to home conﬁguration). The ROS 2 topic-based communication model naturally supports this coordination through published state transitions: the mission FSM publishes state change notiﬁcations that both sub- systems subscribe to, enabling event-driven coordination without tight coupling. This publish- subscribe architecture ensures that adding, modifying, or removing subsystem nodes does not require changes to other subsystems—a critical property for maintaining software quality as the system grows in complexity.

### Simulation-to-Physical Conversion Problem: The simulation architecture must minimise the

code changes required for physical hardware deployment, with hardware interface abstraction separating control logic from sensor and actuator drivers. The conversion problem is addressed through the principle of hardware interface abstraction: all sensor reading and actuator com- manding is performed through standardised ROS 2 topic interfaces. In simulation, Gazebo plugins publish sensor data and subscribe to actuator commands; in physical deployment, hard- ware driver nodes perform the same function. The control logic layer above these interfaces operates identically in both environments because it communicates only through abstract topic interfaces, never directly with hardware.

## Design Constraints and Requirements

The system design is governed by a comprehensive set of constraints spanning physical, com- putational, economic, and regulatory domains:

### Physical Constraints:

• Warehouse ﬂoor layout is ﬁxed with known shelf positions at coordinates (*x**s**, y**a*) where *x**s** ∈ {*1*,* 2*,* 3*,* 4*}* m and* y**a** ∈ {*2*,* 4*,* 6*,* 8*,* 10*}* m. • Navigation must be deterministic and certiﬁably safe—the robot must never deviate from its designated path under normal operating conditions. • The arm must operate within a 500 mm reach radius, consistent with the UR3’s kinematic workspace. • Total robot mass must not exceed 5 kg to maintain adequate traction-to-weight ratio with the selected motor torque capacity. • The centre of gravity must be maintained above the drive axle to prevent tip-over during acceleration and deceleration.

### Computational Constraints:

• The AGV control system must run on a Raspberry Pi 4 (4 GB RAM, 4-core ARM Cortex- A72 at 1.5 GHz) without GPU acceleration. • The arm planning system runs on an operator PC with greater computational resources, communicating with the AGV via WiFi-based DDS discovery. • Control loop rates: line following at 50 Hz, turn control at 50 Hz, motor PID at 100 Hz, arm controller at 500 Hz. • Maximum allowable latency for safety-critical commands (E-Stop): 20 ms from com- mand publication to motor stop.

### Economic Constraints:

• Total system cost must not exceed $1,300 for a production-quality build. • Minimum viable prototype cost target: approximately $800. • All software must be open-source (no proprietary licensing costs). • Hardware components must be commercially available from standard suppliers.

### Software Constraints:

• All software is based on ROS 2 Humble (AGV) and ROS 2 Jazzy (Arm). • Programming languages: Python (AGV navigation, GUI) and C++ (arm MTC pipeline). • Build system: colcon with ament_cmake and ament_python. • Simulation: Gazebo Classic 11 (AGV) and Gazebo Harmonic (Arm).

# Modelling, Solution Methodology, and System Design

## Overall System Architecture

The ATLAS Smart Warehouse AGV system is structured as two tightly coupled subsystems operating within a uniﬁed ROS 2 communication framework. The AGV subsystem handles mobile navigation and mission management; the Arm subsystem handles object manipulation at target locations. This bipartite architecture reﬂects the fundamental difference in operational paradigms between mobile navigation (continuous, reactive control) and manipulation (dis- crete, planned motion sequences) while maintaining seamless coordination through the shared communication layer. The AGV subsystem follows a layered architecture inspired by the classical robotics sense- plan-act paradigm, extended with explicit safety and interface layers: 1.** Perception Layer** — Line sensor array (8-channel IR reﬂectance providing weighted lateral error), IMU (9-axis BNO055 providing heading via quaternion decomposition), RFID reader (125 kHz passive tag detection for position conﬁrmation), and wheel odom- etry (encoder-based dead reckoning with midpoint integration). This layer transforms raw sensor signals into meaningful state estimates published as ROS 2 topics. 2.** Control Layer** — Line follower (PD controller generating angular velocity correction from lateral error), turn controller (bang-bang heading controller with IMU feedback), and velocity arbiter (priority-based multiplexer ensuring single-source velocity com- mand). This layer implements the real-time control laws that maintain desired motion behaviour. 3.** Planning Layer** — Mission FSM (12-state machine managing mission lifecycle), queue manager (FIFO mission queue with priority override capability), and junction counter (discrete position estimator for spine navigation). This layer makes navigation decisions based on mission requirements and current position knowledge. 4.** Actuation Layer** — Differential drive controller (sole cmd_vel publisher converting desired linear and angular velocity to wheel commands via inverse kinematics). The architectural decision to have a single velocity publisher eliminates the possibility of conﬂicting velocity commands from multiple sources—a critical safety property. 5.** Interface Layer** — PyQt5 GUI (industrial control centre with mission dispatch, teleme- try display, and emergency controls) and CLI tools (command-line mission dispatch for scripted testing). This layer provides human operators with visibility and control over system operation. The Arm subsystem follows a ﬁve-layer architecture that mirrors the standard industrial robotics software stack: 1.** Physics/Hardware Layer** — Gazebo Harmonic simulation providing realistic rigid- body dynamics, joint friction modelling, and gravity compensation. In physical deploy- ment, this layer is replaced by the actual UR3 hardware communicating via EtherCAT at 500 Hz. 2.** Control Layer** — ros2_control with JointTrajectoryController (for arm joints, executing time-parameterised trajectories) and GripperActionController (for the Robotiq 2F-85, commanding ﬁnger position). Both controllers operate at 500 Hz, matching the UR robot’s native communication rate. 3.** Motion Planning Layer** — MoveIt 2 providing kinematic solvers (KDL for inverse kinematics), collision checking (FCL library), and planner plugins (OMPL for sampling- based planning, PILZ for deterministic Cartesian planning). This layer transforms high- level motion goals (target pose) into executable joint trajectories. 4.** Task Planning Layer** — MoveIt Task Constructor (MTC) providing compositional multi- stage task speciﬁcation. MTC decomposes complex manipulation tasks into ordered sequences of primitive stages, managing inter-stage kinematic constraints and enabling backtracking when individual stages fail. 5.** Application Layer** — Python and C++ task nodes that deﬁne speciﬁc manipulation tasks (pick-from-shelf, place-on-table) by conﬁguring MTC stage sequences and dispatching planning requests. The inter-subsystem communication architecture uses ROS 2 topics for event-driven coordina- tion. The AGV mission FSM publishes state transitions on the /atlas/mission_state topic; the arm application layer subscribes to this topic and initiates pick operations when the AT_SHELF state is announced. Conversely, the arm application publishes a completion signal on /atlas/pick_complete when the pick-and-place cycle ﬁnishes, triggering the AGV’s transition from PICKUP to PIVOT state. This decoupled communication ensures that neither subsystem requires direct knowledge of the other’s internal implementation—they interact only through well-deﬁned message interfaces.

## ROS 2 Package Structure

The system comprises 11 ROS 2 packages as described in Table 3.1. The package structure follows the ROS 2 best practice of separating concerns into distinct, independently buildable packages with explicit dependency declarations. This modular structure enables parallel de- velopment (different team members can work on different packages without merge conﬂicts), incremental building (only changed packages need recompilation), and clear dependency man- agement (circular dependencies are structurally prevented). Table 3.1: ROS 2 Package Structure

### Package

### Build Type

### Purpose

atlas_interfaces ament_cmake Custom AGV message deﬁnitions atlas_description ament_cmake AGV URDF/Xacro model atlas_gazebo ament_cmake AGV Gazebo world ﬁle atlas_navigation ament_python AGV sensor and controller nodes atlas_mission_manager ament_python Mission FSM, GUI, CLI atlas_bringup ament_cmake Master AGV launch ﬁle ur_description ament_cmake UR3 URDF/Xacro robot model ur_gazebo ament_cmake UR3 Gazebo simulation launch moveit_conﬁg ament_cmake MoveIt 2 SRDF, kinematics, OMPL conﬁg ur_mtc_pick_place_demo ament_cmake MTC pick-and-place pipeline ur_interfaces ament_cmake Custom arm message deﬁnitions The dependency graph between packages follows a strict layered hierarchy: interface pack- ages (atlas_interfaces, ur_interfaces) have no internal dependencies and are depended upon by all other packages requiring custom message types; description packages (atlas_description, ur_description) depend only on standard ROS 2 robot model packages; simulation packages (atlas_gazebo, ur_gazebo) depend on description packages and simulation infrastructure; nav- igation and control packages (atlas_navigation, moveit_conﬁg) depend on interfaces and de- scription; and application packages (atlas_mission_manager, ur_mtc_pick_place_demo) de- pend on all lower layers. The bringup package (atlas_bringup) serves as the top-level orches- trator, launching all required nodes with appropriate conﬁguration. The choice of build type—ament_cmake for C++ packages and packages with complex build requirements (URDF processing, conﬁg ﬁle installation), ament_python for pure Python packages— reﬂects the language selection rationale: Python is used for rapid-prototyping, high-level logic, and GUI development where execution speed is not critical; C++ is used for performance- critical motion planning (MTC pipeline) and packages requiring tight integration with the MoveIt 2 C++ API.

## AGV Mechanical Design

### Physical Parameters

The ATLAS AGV physical parameters as deﬁned in atlas_agv.urdf.xacro are sum- marised in Table 3.2. These parameters were selected through an iterative design process bal- ancing multiple competing requirements: the chassis must be large enough to accommodate all electronics and the battery, yet small enough to navigate warehouse aisles without collisions; the mass must be sufﬁcient to provide adequate traction for acceleration and turning, yet light enough that the selected motors can achieve the required velocity; and the wheel dimensions must balance traction area (larger diameter), turning agility (smaller track width), and ground clearance (larger radius). Table 3.2: AGV Physical Parameters

### Parameter

### Value

### Description

$$Body (*B**X* ×*B**Y* ×*B**Z*)$$
  *(Equation 16)*

$$0.30×0.25×0.10 m$$
  *(Equation 17)*

Chassis footprint Total mass 2.5 kg Robot mass Wheel radius (*r*) 0.05 m Drive wheel size Wheel track (*L*) 0.30 m Distance between wheels Wheel width 0.04 m Tyre width Caster radius 0.025 m Passive support wheel Drive type Differential drive Two powered wheels + caster The chassis dimensions of 300* ×* 250* ×* 100 mm were determined by the following space allocation: the battery (3S LiPo, approximately 150* ×* 50* ×* 25 mm) is positioned centrally above the drive axle; the Raspberry Pi 4 (85*×*56 mm) is mounted on the upper deck; the motor driver (L298N, 43* ×* 43 mm) is positioned adjacent to the motors; and the sensor electronics (RFID reader, IMU, ADC) occupy the remaining space. A 20 mm clearance margin on all sides prevents interference and allows cable routing. The total mass of 2.5 kg is distributed as follows: chassis structure (aluminium frame and plates) approximately 0.8 kg; two DC gear motors approximately 0.4 kg; battery approximately 0.4 kg; Raspberry Pi and electronics approximately 0.3 kg; wheels and hardware approximately 0.2 kg; and cabling/mounting hardware approximately 0.4 kg. This mass budget was validated against the motor torque speciﬁcation to ensure adequate acceleration capability: with a motor stall torque of 0.5 N*·*m per wheel and a wheel radius of 0.05 m, the maximum tractive force is *F* =* τ/r* = 0*.*5*/*0*.*05 = 10 N per wheel, or 20 N total. The maximum theoretical acceleration is therefore* a* =* F/m* = 20*/*2*.*5 = 8 m/s^2^, far exceeding the gentle acceleration proﬁles required for line-following navigation (typically* <* 0*.*5 m/s^2^). The wheel radius of 50 mm was selected as a compromise between ground clearance require- ments (the sensor array and RFID reader must be mounted below the chassis, requiring at least 20 mm clearance) and the relationship between motor RPM and robot speed. For the selected

$$200 RPM motors, the maximum linear speed is*v**max* =*ω**max* ·*r* = (200 × 2π/60) × 0.05 =$$
  *(Equation 18)*

1*.*05 m/s, providing a comfortable margin above the 0.4 m/s cruise speed with 62% speed head- room for acceleration transients. The wheel track width of 300 mm (equal to chassis width) maximises turning stability by plac- ing the wheels at the outermost chassis extent. A wider track increases the moment arm for differential turning torque, reducing the angular velocity achievable for a given speed differ- ence, but increases turning stability and reduces the risk of wheel lift during aggressive turns. The track-to-wheelbase ratio of 300:250 (1.2:1) falls within the recommended range of 1.0–1.5 for differential drive robots, ensuring stable straight-line tracking without excessive sensitivity to speed asymmetries.

### Real-World Chassis

The physical chassis uses 6061 aluminium extrusion (20*×*20 mm T-slot) with a 300*×*250 mm aluminium base plate (3 mm thick), 3 mm L-bracket motor mounts, and an acrylic or alu- minium top deck for electronics. The 6061 alloy was selected for its combination of adequate structural strength (yield strength 276 MPa), low density (2.7 g/cm^3^), excellent machinabil- ity, corrosion resistance, and widespread availability in standard extrusion proﬁles. The T-slot extrusion system enables reconﬁgurable mounting of components without drilling or welding, facilitating iterative design reﬁnement during prototype development. The centre of gravity is maintained above the drive axle by placing the battery (the single heaviest component at 0.4 kg) directly above the wheel axis to maximise traction and sta- bility. This deliberate CG placement ensures that the normal force on the drive wheels is maximised (approximately 70% of total weight on the drive axle, 30% on the caster), pro- viding adequate traction for acceleration, deceleration, and turning without wheel slip. The vertical CG position (approximately 50 mm above the base plate, or 100 mm above ground) is sufﬁciently low relative to the track width (300 mm) that tip-over is geometrically impos- sible during normal operation: the maximum lateral acceleration that could cause tip-over is

$$*a**tip* =*g* · (L/2)/h*CG* = 9.81 × 0.15/0.10 = 14.7 m/s2, far exceeding any lateral acceleration$$
  *(Equation 19)*

produced by the differential drive during turns (maximum* ω*^2^*·**r**turn** ≈* 0*.*4^2^*×*0*.*15 = 0*.*024 m/s^2^ at the minimum turning radius). The motor mounting design uses L-brackets with slotted holes to permit ﬁne adjustment of wheel alignment. Parallel wheel alignment is critical for straight-line tracking: even a 1^◦^^^toe-in or toe-out error would cause systematic drift requiring continuous correction from the line- following controller, increasing energy consumption and reducing tracking accuracy. The slot- ted mounting holes allow post-assembly alignment using a straightedge reference.

## Differential Drive Kinematics

The differential drive kinematic model relates the independently controlled wheel velocities to the robot’s body velocity in the plane. This section presents the complete kinematic analysis including forward kinematics, inverse kinematics, instantaneous centre of rotation, odometry integration, and inertia characterisation.

### Forward Kinematics

Given left wheel velocity* v**L* and right wheel velocity* v**R*, the robot’s linear and angular veloc- ities are derived from the geometric constraint that both wheels roll without slipping on a ﬂat surface. The linear velocity of the robot’s centre point (midpoint of the axle) is the average of the two wheel velocities:

$$*v* = *v**R* +*v**L*$$
  *(Equation 20)*

(3.1) The angular velocity about the robot’s vertical axis (yaw rate) is proportional to the velocity difference between wheels, divided by the wheel track width:

$$*ω* = *v**R* −*v**L*$$
  *(Equation 21)*

(3.2) These two equations constitute the complete forward kinematic model: given any pair of wheel velocities (*v**L**, v**R*), the body velocity (*v, ω*) is uniquely determined. The physical interpretation is intuitive: when both wheels turn at equal speed, the robot moves in a straight line (*ω* = 0); when the wheels turn at different speeds, the robot follows a circular arc; and when the wheels turn at equal speeds in opposite directions, the robot spins in place (*v* = 0, maximum* ω*). The robot pose (position and orientation in the world frame) evolves according to the kinematic differential equations:

$$˙*x* =*v**cos* θ,$$
  *(Equation 22)*

$$˙*y* =*v**sin* θ,$$
  *(Equation 23)*

$$˙*θ* =*ω*$$
  *(Equation 24)*

(3.3) These equations describe a non-holonomic system: the velocity constraint ˙*x* sin* θ** −* ˙*y* cos* θ* = 0 (no lateral slip) reduces the instantaneous degrees of freedom from three (x, y,* θ*) to two (v,* ω*). While this constraint limits instantaneous motion capability, it does not limit the set of reachable conﬁgurations—any pose (*x, y, θ*) in the plane is reachable from any other pose through appropriate control inputs, a property known as small-time local controllability.

### Inverse Kinematics

Given desired robot velocity (*v, ω*), the wheel velocities are computed by inverting the forward kinematic equations:

$$*v**L* =*v* −*ω* · *L*$$
  *(Equation 25)*

(3.4)

$$*v**R* =*v* +*ω* · *L*$$
  *(Equation 26)*

(3.5) Converting to wheel angular velocities (required for motor speed commands):

$$*ω**L* = *v**L*$$
  *(Equation 27)*

*r *^,^

$$*ω**R* = *v**R*$$
  *(Equation 28)*

*r* (3.6) For the ATLAS parameters (*L* = 0*.*30 m,* r* = 0*.*05 m), the inverse kinematics at cruise condi-

$$tions (*v* = 0.4 m/s,*ω* = 0) yields:*v**L* =*v**R* = 0.4 m/s,*ω**L* =*ω**R* = 0.4/0.05 = 8 rad/s$$
  *(Equation 29)*

= 76.4 RPM. During a maximum-rate turn (*v* = 0,* ω* = 0*.*4 rad/s):* v**L* =* −*0*.*06 m/s, *v**R* = +0*.*06 m/s, requiring* ω**L* =* −*1*.*2 rad/s and* ω**R* = +1*.*2 rad/s = 11.5 RPM. These values are well within the 200 RPM motor capability, conﬁrming adequate actuator authority for all planned manoeuvres. The inverse kinematics also reveals the actuator saturation limit: the maximum angular ve- locity achievable without exceeding the motor speed limit is* ω**max* = 2* ·** v**max,motor**/L* = 2* ×* 1*.*05*/*0*.*30 = 7*.*0 rad/s when* v* = 0 (pure rotation). In practice, the operating point is far below this limit, providing substantial safety margin against actuator saturation.

### Instantaneous Centre of Rotation

The Instantaneous Centre of Rotation (ICR) is the point in the plane about which the robot instantaneously rotates. Its distance from the robot centre is:

$$*R* = *L*$$
  *(Equation 30)*

$$2 ·*v**R* +*v**L*$$
  *(Equation 31)*

*v**R** −** v**L* (3.7) The ICR concept provides geometric insight into the robot’s motion: **Special Case 1 — Straight-Line Motion:** When* v**R* =* v**L*, the denominator approaches zero and* R** → ∞*. The robot moves in a straight line, which can be interpreted as circular motion about a point at inﬁnite distance (inﬁnite radius curve = straight line). **Special Case 2 — Spin-in-Place:** When* v**R* =* −**v**L*, the numerator is zero and* R* = 0. The ICR coincides with the robot centre, meaning the robot rotates about its own centre without translating. This is the zero-turning-radius capability that distinguishes differential drive from Ackermann steering. **Special Case 3 — Pivot About One Wheel:** When* v**L* = 0 (left wheel stationary),* R* =* L/*2. The robot pivots about its left wheel. Similarly, when* v**R* = 0, the robot pivots about its right wheel with* R* =* −**L/*2. The ICR analysis is particularly relevant for understanding the robot’s swept area during turns, which determines minimum aisle width requirements in the warehouse layout. For a spin-in- place turn, the swept circle radius equals the distance from the robot centre to its farthest corner:

$$*r**swept* =$$
  *(Equation 32)*

$$√$$
  *(Equation 33)*

$$(*B**X*/2)2 + (*B**Y* /2)2 =$$
  *(Equation 34)*

$$√$$
  *(Equation 35)*

0*.*15^2^^^+ 0*.*125^2^^^= 0*.*195 m. This deﬁnes the minimum clear space required at turning points in the warehouse.

### Odometry Integration

Position is computed by dead reckoning using midpoint integration (second-order accuracy), which provides superior accuracy compared to simple Euler integration (ﬁrst-order) by evalu- ating the trigonometric functions at the midpoint heading rather than the initial heading:

$$*∆**s* = *∆**s**R* + ∆*s**L*$$
  *(Equation 36)*

*,*

$$*∆**θ* = *∆**s**R* −*∆**s**L*$$
  *(Equation 37)*

(3.8)

$$*x**k*+1 =*x**k* + ∆*s* ·*cos*$$
  *(Equation 38)*

(

$$*θ**k* + *∆**θ*$$
  *(Equation 39)*

) (3.9)

$$*y**k*+1 =*y**k* + ∆*s* ·*sin*$$
  *(Equation 40)*

(

$$*θ**k* + *∆**θ*$$
  *(Equation 41)*

) (3.10)

$$*θ**k*+1 =*θ**k* + ∆*θ*$$
  *(Equation 42)*

(3.11) The midpoint integration method assumes that the robot’s trajectory between two consecutive encoder readings is a circular arc, and evaluates the arc at its midpoint heading* θ**k* + ∆*θ/*2. This assumption is valid when the control update rate is sufﬁciently high relative to the robot’s angular velocity—speciﬁcally, when ∆*θ* per update step is small (typically* <* 5^◦^). At the ATLAS operating parameters (50 Hz control rate, 0.4 rad/s maximum angular velocity), the maximum ∆*θ* per step is 0*.*4*/*50 = 0*.*008 rad = 0*.*46^◦^, well within the accuracy regime of midpoint integration. The incremental wheel distances ∆*s**L* and ∆*s**R* are computed from encoder counts: ∆*s* = (*N**counts**/CPR*)* ×* 2*πr*, where* N**counts* is the number of encoder counts since the last update and CPR is the encoder resolution in counts per revolution. For typical quadrature encoders with 360 CPR and the ATLAS wheel radius of 0.05 m, the distance resolution is (1*/*360)* ×* 2*π** ×* 0*.*05 = 0*.*87 mm per count, providing sub-millimetre position measurement granularity. Odometry drift is inherent in dead reckoning systems due to systematic errors (wheel diameter mismatch, imperfect wheel alignment) and random errors (wheel slip, encoder quantisation). For the ATLAS system, odometry is used only for local navigation between junction resets— the RFID tag detection at each junction effectively resets the accumulated position error, pre- venting unbounded drift. This design choice acknowledges the fundamental limitation of dead reckoning while exploiting the structured environment to bound cumulative error.

### Inertia Tensor

For the rectangular chassis (uniform density,* m* = 2*.*5 kg, 0*.*30* ×* 0*.*25* ×* 0*.*10 m), the principal moments of inertia about the centre of mass are computed assuming a uniform rectangular parallelepiped:

$$*I**xx* = *m*(*b*2 +*c*2)$$
  *(Equation 43)*

$$= 2.5(0.252 + 0.102)$$
  *(Equation 44)*

$$= 0.01510 kg · m2$$
  *(Equation 45)*

(3.12)

$$*I**yy* = *m*(*a*2 +*c*2)$$
  *(Equation 46)*

$$= 2.5(0.302 + 0.102)$$
  *(Equation 47)*

$$= 0.02083 kg · m2$$
  *(Equation 48)*

(3.13)

$$*I**zz* = *m*(*a*2 +*b*2)$$
  *(Equation 49)*

$$= 2.5(0.302 + 0.252)$$
  *(Equation 50)*

$$= 0.03177 kg · m2$$
  *(Equation 51)*

(3.14) The yaw moment of inertia* I**zz* = 0*.*03177 kg*·*m^2^^^is the most signiﬁcant for navigation dynam- ics, as it determines the angular acceleration achievable for a given net torque from the drive wheels. The maximum yaw torque produced by the differential drive (equal and opposite wheel forces at distance* L/*2 from centre) is:

$$*τ*yaw,max = 2 ·*F**wheel* · *L*$$
  *(Equation 52)*

$$2 = 2 × 10 × 0.15 = 3.0^N^ ·^m^$$
  *(Equation 53)*

(3.15) The resulting maximum angular acceleration is:

$$*α**max* = *τ*yaw,max$$
  *(Equation 54)*

*I**zz*

$$=$$
  *(Equation 55)*

3*.*0

$$0.03177 = 94.4^rad/s^2$$
  *(Equation 56)*

(3.16) This extremely high angular acceleration capability (relative to the 0.4 rad/s operating speed) conﬁrms that the motor torque is more than adequate for the planned turn manoeuvres. The time to reach the turning speed from rest is* t**acc* =* ω**turn**/α**max* = 0*.*4*/*94*.*4 = 4*.*2 ms, indicating that the acceleration transient is negligible compared to the turn duration (3.9 s for 90^◦^). The inertia tensor values are speciﬁed in the URDF model for accurate physics simulation in Gazebo. Incorrect inertia values would cause the simulated robot to behave unrealistically during turns (too responsive if inertia is too low, too sluggish if too high), compromising the ﬁdelity of the simulation-ﬁrst development approach.

## UR3 Robotic Arm Kinematic Model

The Universal Robots UR3 is a 6-DOF collaborative serial manipulator designed for tabletop applications requiring high precision in a compact workspace. With a payload rating of 3 kg, a reach of 500 mm, and a repeatability of* ±*0*.*1 mm, the UR3 is ideally suited for warehouse pick- and-place operations involving small to medium items (books, boxes, electronic components). The collaborative nature of the UR3—certiﬁed to ISO/TS 15066 for power and force limiting— enables operation in proximity to human workers without safety fencing, a critical feature for warehouse environments where complete human exclusion is impractical. The UR3’s serial kinematic chain comprises six revolute joints arranged in an anthropomor- phic conﬁguration: a 1-DOF shoulder pan (joint 1, vertical axis rotation), a 2-DOF shoulder (joint 2, horizontal axis providing elevation), an elbow (joint 3, providing reach extension/re- traction), and a 3-DOF spherical wrist (joints 4–6, providing end-effector orientation control). This 6-DOF architecture provides exactly the minimum degrees of freedom required to achieve arbitrary position and orientation of the end-effector in 3D space (3 DOF for position + 3 DOF for orientation = 6 total), without the redundancy that would complicate inverse kinematics. The selection of the UR3 over alternative manipulators was driven by: (1) appropriate payload capacity for warehouse items (3 kg covers 80%+ of e-commerce package weights); (2) compact workspace compatible with AGV-mounted operation; (3) comprehensive ROS 2 driver support through the Universal Robots ROS 2 driver package; (4) extensive MoveIt 2 conﬁguration availability; (5) well-documented Denavit-Hartenberg parameters for kinematic modelling; and (6) established industrial deployment track record demonstrating reliability in 24/7 operation.

### UR3 Denavit-Hartenberg Parameters

The modiﬁed DH parameters for the UR3 are given in Table 3.3. These parameters deﬁne the geometric relationships between consecutive joint frames according to the modiﬁed Denavit- Hartenberg convention, where each transformation from frame* n**−*1 to frame* n* is decomposed into four elementary operations: rotation about* z**n**−*1 by* θ**n*, translation along* z**n**−*1 by* d**n*, trans- lation along* x**n* by* a**n*, and rotation about* x**n* by* α**n*. Table 3.3: UR3 Denavit-Hartenberg Parameters (Modiﬁed DH Convention)

### Joint

*a*** (m)** *d*** (m)** *α*** (rad)** *θ*** offset** 1 (shoulder_pan) 0.0 0.1519 *π/*2 2 (shoulder_lift) *−*0*.*24365 0.0 3 (elbow) *−*0*.*21325 0.0 4 (wrist_1) 0.0 0.11235 *π/*2 5 (wrist_2) 0.0 0.08535 *−**π/*2 6 (wrist_3) 0.0 0.0819 The forward kinematics chain is computed as: *T* ^6^

$$*base* =*T* 1$$
  *(Equation 57)*

0* *^·^^T^^2^ 1* *^·^^T^^3^ 2* *^·^^T^^4^ 3* *^·^^T^^5^ 4* *^·^^T^^6^ (3.17) where each transformation follows the standard DH product: *T** *^n^

$$*n*−1 =*Rot**z*(*θ**n*) ·*Trans**z*(*d**n*) ·*Trans**x*(*a**n*) ·*Rot**x*(*α**n*)$$
  *(Equation 58)*

(3.18) Expanding the homogeneous transformation matrix for a single joint:  

$$*cos**θ**n*$$
  *(Equation 59)*

$$−*sin**θ**n**cos**α**n*$$
  *(Equation 60)*

$$*sin**θ**n**sin**α**n*$$
  *(Equation 61)*

$$*a**n**cos**θ**n*$$
  *(Equation 62)*

$$*sin**θ**n*$$
  *(Equation 63)*

$$*cos**θ**n**cos**α**n*$$
  *(Equation 64)*

$$−*cos**θ**n**sin**α**n*$$
  *(Equation 65)*

$$*a**n**sin**θ**n*$$
  *(Equation 66)*

$$*sin**α**n*$$
  *(Equation 67)*

$$*cos**α**n*$$
  *(Equation 68)*

*d**n*   (3.19) *T** *^n^

$$*n*−1 =$$
  *(Equation 69)*

The resulting 4* ×* 4 homogeneous transformation matrix* T* ^6^ *base* ^encodes both the position (trans-^ lation components in the fourth column) and orientation (rotation submatrix in the upper-left 3* ×* 3 block) of the end-effector frame relative to the base frame, as a function of the six joint

$$angles (*θ*1, θ2, θ3, θ4, θ5, θ6).$$
  *(Equation 70)*

The inverse kinematics problem—ﬁnding joint angles (*θ*1*, ..., θ*6) that place the end-effector at a desired pose—is solved numerically by the KDL (Kinematics and Dynamics Library) solver integrated with MoveIt 2. The UR3’s kinematic structure (three intersecting wrist axes at joints 4–6) admits closed-form inverse kinematic solutions; however, the KDL numerical solver was selected for implementation simplicity and compatibility with the MoveIt 2 planning pipeline. The numerical solver uses the Newton-Raphson method with Jacobian pseudo-inverse to iter- atively converge on a joint conﬁguration that achieves the desired end-effector pose within a tolerance of 10^−^^5^^^m in position and 10^−^^4^^^rad in orientation. The Jacobian matrix* J*(*θ*)* ∈* R^6^^×^^6^^^relates joint velocities to end-effector velocities:    

$$˙*θ*1$$
  *(Equation 71)*

$$˙*θ*2$$
  *(Equation 72)*

$$˙*θ*3$$
  *(Equation 73)*

$$˙*θ*4$$
  *(Equation 74)*

$$˙*θ*5$$
  *(Equation 75)*

$$˙*θ*6$$
  *(Equation 76)*

˙*x* ˙*y* ˙*z*

$$*ω**x*$$
  *(Equation 77)*

$$*ω**y*$$
  *(Equation 78)*

$$*ω**z*$$
  *(Equation 79)*

    (3.20)

$$=*J*(*θ*)$$
  *(Equation 80)*

Singularities occur when det(*J*) = 0, indicating conﬁgurations where the arm loses one or more degrees of freedom in Cartesian space. For the UR3, singularities occur at: (1) shoulder singularity (wrist centre on the shoulder pan axis); (2) elbow singularity (arm fully extended or fully folded, where links 2 and 3 are collinear); and (3) wrist singularity (joints 4 and 6 axes aligned, causing rotational ambiguity). The motion planning algorithms (OMPL, PILZ) include singularity detection and avoidance, generating trajectories that maintain a minimum distance from singular conﬁgurations.

### Robotiq 2F-85 Gripper

The Robotiq 2F-85 is an adaptive parallel two-ﬁnger gripper with an 85 mm stroke, designed for collaborative robot applications. Its adaptive ﬁnger mechanism enables the gripper to conform to object geometry during grasping, distributing contact forces across a larger surface area and improving grasp stability. The gripper’s speciﬁcations include: 85 mm maximum stroke (ﬁnger opening), 235 N maximum grip force, 5 kg payload capacity, and 0.6 s open/close cycle time. In simulation, ﬁnger coupling is modelled via URDF mimic joints. Only finger_joint (range 0.0–0.8 rad) receives commanded positions; all other gripper joints follow through the mimic mechanism. The GripperActionController position range is 0.0 rad (fully open, 85 mm gap) to 0.8 rad (fully closed, 0 mm gap). The mimic joint mechanism mathematically couples the motion of multiple ﬁnger links to a single actuated joint:

$$*θ**mimic* =*m* ·*θ**master* +*b*$$
  *(Equation 81)*

(3.21) where* m* is the multiplier (typically 1.0 for parallel linkages or* −*1*.*0 for opposing linkages) and* b* is the offset. This coupling reduces the control dimensionality from multiple ﬁnger joints to a single scalar command, simplifying the planning and control interface. The selection of the Robotiq 2F-85 over alternative grippers was motivated by: (1) 85 mm stroke covering a wide range of object sizes typical in warehouse environments; (2) adaptive ﬁngers providing stable grasps without requiring precise object pose estimation; (3) compre- hensive ROS 2 driver support through the Robotiq ROS 2 package; (4) parallel jaw conﬁgu- ration enabling top-down and side grasps with simple grasp planning; and (5) force control capability enabling delicate object handling when deployed on physical hardware.

## Sensor Systems

### Line Following Sensor Array

The 8-channel infrared reﬂectance sensor array (Pololu QTR-8A equivalent) provides the pri- mary navigation feedback for line-following control. Each sensor channel consists of an in- frared LED and a phototransistor arranged in a reﬂective conﬁguration: the LED illuminates the ﬂoor surface, and the phototransistor measures the reﬂected infrared intensity. Dark sur- faces (black tape) absorb infrared radiation, producing low reﬂectance readings, while light surfaces (white/grey ﬂoor) reﬂect infrared radiation, producing high reﬂectance readings. Bi- nary thresholding converts the analog reﬂectance values to digital line/no-line decisions. The weighted average formula computes the lateral displacement error from the sensor array readings:

$$*e*(*t*) =$$
  *(Equation 82)*

$$∑8$$
  *(Equation 83)*

$$*i*=1 *w**i* ·*s**i*$$
  *(Equation 84)*

$$∑8$$
  *(Equation 85)*

$$*i*=1 *s**i*$$
  *(Equation 86)*

(3.22)

$$where*w**i* = [1.0, 0.71, 0.43, 0.14, −0.14, −0.43, −0.71, −1.0] and*s**i* ∈ {0, 1}. The weights$$
  *(Equation 87)*

are assigned based on the physical position of each sensor relative to the array centre, creating a signed error signal: positive when the line is to the right of centre, negative when to the left, and zero when the line is centred beneath the array. Binary thresholding:* s**i* = 1 if sensor distance to tape* d**i** ≤* 0*.*04 m, else* s**i* = 0. This threshold was empirically determined to provide robust detection across the range of ﬂoor-tape contrast ratios expected in indoor warehouse environments. The 40 mm detection radius per sensor, combined with the 9.5 mm inter-sensor spacing, ensures continuous line detection without gaps for tape widths of 19 mm or greater. Junction detection ﬁres when ^∑^^^*s**i** ≥* 5 for three consecutive frames at a minimum of 2.0 s since the last junction. This triple-frame requirement eliminates spurious junction detections from noise, sensor misalignment, or partial tape overlap at non-junction locations. The 2.0 s temporal ﬁlter prevents double-counting of a single junction (at 0.4 m/s cruise speed, the robot traverses 0.8 m in 2.0 s, far exceeding the physical extent of any junction region). Table 3.4: Sensor Speciﬁcations

### Sensor

### Model

### Interface

### Key Parameter

Line array QTR-8A SPI via MCP3008 8 channels, 9.5 mm spacing IMU BNO055 I2C 100 Hz, 9-axis,* ±*0*.*5^◦^ RFID RDM6300 UART 125 kHz, 5–10 cm range The sensor array’s physical mounting position is critical for control performance. The array is mounted at the front of the chassis, approximately 100 mm ahead of the drive axle. This for- ward mounting creates a “preview” effect: the sensor detects line position slightly ahead of the robot’s current position, providing advance warning of curves and enabling earlier corrective action. The preview distance (100 mm) combined with the vehicle speed (400 mm/s) provides a 250 ms preview time—sufﬁcient for the PD controller (with 150 ms settling time) to begin correction before the wheels reach the curve.

### IMU Sensor

The BNO055 9-axis Inertial Measurement Unit provides heading (yaw angle) feedback for the turn controller. The BNO055 integrates a 3-axis accelerometer, 3-axis gyroscope, and 3- axis magnetometer with an onboard ARM Cortex-M0 sensor fusion processor that outputs orientation as a quaternion at 100 Hz. The onboard fusion eliminates the need for external Kalman ﬁltering on the Raspberry Pi, reducing computational load on the primary processor. Yaw angle* ψ* is extracted from the IMU quaternion using the standard ZYX Euler angle decom- position:

$$ψ = arctan$$
  *(Equation 88)*

$$(2(*q**w**q**z* +*q**x**q**y*)$$
  *(Equation 89)*

) (3.23) 1* −* 2(*q*^2^

$$*y* +*q*2$$
  *(Equation 90)*

*z*^)^ The quaternion representation avoids gimbal lock (a singularity in Euler angle representation at* ±*90^◦^^^pitch), although for the ATLAS application (motion conﬁned to a horizontal plane with near-zero pitch and roll), gimbal lock is not a practical concern. The BNO055’s absolute heading accuracy of* ±*0*.*5^◦^^^(after calibration) exceeds the turn controller’s termination tolerance of* ±*3^◦^^^by a factor of 6, ensuring that heading measurement error does not limit turn precision. The IMU is mounted at the geometric centre of the chassis to minimise the effect of translational vibrations on angular measurements. Any offset between the IMU position and the robot’s centre of rotation would introduce spurious angular readings during translational motion due to centripetal acceleration. Central mounting eliminates this error source, ensuring that the IMU reports only true rotational motion.

### RFID Detection with Hysteresis

The system employs 21 RFID tags: 1 home tag at (0*,* 0) and 20 shelf tags at (*x**s**, y**a*) where* x**s** ∈* *{*1*,* 2*,* 3*,* 4*}* m and* y**a** ∈ {*2*,* 4*,* 6*,* 8*,* 10*}* m. The hysteresis detection model prevents oscillatory tag readings at detection boundaries:

### Detection condition (transition from undetected to detected):

detect:

$$√$$
  *(Equation 91)*

$$(*x* −*x**tag*)2 + (*y* −*y**tag*)2 ≤ 0.5 m AND armed$$
  *(Equation 92)*

(3.24)

### Rearm condition (enable future detection after moving away):

rearm:

$$√$$
  *(Equation 93)*

$$(*x* −*x**tag*)2 + (*y* −*y**tag*)2 > 0.8 m$$
  *(Equation 94)*

(3.25) The 0.3 m hysteresis gap prevents oscillatory detection at the boundary. Without hysteresis, a robot hovering near the 0.5 m detection radius would alternate between “detected” and “not detected” states as minor position ﬂuctuations (vibration, encoder noise) move the estimated distance above and below the threshold. The hysteresis mechanism requires the robot to move deﬁnitively beyond the outer radius (0.8 m) before the system can detect the same tag again, ensuring that each physical pass over a tag produces exactly one detection event. The tag placement strategy ensures unique identiﬁcation of every shelf position in the ware- house. Each tag stores a unique 10-character hexadecimal ID that maps to a speciﬁc shelf location through a lookup table maintained in the mission manager’s conﬁguration. The tag-to- shelf mapping is loaded at system startup and can be updated without code changes, supporting warehouse reconﬁguration without software modiﬁcation.

## Control Systems Design

### Line Following PD Controller

A Proportional-Derivative controller is employed for line tracking. The discrete PD control law at 50 Hz is:

$$*u**k* =*K**p* ·*e**k* +*K**d* · (*e**k* −*e**k*−1) ·*f**s*$$
  *(Equation 95)*

(3.26) with* K**p* = 0*.*6,* K**d* = 0*.*2, and* f**s* = 50 Hz. The output* u* becomes angular.z in the ROS 2 Twist command, directly commanding the robot’s yaw rate. The linear velocity (linear.x) remains constant at 0.4 m/s during line following, creating a coupled linear-angular motion that traces the line path. The proportional gain* K**p* = 0*.*6 was determined empirically through the following procedure: starting with a low value (0.1) and incrementing by 0.1 until oscillation was observed (at* K**p* = 1*.*2), then reducing to half the oscillation gain (0.6) following a simpliﬁed Ziegler-Nichols approach. The derivative gain* K**d* = 0*.*2 was then added to damp the remaining oscillation, with its value selected to achieve critical damping (no overshoot) during step disturbances. The integral term is set to zero (*K**I* = 0) to eliminate windup at junction transitions where the setpoint changes rapidly and no steady-state error exists due to direct position measurement. The physical justiﬁcation for omitting integral action is twofold: (1) the line sensor provides a direct measurement of lateral position error (not velocity or acceleration), so there is no inherent integrator in the plant that would produce steady-state error under proportional-only control; and (2) at junction transitions, the sensor pattern changes from a single narrow line (2–3 sensors active) to a wide junction region (5–8 sensors active), causing the error signal to change abruptly. An integral term would “remember” the pre-junction error and produce inappropriate corrective action as the robot enters the junction.

### Stability Analysis:

The closed-loop characteristic equation, derived from the transfer function of the PD-controlled line-following system, is:

$$*s*2 + 6*s* + 3 = 0$$
  *(Equation 96)*

(3.27) with roots* s* =* −*0*.*55 and* s* =* −*5*.*45 (both real and negative: stable, overdamped). The fact that both poles are real and negative conﬁrms: (1) asymptotic stability—all transients decay to zero; (2) overdamped response—no oscillation, the error decays monotonically after a disturbance; and (3) the dominant pole at* s* =* −*0*.*55 determines the system’s time constant (*τ* = 1*/*0*.*55 = 1*.*82 s in continuous time, corresponding to 91 samples at 50 Hz). Natural frequency:* ω**n* =

$$√$$
  *(Equation 97)*

$$3 = 1.73 rad/s. Damping ratio:*ζ* = 6/(2*ω**n*) = 6/(2×1.73) = 1.73$$
  *(Equation 98)*

(overdamped,* ζ >* 1). The 2% settling time is* t**s** ≈* 4*/*0*.*55 = 7*.*3 samples = 0.15 s. This settling time means that after a sudden lateral disturbance (e.g., a wheel encountering a ﬂoor irregularity), the controller returns the robot to within 2% of the line centre in 0.15 s—during which the robot travels only 0*.*15* ×* 0*.*4 = 0*.*06 m = 60 mm. This rapid recovery ensures that the robot remains well within the sensor array’s detection window (*±*33 mm) even under signiﬁcant disturbances. The Bode analysis of the open-loop transfer function reveals a gain margin of 12 dB and a phase margin of 62^◦^, both well within acceptable stability margins (gain margin* >* 6 dB, phase margin *>* 30^◦^^^are industry standards). These margins indicate robustness to parameter variations: the controller remains stable even if the effective gains change by up to a factor of 4 (due to ﬂoor surface changes, wheel slip, or battery voltage variations affecting motor response).

### Turn Controller

A bang-bang controller with IMU feedback executes 90^◦^^^and 180^◦^^^turns. A constant angular velocity is applied until the target heading is reached:

$$*ω**cmd* = sign(∆*θ*) × 0.4 rad/s$$
  *(Equation 99)*

(3.28) The stop condition is* |**θ**target** −** θ**current**|** <* 3^◦^. Theoretical turn durations: 90^◦^^^turn:* t* =

$$π/(2 × 0.4) = 3.93 s; 180◦^^turn:*t* = π/0.4 = 7.85 s.$$
  *(Equation 100)*

The bang-bang control strategy was selected over smoother alternatives (trapezoidal velocity proﬁle, S-curve proﬁle) for the following engineering reasons: (1) simplicity—the controller requires only a constant velocity command and a heading comparison, with no trajectory gen- eration computation; (2) time-optimality—bang-bang control is the time-optimal solution for minimum-time rotation under the constraint of bounded angular velocity; (3) adequacy—the *±*3^◦^^^accuracy achieved is sufﬁcient for the subsequent line-following phase to recapture the line after the turn; and (4) IMU accuracy margin—the BNO055’s* ±*0*.*5^◦^^^heading accuracy provides 6*×* margin below the 3^◦^^^termination tolerance, ensuring reliable turn completion. The potential limitation of bang-bang control is heading overshoot due to system inertia: the robot continues rotating brieﬂy after the stop command due to angular momentum. With* I**zz* = 0*.*03177 kg*·*m^2^^^and* ω* = 0*.*4 rad/s, the angular momentum at the stop instant is* L* =* I**zz** ×** ω* = 0*.*0127 kg*·*m^2^/s. The braking torque from wheel friction (*µ* = 0*.*5,* F**N* =* mg/*2 = 12*.*26 N

$$per wheel, moment arm L/2 = 0.15 m) provides*τ**brake* = 2 × µF*N* × L/2 = 1.84 N·m. The$$
  *(Equation 101)*

$$deceleration time is*t**dec* =*L**angular*/τ*brake* = 0.0127/1.84 = 6.9 ms, during which the heading$$
  *(Equation 102)*

$$changes by*∆**θ**overshoot* =*ω* ×*t**dec*/2 = 0.4 × 0.0069/2 = 0.0014 rad = 0.08◦. This overshoot$$
  *(Equation 103)*

is negligible compared to the 3^◦^^^tolerance, conﬁrming that bang-bang control is appropriate for this system’s inertia characteristics.

### Motor Velocity PID

Each drive motor requires a velocity PID controller to maintain the commanded wheel speed despite load disturbances (ﬂoor friction changes, slopes, payload variations). The initial PID parameters are:* K**p* = 2*.*0,* K**i* = 1*.*0,* K**d* = 0*.*05. Tuning targets: steady-state error* <* 2%; step-response overshoot* <* 10%. The motor velocity PID operates at a higher rate (100 Hz) than the navigation PD controller (50 Hz), creating a nested control architecture where the outer loop (navigation) generates velocity setpoints and the inner loop (motor PID) tracks those setpoints. This cascade structure provides disturbance rejection at the motor level without requiring the navigation controller to model motor dynamics—a classical separation-of-concerns approach in control engineering. The integral term is included in the motor PID (unlike the navigation PD) because the motor plant does have an inherent integrator-like behaviour when loaded: friction and back-EMF cre- ate steady-state speed errors under constant voltage drive that only integral action can eliminate. The low derivative gain (*K**d* = 0*.*05) provides minimal high-frequency noise ampliﬁcation while damping speed oscillations during load transients.

## Mission State Machine

The complete mission lifecycle is governed by a 12-state Finite State Machine (FSM), as sum- marised in Table 3.5. The FSM paradigm was selected over alternative mission management approaches (behaviour trees, hierarchical task networks, scripted sequences) for the following reasons: (1) complete state visibility—at any instant, the system is in exactly one well-deﬁned state with known properties; (2) deterministic transitions—given the current state and an event, the next state is uniquely determined; (3) exhaustive coverage—all possible state-event com- binations are explicitly handled, either with a transition or with an explicit “stay in current state” decision; (4) formal veriﬁability—the state machine can be analysed for completeness, reachability, and deadlock-freedom using standard formal methods; and (5) implementation simplicity—a state machine maps directly to a switch-case structure or state pattern in code. Table 3.5: Mission State Machine Summary

### State

### Purpose

### Velocity Source

IDLE Waiting for queued mission Zero NAV_SPINE Following spine north Line follower TURNING 90^◦^^^turn into aisle Turn controller NAV_AISLE Following aisle east Line follower AT_SHELF Arrived at target shelf Zero PICKUP Simulated pick (2 s) Zero PIVOT 180^◦^^^turn Turn controller RET_AISLE Following aisle west Line follower RET_TURN 90^◦^^^turn onto spine Turn controller RET_SPINE Following spine south Line follower DOCKED Arrived at home dock Zero *→* Docking FSM ERROR E-Stop active Zero The Velocity Arbiter ensures that only one velocity source reaches the wheels at any time, implementing a priority-based multiplexer that prevents conﬂicting velocity commands:         nav_vel if state* ∈* navigation states turn_vel if state* ∈* turning states output = (3.29)        dock_velocity if state* ∈* docking states if E-stopped (safety override) The velocity arbiter is architecturally critical: it is the* sole* publisher on the /cmd_vel topic, ensuring that no node can bypass the safety logic to directly command wheel motion. All ve- locity sources (line follower, turn controller, docking controller) publish to intermediate topics (/nav_vel, /turn_vel, /dock_vel), and the arbiter selects among them based on the current FSM state. The E-Stop override has highest priority and cannot be overridden by any other velocity source—a fundamental safety property that ensures the robot stops immediately regardless of the mission state or controller output. The mission execution workﬂow proceeds as follows: (1) the operator dispatches a mission via the GUI or CLI, specifying a target shelf ID; (2) the mission manager enqueues the mis- sion and transitions from IDLE to NAV_SPINE; (3) the AGV follows the spine line northward, counting junctions until the target aisle index is reached; (4) at the target junction, the FSM transitions to TURNING and commands a 90^◦^^^turn into the aisle; (5) upon turn completion, the FSM transitions to NAV_AISLE and the AGV follows the aisle line eastward; (6) the RFID reader detects the target shelf tag, conﬁrming arrival, and the FSM transitions to AT_SHELF; (7) after a 0.5 s settling period, the FSM transitions to PICKUP, signalling the arm subsystem to begin the pick-and-place operation; (8) upon pick completion (signalled by the arm subsys- tem), the FSM transitions to PIVOT and commands a 180^◦^^^turn; (9) the AGV follows the aisle westward (RET_AISLE), turns south onto the spine (RET_TURN), follows the spine south- ward (RET_SPINE), and arrives at the home dock; (10) the docking sub-FSM performs ﬁne alignment veriﬁcation before transitioning to IDLE for the next mission. This sequential workﬂow represents the simplest correct mission execution strategy. More com- plex strategies (shortest-path planning, multi-shelf batching, return-trip picking) are identiﬁed as future extensions but are excluded from the initial implementation to maintain architectural clarity and enable thorough validation of the core mission cycle.

## UR3 Arm Motion Planning

### MoveIt 2 Architecture

The arm motion planning follows a ﬁve-layer architecture that separates concerns and enables independent testing of each layer. MoveIt 2 is the de facto standard motion planning framework for ROS 2, providing a uniﬁed interface to multiple planning algorithms, collision checking engines, and kinematic solvers. The URDF/SRDF pair deﬁnes the robot geometry and semantic collision groupings. The URDF (Uniﬁed Robot Description Format) speciﬁes the kinematic chain (links, joints, parent- child relationships), visual geometry (mesh ﬁles for rendering), collision geometry (simpliﬁed shapes for fast collision checking), and dynamic parameters (mass, inertia, joint limits). The SRDF (Semantic Robot Description Format) augments the URDF with planning-speciﬁc in- formation: named joint conﬁgurations (“home”, “ready”, “extended”), planning groups (“ma- nipulator”, “gripper”), allowed collision pairs (links that are always or never in collision), and virtual joints (connecting the robot to the world frame). The ros2_control layer provides real-time position command interfaces at 500 Hz, matching the native control rate of the UR3 hardware. The JointTrajectoryController receives time-stamped joint trajectory waypoints from MoveIt 2 and interpolates between them at the control rate, ensuring smooth motion even when planning produces coarse waypoint spacing. The Grip- perActionController provides a simple position command interface for the gripper, abstracting away the mimic joint coupling that is handled internally. MoveIt 2’s move_group node is the central coordination point, managing: the planning scene (geometric representation of the robot and its environment for collision checking), kinematic solver plugins (KDL for inverse kinematics), planner plugins (OMPL, PILZ), and trajectory processing (time parameterisation, smoothing). The planning scene is maintained as a collision world containing the robot’s own geometry (self-collision checking) and environmental objects (tables, shelves, walls) represented as primitive shapes or mesh objects. The MoveIt Task Constructor (MTC) provides compositional multi-stage task speciﬁcation, enabling complex manipulation tasks to be expressed as ordered sequences of primitive stages. Each stage deﬁnes a local planning problem (move to a pose, generate a grasp, modify the planning scene), and the MTC framework manages the global constraint that the end state of each stage must be kinematically compatible with the start state of the next stage. This inter- stage constraint propagation enables the planner to backtrack when a locally feasible stage solution is globally infeasible (e.g., a grasp that can be reached but cannot be lifted from due to joint limits). The application layer dispatches planning requests to the MTC pipeline, specifying task-level goals (“pick object X from location A and place at location B”) that are decomposed into the appropriate stage sequences by the MTC task deﬁnition.

### Pick-and-Place Workﬂow

The complete pick-and-place operation is structured in ﬁve phases, comprising nine MTC stages that are planned as a single integrated task:

### Phase 1 — Initialisation: Gazebo spawning, controller activation, MoveIt 2 start, planning

scene population with collision geometry (table surface, shelf geometry, target object), and arm moved to home conﬁguration. The initialisation phase ensures that all planning infrastructure is operational and that the planning scene accurately represents the physical environment before any motion is commanded. **Phase 2 — Pre-Grasp:** Gripper commanded to OPEN state (0.0 rad); arm planned via OMPL to pre-grasp approach pose (100 mm above target object). The OMPL RRTConnect planner is used for this free-space motion because the start conﬁguration (home) and goal conﬁguration (pre-grasp) may be widely separated in joint space with obstacles (table edges, shelf walls) between them. RRTConnect’s bidirectional tree-growing strategy efﬁciently navigates these complex environments by simultaneously exploring from both endpoints and connecting when the trees meet. **Phase 3 — Grasp:** Arm descends linearly to grasp pose using PILZ LIN Cartesian planner (10 mm step size); gripper commanded to CLOSED state (0.8 rad); object attached to gripper in planning scene. The PILZ LIN planner is used for this approach motion because a straight- line descent is required to engage the gripper with the object from directly above, avoiding lateral forces that could displace the object or cause an unstable grasp. The 10 mm step size ensures smooth Cartesian interpolation with inverse kinematics solved at each step to detect reachability issues early in the descent. After grasp closure, the object is “attached” to the gripper link in the MoveIt planning scene, meaning that subsequent collision checks treat the object as part of the robot geometry rather than an environmental obstacle—enabling the robot to move with the object without detecting false collisions. **Phase 4 — Lift and Transfer:** Arm linearly lifted above table (PILZ LIN, maintaining vertical motion to avoid contact with adjacent objects); arm planned to pre-place pose via OMPL (free- space motion to the placement location); arm descends to place pose via PILZ LIN (straight-line descent to the placement surface). **Phase 5 — Place and Retreat:** Gripper commanded to OPEN state (0.0 rad, releasing the object); object detached from planning scene (returned to environmental object status); arm retreats vertically via PILZ LIN (straight-line withdrawal to avoid disturbing the placed object); arm returns to home conﬁguration via OMPL (free-space motion back to the rest position). The nine MTC stages map to this ﬁve-phase workﬂow as: CurrentState* →* OpenGripper* →* Approach (OMPL)* →* Descend (PILZ LIN)* →* CloseGripper* →* Lift (PILZ LIN)* →* Move (OMPL)* →* Place (PILZ LIN)* →* OpenGripper* →* Retreat (PILZ LIN)* →* Home (OMPL).

### Planner Selection Rationale

OMPL RRTConnect is used for free-space arm motions based on the following engineering justiﬁcation: (1) probabilistic completeness—given sufﬁcient planning time, RRTConnect is guaranteed to ﬁnd a solution if one exists in the conﬁguration space; (2) empirical speed—for 6-DOF robot arms in cluttered environments, RRTConnect consistently produces solutions in under 2 s, outperforming alternative planners (PRM, RRT*, EST) for single-query problems; (3) bidirectional growth—by growing trees from both start and goal, RRTConnect exploits the structure of manipulation problems where both endpoints are typically in relatively open regions of conﬁguration space; and (4) anytime availability—RRTConnect returns the ﬁrst fea- sible solution found, enabling the system to begin execution quickly even if a shorter path might exist. PILZ LIN is used for Cartesian approach and retreat phases based on: (1) determinism— PILZ generates identical trajectories for identical requests, enabling repeatability testing and industrial certiﬁcation; (2) guaranteed straight-line motion—the end-effector traces a geomet- rically exact straight line in Cartesian space, essential for approach and retreat where lateral motion could contact objects; (3) predictable timing—PILZ trajectories have analytically com- puted durations based on the speciﬁed velocity and acceleration limits, enabling precise mis- sion timing predictions; and (4) industrial provenance—PILZ motion planning originates from PILZ GmbH’s safety-certiﬁed industrial controllers, providing conﬁdence in its correctness for safety-critical applications. This dual-planner strategy provides optimal coverage of both free-space and Cartesian motion requirements within a single uniﬁed pipeline. Alternative approaches considered include: (a) using OMPL for all motions (loses Cartesian straight-line guarantee); (b) using PILZ for all motions (fails for complex free-space motions with obstacles); (c) using Descartes planner for Cartesian segments (heavier computational requirements, less mature ROS 2 support). The OMPL + PILZ combination was selected as the best engineering trade-off between capability, reliability, and computational efﬁciency.

## Hardware Architecture

### Computing Platform

The Raspberry Pi 4 (4 GB) is selected as the AGV on-board computer. The selection rationale is that no GPU, SLAM, or computer vision processing is required for AGV navigation—the computational requirements are limited to: (1) sensor data processing at 50–100 Hz (integer arithmetic on 8-channel sensor array, quaternion extraction from IMU); (2) PD control law evaluation at 50 Hz (two multiply-add operations per cycle); (3) FSM state logic (conditional branching, negligible computation); and (4) ROS 2 node management (DDS communication overhead). These requirements are comfortably met by the Pi 4’s quad-core ARM Cortex-A72 processor. The arm planning runs on an operator PC with greater computational resources (multi-core x86, 16+ GB RAM) because MoveIt 2 motion planning and the OMPL sampling algorithms are computationally intensive, particularly for complex environments with many collision ob- jects. The operator PC communicates with the AGV via WiFi-based DDS discovery, enabling seamless topic-level communication without explicit network conﬁguration. Table 3.6: AGV Computing Platform Comparison

### Platform

### CPU

### RAM

### ROS 2

### Price

Raspberry Pi 4 (4 GB)

$$4 × 1.5 GHz$$
  *(Equation 104)*

4 GB Yes $55 Raspberry Pi 5 (8 GB)

$$4 × 2.4 GHz$$
  *(Equation 105)*

8 GB Yes $80 Jetson Nano

$$×$$
  *(Equation 106)*

1*.*4 GHz+GPU 4 GB Yes $150 The Raspberry Pi 4 was selected over the Pi 5 (available but higher cost without proportional capability beneﬁt) and the Jetson Nano (GPU unnecessary, 3*×* cost premium for unused GPU capability). The 4 GB RAM variant provides sufﬁcient memory for ROS 2 Humble, the Ubuntu 22.04 base system, and all navigation nodes running simultaneously (typical memory usage: 1.2 GB system, 0.8 GB ROS 2 nodes, leaving 2 GB headroom for peak allocations and logging).

### Power System

The AGV power budget is shown in Table 3.7. A 3S LiPo battery (11.1 V nominal, 5000 mAh capacity) provides the primary power supply with approximately 1.5 hours of continuous run- time at average load. Table 3.7: AGV Power Budget

### Component

### Voltage

### Current

### Power

2*×* DC motors (loaded) 12 V 1.0 A each 24 W Raspberry Pi 4 5 V 2.5 A 12.5 W All sensors 3.3 V 0.2 A 0.66 W Motor controller (logic) 5 V 0.05 A 0.25 W

### Total

*≈***37 W** The runtime calculation: battery energy = 11*.*1 V* ×* 5*.*0 Ah = 55*.*5 Wh. At average power consumption of 37 W, runtime = 55*.*5*/*37 = 1*.*50 hours. The average power ﬁgure assumes continuous motor operation at 50% of stall load; actual runtime during warehouse operation (with frequent stops during picking and docking) is expected to exceed 2 hours. A safety margin of 20% is applied, yielding a guaranteed minimum runtime of 1.2 hours—sufﬁcient for approximately 96 pick missions at 45 s per cycle. The power distribution architecture uses a Buck converter (12 V to 5 V, 3 A output) to supply the Raspberry Pi and logic-level components from the LiPo battery. The motors are driven directly from the battery voltage through the L298N H-bridge motor driver, which provides PWM speed control. A battery management system (BMS) provides over-discharge protection (cutoff at 9.0 V = 3.0 V per cell), over-current protection, and cell balancing during charging.

### Simulation-to-Physical Component Conversion

The complete conversion from simulation to physical hardware is documented in Table 3.8. This table serves as the engineering speciﬁcation for physical deployment, identifying every simulation abstraction that requires hardware replacement. Table 3.8: Simulation-to-Physical Component Conversion

### Simulation

### Real Hardware

### Cost

### Integration

Gazebo diff_drive L298N + DC motors + encoders $50 GPIO PWM URDF geometry Aluminium frame $60 Fabrication Virtual line sensor QTR-8A + MCP3008 $20 SPI Virtual IMU BNO055 $30 I2C Virtual RFID RDM6300 + 25 tags $30 UART ROS 2 on x86 ROS 2 on RPi 4 $70 microSD Simulated power 3S LiPo + BMS + Buck $50 Wiring Gazebo ﬂoor Real ﬂoor + black tape $20 Installation GUI (PyQt5) Same GUI on operator PC $0 WiFi DDS

### Total

*≈***$330** The $330 hardware cost represents only the AGV subsystem conversion. The complete production- quality build including the UR3 arm (or equivalent 6-DOF manipulator), Robotiq gripper, and operator PC brings the total to approximately $1,250. The minimum viable build (using a lower-cost 6-DOF arm such as the MyCobot 280 at $500) reduces the total to approximately $800. The conversion methodology is systematic: for each simulation component, the corresponding hardware module is identiﬁed, the ROS 2 topic interface is veriﬁed to be identical (same mes- sage type, same topic name, same QoS), and a hardware driver node is written that publishes/- subscribes using the same interface. The control logic nodes (line_follower.py, turn_controller.py, mission_node.py) require zero modiﬁcation because they communicate exclusively through ab- stract topic interfaces.

### GPIO Pin Mapping

The Raspberry Pi 4 GPIO pin assignment is given in Table 3.9. The pin allocation was de- termined by the interface requirements of each peripheral and the Pi 4’s hardware peripheral mapping (dedicated SPI, I2C, UART, and PWM pins). Table 3.9: Raspberry Pi 4 GPIO Pin Mapping

### GPIO

### Function

### Direction

### Notes

I2C SDA (IMU) Bidirectional 3.3 V level I2C SCL (IMU) Output 3.3 V level Encoder L-A Input Interrupt Encoder L-B Input Interrupt SPI CE0 (ADC) Output Active low SPI MISO Input — SPI MOSI Output — SPI CLK Output 1.35 MHz PWM0 (left motor) Output 10 kHz PWM1 (right motor) Output 10 kHz UART TX (RFID) Output 9600 baud UART RX (RFID) Input 9600 baud Motor L-IN1 Output Direction Motor R-IN3 Output Direction Motor R-IN4 Output Direction Encoder R-A Input Interrupt Encoder R-B Input Interrupt Motor L-IN2 Output Direction The GPIO allocation follows hardware peripheral constraints: GPIO 2/3 are the dedicated I2C1 pins (hardware I2C with clock stretching support required by BNO055); GPIO 8–11 are the SPI0 hardware pins (required for the 1.35 MHz clock rate to the MCP3008 ADC); GPIO 12/13 are the hardware PWM0/PWM1 pins (providing precise 10 kHz PWM without CPU overhead); and GPIO 14/15 are the dedicated UART pins (hardware UART for reliable 9600 baud RFID communication). The remaining GPIOs (5, 6, 17, 22, 23, 24, 25, 27) are general-purpose pins used for encoder interrupt inputs and motor direction control.

## Software Deployment Architecture

### Files Unchanged for Physical Deployment

The following ﬁles run without modiﬁcation on physical hardware, representing 70% of the total codebase: • line_follower.py — reads weighted error from /atlas/line_error topic, computes PD output, publishes angular velocity to /nav_vel. This node is hardware- agnostic because it operates entirely on abstract error values, never directly accessing sensor hardware. • turn_controller.py — reads heading from /atlas/imu/yaw topic, computes turn commands, publishes to /turn_vel. The IMU topic provides heading regardless of whether the source is a simulated or physical sensor. • mission_node.py — pure state machine logic operating on topic-level events. No hardware access of any kind; all state transitions are triggered by incoming messages. • send_mission.py — CLI publisher, sends mission commands to /atlas/mission_command. Pure application-layer logic. • atlas_control_center.py — PyQt5 GUI on operator PC. Communicates with the robot exclusively through ROS 2 topics over WiFi/DDS. The GUI runs on the opera- tor’s machine, not on the robot, so it is inherently hardware-independent. This 70% code reuse ratio validates the hardware abstraction architecture: by consistently sep- arating control logic from hardware access, the majority of the codebase becomes platform- independent and transferable between simulation and physical deployment without modiﬁca- tion.

### New Hardware Interface Nodes

Four simulation plugins are replaced by hardware driver nodes for physical deployment, as shown in Table 3.10. Each replacement node provides an identical ROS 2 topic interface to its simulation counterpart, ensuring that upstream nodes (line follower, turn controller, mission node) cannot distinguish between simulated and physical sensor data. Table 3.10: Code Changes for Physical Deployment

### Simulation File

### Replaced By

### New File

libgazebo_ros_diff_drive.so motor_driver node atlas_hardware/motor_driver.py line_sensor.py (geo- metric) SPI ADC reader atlas_hardware/line_sensor_hw.py tag_detector.py (dis- tance) UART RFID reader atlas_hardware/rﬁd_reader_hw.py libgazebo_ros_imu_sensor.so I2C BNO055 driver atlas_hardware/imu_hw.py atlas_full.launch.py No-Gazebo launch atlas_real.launch.py The hardware driver nodes follow a common architectural pattern: (1) initialise the hardware peripheral (open SPI/I2C/UART connection, conﬁgure registers); (2) enter a timed callback loop at the appropriate rate (50 Hz for line sensor, 100 Hz for IMU, continuous for RFID); (3) read raw data from hardware; (4) convert to ROS 2 message format; (5) publish on the standard topic with the standard message type. This pattern ensures that each driver node is self-contained, independently testable, and trivially replaceable if the hardware component changes.

## GUI and Human-Machine Interface

The PyQt5 control centre uses a dual-threaded architecture to prevent the blocking nature of rclpy.spin() from freezing the GUI event loop. The main Qt thread handles event loop, widget rendering, and user interaction; a background thread runs rclpy.spin() and sub- scription callbacks. Thread-safe communication between ROS callbacks (background thread) and Qt widgets (main thread) uses pyqtSignal objects, which are Qt’s thread-safe inter- thread communication mechanism. GUI panels include: •** Mission Creation:** Shelf selection dropdown, mission dispatch button, batch mission entry for sequential multi-shelf operations. •** Mission Control:** Pause/Resume/Cancel buttons with conﬁrmation dialogs to prevent accidental mission termination. •** Emergency Controls:** E-Stop button (immediately halts all motion), Reset AGV (clears error state and returns to IDLE), Reset to Dock (teleports robot to home in simulation, initiates return-to-dock in physical deployment). •** Robot Status:** 11 live telemetry ﬁelds updated at 10 Hz including: current state, linear velocity, angular velocity, heading, position (x, y), junction count, target shelf, battery voltage (physical only), motor currents (physical only). •** Docking Status:** Lateral offset, heading error, alignment veriﬁcation result, dock conﬁ- dence indicator. •** Mission Queue:** Visual display of pending missions with drag-and-drop reordering ca- pability. •** Event Log:** Timestamped log of all state transitions, mission events, and error conditions for post-mission analysis. The 10 Hz GUI update rate was selected as a balance between operator situational awareness (human perception of continuous motion requires* >* 5 Hz update) and computational overhead (each update requires widget repaint operations that consume CPU cycles on the operator PC). The pyqtSignal mechanism ensures that ROS 2 callback data (arriving at up to 50 Hz from the line sensor) is decimated to the GUI update rate without data loss—only the most recent value is displayed, while the full-rate data remains available for logging and analysis.

# Results and Discussions with Conclusions

## System Performance Metrics

The integrated system was validated through complete mission execution in the Gazebo simula- tion environment. Validation testing comprised 50 consecutive mission cycles to different shelf locations, executed over a 40-minute continuous operation period without human intervention. Key performance metrics are presented in Table 4.1. Table 4.1: System Performance Metrics

### Metric

### Measured Value

### Requirement

### Status

Line following speed 0.4 m/s *≥* 0*.*3 m/s PASS Turn accuracy *±*3^◦^ *±*5^◦^ PASS RFID detection rate 100% *≥* 95% PASS Mission completion 100% *≥* 98% PASS Docking alignment *±*3 cm,* ±*3^◦^ *±*5 cm,* ±*5^◦^ PASS E-Stop response time *<* 20 ms *<* 100 ms PASS Mission cycle (S05) *≈* 45 s *<* 60 s PASS GUI update rate 10 Hz *≥* 5 Hz PASS Line tracking accuracy *<* 5 mm *<* 10 mm PASS **Line Following Speed (0.4 m/s):** The cruise velocity of 0.4 m/s was achieved consistently across all straight-path segments during validation testing. This speed represents the design point where the PD controller maintains sub-5 mm tracking accuracy—higher speeds (tested up to 0.6 m/s) degraded tracking accuracy beyond the 5 mm threshold due to increased control latency effects. The 0.4 m/s speed exceeds the 0.3 m/s minimum requirement by 33%, provid- ing operational headroom for future optimisation. In comparison, commercial line-following AGVs (Daifuku, Dematic) typically operate at 1.0–2.0 m/s; the ATLAS speed is lower pri- marily due to the shorter sensor preview distance (100 mm vs. 300–500 mm in commercial systems) and the 50 Hz control rate (vs. 200–500 Hz in industrial AGV controllers). **Turn Accuracy (***±*3^◦^**):** The measured turn accuracy of* ±*3^◦^^^represents the maximum devia- tion from the target heading observed across all 90^◦^^^and 180^◦^^^turns during validation. This performance exceeds the* ±*5^◦^^^requirement by 40%. The accuracy is limited by the interaction between the bang-bang controller’s stop condition and the 50 Hz sampling rate: at 0.4 rad/s an- gular velocity and 50 Hz sampling, the heading can change by up to 0*.*4*/*50 = 0*.*008 rad = 0*.*46^◦^ between consecutive checks of the stop condition. The observed 3^◦^^^error suggests additional contributing factors including IMU measurement noise (*±*0*.*5^◦^) and the 20 ms communication latency between the turn controller node and the velocity arbiter. **RFID Detection Rate (100%):** All 21 RFID tags were successfully detected during every pass throughout the validation campaign. This 100% detection rate validates the hysteresis detection model and the tag placement strategy. The 0.5 m detection radius provides comfortable margin for the robot’s maximum lateral deviation from the line centre (*<* 5 mm), ensuring that the tag is always well within detection range when the robot passes over it. In physical deployment, the detection rate may decrease slightly due to tag orientation sensitivity, ﬂoor material RF ab- sorption, and reader antenna alignment; however, the 0.5 m radius provides substantial margin against these degradation mechanisms.

### Mission Completion Rate (100%): All 50 validation missions completed successfully with-

out human intervention, E-Stop activation, or error state entry. This 100% completion rate against a 98% requirement demonstrates the robustness of the FSM logic, the reliability of the sensor systems (in simulation), and the correctness of the state transition logic. The FSM was subjected to additional stress testing including rapid sequential mission dispatch (queuing 10 missions within 1 s), mid-mission pause/resume cycles, and E-Stop/reset during various states—all scenarios completed without error. **E-Stop Response Time (***<* 20** ms):** The measured E-Stop response time (from command pub- lication to zero velocity command output) was consistently below 20 ms, exceeding the 100 ms requirement by a factor of 5. This rapid response is achieved by the velocity arbiter’s direct E-Stop ﬂag polling at every control cycle (20 ms period at 50 Hz), combined with ROS 2’s reliable QoS for E-Stop messages. The* <* 20 ms response time satisﬁes the requirements of ISO 3691-4 for AGV emergency stop performance.

### Line Tracking Accuracy (< 5 mm): The root-mean-square (RMS) lateral tracking error

during straight-path following was measured at 3.2 mm, with peak excursions not exceed- ing 4.8 mm. This sub-5 mm accuracy validates the PD controller design and the sensor array resolution. The primary error contributors are: (1) sensor quantisation (9.5 mm spacing lim- its position estimation resolution to approximately* ±*2 mm); (2) control latency (one sample period of 20 ms at 0.4 m/s corresponds to 8 mm of forward travel, during which the error is uncompensated); and (3) wheel speed asymmetries (minor differences in motor characteristics creating systematic drift that the controller must continuously correct).

## Mission Timing Analysis

The complete timing for a representative mission (Shelf S05, aisle 5, rack 1) is presented in Table 4.2. This timing characterisation enables mission scheduling, throughput estimation, and battery life prediction for ﬂeet planning purposes. Table 4.2: Mission Timing Analysis (Shelf S05)

### Phase

### Distance/Angle

### Speed

### Time

NAV_SPINE 4.0 m 0.4 m/s 10.0 s TURNING *π/*2 rad 0.4 rad/s 3.9 s NAV_AISLE 1.0 m 0.4 m/s 2.5 s AT_SHELF — — 0.5 s PICKUP — — 2.0 s PIVOT *π* rad 0.4 rad/s 7.9 s RET_AISLE 1.0 m 0.4 m/s 2.5 s RET_TURN *π/*2 rad 0.4 rad/s 3.9 s RET_SPINE 4.0 m 0.4 m/s 10.0 s DOCKED — — 1.0 s

### Total

*≈***44.2 s** The timing analysis reveals that navigation phases (NAV_SPINE forward + RET_SPINE re- turn = 20 s) dominate the mission duration at 45% of total time, followed by turning phases (TURNING + PIVOT + RET_TURN = 15.7 s at 36%), with manipulation and overhead con- suming only 19% (8.5 s). This distribution suggests that speed improvement would have the greatest throughput impact if applied to the spine navigation phase—increasing spine speed from 0.4 m/s to 0.6 m/s would reduce total mission time by 6.7 s (15% reduction). The timing model also enables mission time prediction for any shelf location: for shelf at aisle index* n* (distance 2*n* m along spine) and rack index* k* (distance* k* m along aisle), the predicted mission time is:

$$*T**mission* = 2 × 2*n*$$
  *(Equation 107)*

0*.*4

$$+ 2*k*$$
  *(Equation 108)*

$$0.4 + 3π/2$$
  *(Equation 109)*

0*.*4 ^+ 4^^.^^0^^s (overhead)^ (4.1) This formula enables the mission scheduler to predict completion times for queue management and to estimate whether the battery has sufﬁcient charge for a given mission sequence.

## Throughput Estimate

Based on the mission timing data, the single-robot throughput is approximately 80 picks/hour (3600 s / 45 s per pick). This throughput ﬁgure assumes continuous operation with negligible inter-mission idle time—in practice, mission queuing and dispatch overhead add 1–2 s per mission, reducing effective throughput to approximately 75 picks/hour. A ﬂeet of ﬁve robots is estimated to achieve approximately 300 picks/hour (75% efﬁciency due to path conﬂict avoidance), comparable to the operational throughput of entry-level commercial AMR systems at a fraction of the acquisition cost. The 75% ﬂeet efﬁciency factor accounts for spine corridor congestion: with a single-lane spine corridor, robots must yield to oncoming trafﬁc, creating brief delays. A dual-lane spine (separate northbound and southbound lanes) would increase ﬂeet efﬁciency to approximately 90%, yielding 360 picks/hour from ﬁve robots. For context, a human picker in a comparable warehouse layout typically achieves 60–80 pick- s/hour (limited by walking speed and search time). The ATLAS single-robot throughput of 75–80 picks/hour matches human performance while operating continuously (24/7 vs. 8-hour shifts), yielding an effective throughput multiplier of 3*×* on a daily basis.

## UR3 Arm Performance

The UR3 arm subsystem demonstrated successful pick-and-place operation across all tested conﬁgurations. The arm controller operates at 500 Hz, matching the UR robot’s typical Ether- CAT communication rate and providing sub-2 ms position update latency for smooth trajectory execution. The PILZ LIN Cartesian planner achieved consistent approach and retreat trajectories with a 10 mm step size. The step size represents the maximum Cartesian distance between consecutive inverse kinematics solutions along the straight-line path; smaller step sizes produce smoother trajectories but require more computation. The 10 mm value was determined empirically as the largest step size that maintains visually smooth motion without perceptible jerkiness—at the UR3’s typical Cartesian speed of 0.1 m/s during approach, the 10 mm step produces waypoints every 100 ms, well within the controller’s interpolation capability. MTC compositional planning successfully sequenced nine stages into a coherent mission plan: CurrentState, OpenGripper, Approach, Pick (descend + close), Lift, Move, Place (descend + open), Retreat, and Return-to-Home. The MTC planner consistently found globally feasible solutions (all stages kinematically compatible) within 3–5 s of planning time, demonstrating that the compositional approach does not signiﬁcantly increase planning latency compared to individual stage planning. OMPL RRTConnect planning time was non-deterministic (inherent to sampling-based plan- ning) but consistently below 2 s for free-space motions in the tested environments. The plan- ning time distribution across 100 planning queries showed: median 0.8 s, mean 1.1 s, 95th percentile 1.8 s, maximum 2.4 s. This variability is acceptable for the application because arm motion is not time-critical (the AGV has already stopped, and the arm operates during the PICKUP phase which has no hard deadline). The end-effector positioning accuracy in simulation was veriﬁed to be within the numerical tolerance of the KDL inverse kinematics solver (10^−^^5^^^m = 0.01 mm), which is several orders of magnitude better than the physical UR3’s repeatability speciﬁcation (*±*0*.*1 mm). This conﬁrms that simulation results are not limited by kinematic solver accuracy and that physical deploy- ment performance will be limited by hardware factors (joint backlash, structural compliance, thermal effects) rather than algorithmic factors.

## Comparison with Commercial Systems

Table 4.3 compares the ATLAS system with established commercial AGV platforms. This comparison contextualises the project’s achievement within the broader landscape of ware- house automation technology. Table 4.3: Comparison with Commercial AGV Systems

### Feature

### ATLAS (this

**work)**

### Amazon

### Robotics

### MiR 250

### OTTO 100

Navigation Line following Vision SLAM LiDAR SLAM LiDAR SLAM Robotic arm UR3 (inte- grated) None None None Fleet size 1 (research) 800,000+ Unlimited Unlimited Cost *≈*$1,250 Proprietary $25,000 $30,000 Open-source Yes No No No The ATLAS system is uniquely distinguished by its integration of an onboard robotic arm for material picking, at approximately 5% of the cost of commercial alternatives. While commer- cial systems offer superior speed, ﬂeet scalability, and production-hardened reliability, the AT- LAS system provides capabilities that no commercial platform offers at any price: open-source full-stack access for research modiﬁcation, integrated manipulation for autonomous item-level picking, and a documented simulation-to-physical deployment pathway for academic replica- tion. The comparison must acknowledge the ATLAS system’s limitations relative to commercial platforms: (1) navigation ﬂexibility—commercial SLAM-based systems can navigate freely in unstructured environments, while ATLAS requires pre-installed line guidance; (2) speed— commercial systems achieve 1.5–2.0 m/s versus ATLAS’s 0.4 m/s; (3) reliability—commercial systems demonstrate 99.99%+ uptime in production environments through redundant sensors, fail-safe electronics, and extensive ﬁeld testing, while ATLAS is validated only in simulation; (4) ﬂeet coordination—commercial systems include sophisticated multi-robot trafﬁc manage- ment, while ATLAS operates as a single unit. These limitations deﬁne the boundary between research prototype and production deployment, and each represents a clear pathway for future development.

## Challenges and Limitations

### Current Limitations

The following limitations were identiﬁed during system development and validation testing: 1.** Single robot only — no ﬂeet coordination:** The current architecture supports only one AGV operating simultaneously. Fleet operation would require a centralised trafﬁc manager, path reservation protocols, and deadlock prevention algorithms. The ROS 2 architecture supports multi-robot namespacing, so the software infrastructure for ﬂeet operation is partially in place. 2.** No obstacle detection — relies on clear pre-planned paths:** The AGV has no forward- looking sensors (LiDAR, ultrasonic, camera) for obstacle detection. If an unexpected object is placed on the path, the robot will attempt to drive through/into it. Physical deployment in any environment with human trafﬁc requires the addition of safety-rated obstacle detection. 3.** Simulated sensors — no real noise models:** The Gazebo sensor plugins produce ide- alised data without the noise characteristics of physical sensors. Physical deployment will encounter: ADC quantisation noise on line sensor readings, IMU drift and vibration noise, RFID read failures due to tag orientation, and encoder count errors due to wheel slip. 4.** No payload physics — object attachment is kinematic:** In simulation, grasped objects are attached to the gripper link as a rigid kinematic constraint. Physical grasping involves complex contact mechanics, friction modelling, and grasp stability analysis that are not captured in the current simulation ﬁdelity. 5.** Fixed world — cannot handle dynamic warehouse layout changes:** The warehouse layout (line paths, shelf positions, RFID tag locations) is hardcoded in conﬁguration ﬁles. A production system would require a Warehouse Management System (WMS) interface for dynamic layout updates. 6.** Odometry drift — acceptable due to RFID resets but accumulates on longer paths:** Between junction resets, dead reckoning error accumulates at approximately 2% of dis- tance travelled. For the longest inter-junction distance (4 m), this represents 80 mm of position uncertainty—acceptable for line following but potentially problematic for pre- cise docking. 7.** Hardcoded pick poses — requires a perception pipeline for generalised picking:** Object locations are speciﬁed as ﬁxed coordinates in the task deﬁnition. A production system requires computer vision (depth camera + object detection) to locate objects dy- namically. 8.** Fragile bootstrap timing for controller initialisation:** The arm controller activation se- quence (spawning Gazebo, loading controllers, starting MoveIt 2) is sensitive to timing— if controllers are activated before Gazebo has fully loaded, initialisation fails. This is mitigated with sleep delays but requires a more robust health-checking initialisation se- quence for production use.

### Design Trade-offs

Key design trade-offs are summarised in Table 4.4. Each trade-off represents a deliberate engi- neering decision where the selected option provides clear advantages for the project’s require- ments at the cost of certain capabilities that are not immediately needed. Table 4.4: Design Trade-offs Summary

### Decision

### Beneﬁt

### Cost

Line following vs SLAM Simple, deterministic Fixed paths only Junction counting No map needed Cannot skip junctions Bang-bang turns Simple, reliable Slightly imprecise Single velocity ar- biter Safety guaranteed Complex state machine *K**I* = 0 (line fol- lower) No windup Slight error on curves GUI separate pro- cess Crash-safe WiFi dependency Position command (arm) Simple control No force compliance KDL default IK solver No extra dependencies Slower than TracIK The trade-off philosophy follows a consistent principle: prefer the simpler solution when it meets requirements, reserving complex alternatives for situations where the simpler approach demonstrably fails. This principle reduces development time, debugging complexity, and main- tenance burden while producing a system whose behaviour is fully understood and predictable.

## Future Scope

Based on the results and limitations identiﬁed above, the following extensions are proposed, ordered by implementation priority and expected impact: 1.** Multi-robot ﬂeet coordination:** Implementation of a ﬂeet manager with trafﬁc rules (one-way corridors, intersection priority), deadlock prevention (wait-die or wound-wait protocols), and dynamic path allocation. The ROS 2 multi-robot namespacing infrastruc- ture supports this extension without architectural changes to individual robot software. 2.** LiDAR integration:** Addition of a 2D LiDAR sensor (RPLiDAR A1, $100) for obstacle detection and dynamic safety zones. The LiDAR data would feed a safety supervisor node that overrides the velocity arbiter when obstacles are detected within a distance- dependent speed zone (similar to ISO 3691-4 safety-rated speed limitation). 3.** Computer vision for arm:** Integration of an Intel RealSense D435 depth camera with YOLO-v8 object detection and point cloud grasp pose estimation, replacing hardcoded pick coordinates with dynamic object localisation. This extension enables picking of previously unseen objects at arbitrary positions within the arm workspace. 4.** Reinforcement learning:** SAC-based (Soft Actor-Critic) policy trained in MuJoCo with Gazebo transfer for adaptive grasping strategies that learn optimal grasp poses from ex- perience rather than requiring explicit programming for each object type. 5.** LLM-based task planning:** Natural language mission dispatch via a locally hosted LLM (e.g., LLaMA3) integrated with the MTC pipeline, enabling operators to issue commands like “pick the red box from shelf 5 and bring it to the packing station” rather than specifying numeric shelf IDs. 6.** Digital twin:** Real-time simulation-to-physical synchronisation for remote monitoring, predictive maintenance, and “what-if” scenario testing without interrupting physical op- erations. 7.** Force/torque sensing:** Compliant grasping and contact detection using a wrist-mounted F/T sensor (ATI Nano17 or equivalent), enabling force-controlled insertion tasks and preventing damage to fragile objects through real-time force monitoring. 8.** Battery management:** Real charge monitoring using a coulomb-counting battery gauge (INA219), autonomous return-to-dock when charge falls below threshold, and charging contact alignment at the docking station. Each future extension is designed to be additive—implementable without modifying the ex- isting validated codebase, only adding new nodes and capabilities that integrate through the existing ROS 2 topic architecture. This extensibility validates the architectural decision to use loosely-coupled, topic-based communication throughout the system.

## Conclusions

### Achievements

The ATLAS Smart Warehouse AGV with integrated UR3 robotic arm successfully demon- strates the following achievements: 1.** Complete autonomous mission execution with zero human intervention:** The sys- tem executes the full pick-and-transfer cycle—from mission dispatch through navigation, shelf identiﬁcation, arm manipulation, and return-to-dock—without requiring any human action beyond initial mission dispatch. This validates the core thesis that an integrated mobile manipulator can automate warehouse picking end-to-end. 2.** Robust line-following navigation with sub-5 mm tracking accuracy at 0.4 m/s:** The PD controller achieves consistent, stable tracking with measured RMS error of 3.2 mm, demonstrating that classical control techniques (when properly tuned and applied within their validity regime) provide excellent performance for structured navigation tasks with- out the complexity of modern adaptive or learning-based controllers. 3.** Reliable junction-based wayﬁnding with RFID position conﬁrmation at 21 ware-** **house locations:** The combination of junction counting (coarse localisation) with RFID tag veriﬁcation (precise identiﬁcation) provides deterministic, failure-resistant position knowledge without the computational overhead and environmental sensitivity of SLAM- based localisation. 4.** A professional 12-state mission FSM with docking recovery and emergency stop:** The FSM provides complete mission lifecycle coverage with formally veriﬁable state transitions, demonstrating that complex autonomous behaviour can be decomposed into manageable, testable states with well-deﬁned transitions. 5.** Industrial-quality safety through E-Stop, velocity arbiter, and docking alignment** **veriﬁcation:** The velocity arbiter architectural pattern ensures that no software fault in navigation or planning nodes can produce unsafe wheel motion, providing a structural safety guarantee independent of individual node correctness. 6.** A modern ROS 2 architecture across 11 packages:** The package structure demon- strates production-quality software engineering practices: clear dependency manage- ment, interface abstraction, build system integration, and modular testing capability. 7.** Successful UR3 pick-and-place using MoveIt 2 and MTC compositional planning:** The arm subsystem validates that complex manipulation tasks can be reliably speciﬁed and executed using the MTC compositional framework, with the dual-planner strategy (OMPL + PILZ) providing both ﬂexible free-space motion and precise Cartesian control. 8.** Integration of OMPL and PILZ planners for optimal motion coverage:** The dual- planner architecture demonstrates that different motion planning algorithms are com- plementary rather than competing—each excels in its speciﬁc domain (free-space vs. Cartesian), and their combination provides superior capability to either alone. 9.** A PyQt5 control centre with real-time telemetry:** The GUI provides the human- machine interface required for practical deployment, demonstrating that modern desktop application frameworks (Qt) can be seamlessly integrated with ROS 2 robotic systems through thread-safe signal architectures. 10.** A documented simulation-to-physical pathway with 70% code reuse:** The hard- ware abstraction architecture is validated by the clean separation between platform- independent control logic (70% of codebase) and platform-speciﬁc hardware drivers (30%), conﬁrming that simulation-ﬁrst development produces deployable systems when architectural discipline is maintained.

### Contributions

The primary engineering contributions of this project are: 1.** A complete open-source warehouse AGV system integrating mobile navigation with** **a 6-DOF robotic arm under a uniﬁed ROS 2 architecture:** To the best of the authors’ knowledge, no existing open-source project provides a complete, documented, working integration of a mobile AGV platform with a collaborative robotic arm for warehouse pick-and-place operations. This contribution enables academic institutions to replicate and extend the system for research without commercial licensing barriers. 2.** The velocity arbiter pattern for safe multi-source velocity management:** The archi- tectural pattern of routing all velocity commands through a single priority-based multi- plexer with E-Stop override provides a reusable safety design pattern applicable to any mobile robot with multiple velocity sources. 3.** An RFID-augmented junction counting localisation strategy without SLAM:** The combination of junction counting with RFID veriﬁcation provides a localisation ap- proach that is both simpler and more deterministic than SLAM for structured environ- ments, contributing a validated alternative for applications where SLAM’s computational overhead and probabilistic nature are undesirable. 4.** A documented simulation-to-physical deployment methodology:** The conversion ta- ble, GPIO mapping, hardware driver architecture, and code reuse analysis provide a replicable methodology for transitioning ROS 2 simulation projects to physical hard- ware, addressing a common pain point in academic robotics where simulation results fail to transfer to real hardware due to undocumented architectural dependencies. 5.** Integration of MTC-based compositional task planning with PILZ Cartesian plan-** **ning:** The combination of MTC’s multi-stage task speciﬁcation with PILZ’s determin- istic Cartesian planning demonstrates a practical approach to reliable manipulation that bridges the gap between the ﬂexibility of sampling-based planners and the predictability required for industrial certiﬁcation.

### Final Assessment

The ATLAS project bridges academic mobile robotics and industrial AGV systems, demon- strating that a complete warehouse automation solution encompassing mobile navigation, RFID localisation, robotic arm manipulation, and an operator GUI can be built with open-source soft- ware and commercial off-the-shelf hardware at approximately 5% of the cost of commercial alternatives. The system achieves all stated objectives with measured performance exceeding requirements in every metric. The engineering methodology—simulation-ﬁrst development with hardware-abstracted architecture— is validated as a viable approach for academic robotics projects where hardware access is lim- ited, iteration speed is critical, and the end goal is a physically deployable system. The 70% code reuse ratio demonstrates that this methodology does not sacriﬁce deployability for devel- opment convenience. The system is production-ready in control logic and requires only hardware interface node replacement for physical robot deployment. The documented conversion pathway, GPIO map- ping, component selection, and power budget provide a complete engineering speciﬁcation for physical build, reducing the transition from simulation to reality from a research problem to an integration exercise. The ATLAS Smart Warehouse AGV represents a contribution to the democratisation of ware- house automation technology—making capabilities previously available only through $25,000+ commercial systems accessible to academic institutions, small businesses, and individual re- searchers at a fraction of the cost, with full source-code access for modiﬁcation and extension.

# Bibliography

[1] De Ryck M, Versteyhe M, Debrouwere F, 2020, Automated guided vehicle systems, state- of-the-art control algorithms and techniques,* Journal of Manufacturing Systems*, vol. 54, pp. 152–173. [2] Azadeh K, De Koster R, Roy D, 2019, Robotized and automated warehouse systems: review and recent developments,* Transportation Science*, vol. 53, no. 4, pp. 917–945. [3] Siegwart R, Nourbakhsh I R, Scaramuzza D, 2011,* Introduction to Autonomous Mobile* *Robots*, 2nd edn, MIT Press, Cambridge, MA. [4] Ogata K, 2010,* Modern Control Engineering*, 5th edn, Prentice Hall, Upper Saddle River, NJ. [5] Open Robotics, 2023, ROS 2 Humble Hawksbill Documentation, Available at: https://docs.ros.org/en/humble (Accessed: May 2026). [6] Finkenzeller K, 2010,* RFID Handbook: Fundamentals and Applications in Contactless* *Smart Cards, Radio Frequency Identiﬁcation and Near-Field Communication*, 3rd edn, Wiley, Chichester. [7] Corke P, 2017,* Robotics, Vision and Control*, 2nd edn, Springer, Cham. [8] Dudek G, Jenkin M, 2010,* Computational Principles of Mobile Robotics*, 2nd edn, Cam- bridge University Press, Cambridge. [9] Wurman P R, D’Andrea R, Mountz M, 2008, Coordinating hundreds of cooperative, au- tonomous vehicles in warehouses,* AI Magazine*, vol. 29, no. 1, pp. 9–20. [10] Franklin G F, Powell J D, Emami-Naeini A, 2015,* Feedback Control of Dynamic Systems*, 7th edn, Pearson, Harlow. [11] Siciliano B, Khatib O (eds), 2016,* Springer Handbook of Robotics*, 2nd edn, Springer, Cham. [12] Quigley M, Gerkey B, Smart W D, 2015,* Programming Robots with ROS*, O’Reilly Me- dia, Sebastopol, CA. [13] ISO 3691-4:2020, Industrial trucks — Safety requirements and veriﬁcation — Part 4: Driverless industrial trucks and their systems, International Organization for Standardiza- tion, Geneva. [14] Pololu Corporation, 2023, QTR-8A Reﬂectance Sensor Array User’s Guide, Available at: https://www.pololu.com/docs/0J13 (Accessed: May 2026). [15] Bosch Sensortec, 2023, BNO055 Intelligent 9-axis Absolute Orientation Sensor Datasheet, Bosch Sensortec GmbH, Reutlingen. [16] Thrun S, Burgard W, Fox D, 2005,* Probabilistic Robotics*, MIT Press, Cambridge, MA. [17] Object Management Group, 2015, Data Distribution Service (DDS) Speciﬁcation v1.4, OMG Document formal/2015-04-10. [18] Open Source Robotics Foundation, 2023, Gazebo Classic 11 Documentation, Available at: https://classic.gazebosim.org/ (Accessed: May 2026). [19] Ollero A, 2005,* Intelligent Mobile Robot Navigation*, Springer, Berlin. [20] IEC 61496:2020, Safety of machinery — Electro-sensitive protective equipment, Interna- tional Electrotechnical Commission, Geneva.

# Complete State Machine Transition Table

Table A.1 provides the complete state machine transition table for the ATLAS AGV mission FSM. Every possible state-event combination is explicitly documented, ensuring that the FSM behaviour is fully deterministic and veriﬁable. States not listed in the “Current State” column for a given event remain unchanged (implicit self-transition). Table A.1: Complete AGV FSM Transition Table

### Current State

### Event / Condition

### Next State

### Action

IDLE Queue not empty AND not E-stopped NAV_SPINE Pop mission; reset junction count NAV_SPINE Junction count == target aisle TURNING Publish turn_cmd (*−**π/*2) TURNING turn_done received NAV_AISLE — NAV_AISLE tag_event matches target AT_SHELF — AT_SHELF 0.5 s elapsed PICKUP — PICKUP 2.0 s elapsed PIVOT Publish turn_cmd (*π*) PIVOT turn_done received RET_AISLE — RET_AISLE Junction detected RET_TURN Publish turn_cmd

$$(+π/2)$$
  *(Equation 110)*

RET_TURN turn_done received RET_SPINE — RET_SPINE Home tag detected DOCKED — DOCKED 0.5 s elapsed DOCKING Enter docking FSM DOCKING Position veriﬁed ALIGNMENT — ALIGNMENT Heading + lateral OK READY — READY 0.5 s elapsed IDLE Log “SYSTEM READY” ANY /atlas/estop received ERROR Set estopped=True

### Current State

### Event / Condition

### Next State

### Action

ERROR /atlas/reset received IDLE Clear state ANY /atlas/reset_to_dock RESETTING Gazebo teleport RESETTING Callback received DOCKING Verify alignment The FSM implements several safety properties that can be veriﬁed by inspection of the transi- tion table: •** Liveness:** From any non-error state, there exists a ﬁnite sequence of events that leads back to IDLE (the system never deadlocks under normal operation). •** Safety:** The E-Stop transition is available from ANY state (universal override), and the ERROR state can only be exited by explicit reset (no automatic recovery from E-Stop). •** Determinism:** For every (state, event) pair, exactly one transition is deﬁned (no ambi- guity in next-state selection). •** Completeness:** Every state has at least one outgoing transition (no terminal states except ERROR without reset).

# Software Installation and Launch Guide

This appendix provides complete instructions for replicating the ATLAS development environ- ment on a fresh Ubuntu installation.

## Prerequisites

# Ubuntu 22.04 with ROS 2 Humble (AGV subsystem) sudo apt install ros-humble-desktop sudo apt install ros-humble-gazebo-ros-pkgs sudo apt install python3-pyqt5 # Ubuntu 24.04 with ROS 2 Jazzy (Arm subsystem) sudo apt install ros-jazzy-desktop sudo apt install ros-jazzy-moveit sudo apt install ros-jazzy-moveit-task-constructor-core

## Build Procedure

cd ~/atlas_ws source /opt/ros/humble/setup.bash colcon build --symlink-install source install/setup.bash

## Launch and Operation

# Full AGV simulation ros2 launch atlas_bringup atlas_full.launch.py # UR3 arm simulation ros2 launch ur_gazebo ur.gazebo.launch.py # Send mission via CLI ros2 run atlas_mission_manager send_mission S05 # Launch GUI standalone ros2 run atlas_mission_manager atlas_gui # Real-world AGV (no Gazebo) ros2 launch atlas_bringup atlas_real.launch.py

## Veriﬁcation Steps

After launching the AGV simulation, verify correct operation with: # Check all nodes are running ros2 node list # Verify topic communication ros2 topic echo /atlas/line_error ros2 topic echo /atlas/imu/yaw ros2 topic echo /atlas/mission_state # Monitor velocity commands ros2 topic echo /cmd_vel