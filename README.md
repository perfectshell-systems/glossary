

---

# Part I: The 12 Necessary Systems

**1. Mechanical Structure**
The mechanical structure is the physical skeleton of the robot: the frame, links, joints, mounting points, enclosures, and cable routing. It defines the robot's shape, reach, degrees of freedom, and structural integrity. Without a rigid frame, actuators have no leverage, sensors vibrate uncontrollably, and payload capacity becomes negligible. The structure transmits forces from actuators to the environment and determines whether the robot survives real-world operation.

**2. Actuation**
Actuation is the system of motors, gearboxes, and transmissions that convert electrical energy into physical motion. It is the muscle of the robot, determining torque, speed, precision, backdrivability, and power consumption. Without actuation, the robot cannot move, manipulate objects, or interact with its environment. Actuator selection and control mode determine whether the robot can perform tasks accurately, safely, and efficiently over millions of cycles.

**3. Power**
The power system is the battery, power distribution network, and management electronics that supply energy to every component. It determines runtime, peak power delivery, weight, and safety. A poorly designed power system causes voltage sag during high-torque movements, leading to control instability or component failure. The battery must store enough energy, deliver enough peak current, and remain safe under all foreseeable conditions including overcharge, overdischarge, short circuit, and thermal runaway.

**4. Sensing**
Sensing is the system of cameras, IMUs, encoders, force sensors, tactile sensors, and range finders that give the robot information about its own state and its environment. Without sensing, the robot is blind, deaf, and unaware of its own position. Sensing is the foundation of feedback control, perception, and autonomy. The quality, quantity, and fusion of sensor data determine what the robot can do and how reliably it can do it.

**5. Onboard Compute**
Onboard compute is the computer or computers mounted on the robot that run perception, control, communication, and data logging software. Latency, bandwidth, and reliability require that critical computation happens onboard. The onboard compute determines what algorithms can run and how fast they can run. A robot that must react in real time cannot wait for a round trip to a distant server.

**6. Communication**
Communication is the hardware and protocols that connect the robot to the operator, the cloud, and other robots. It carries control commands to the robot and carries video, audio, and telemetry back to the operator. Without communication, a remotely controlled avatar is just a robot. The quality of communication directly determines the quality of teleoperation, including latency, bandwidth, reliability, and security.

**7. Low-Level Control**
Low-level control is the firmware and real-time control loops that stabilize the robot, execute motor commands, and handle sensor feedback at high frequency. High-level commands must be translated into motor currents updated hundreds or thousands of times per second. Without low-level control, the robot is unstable, inaccurate, and unsafe. The control loop must be fast, deterministic, and robust across the full range of operating conditions.

**8. Teleoperation Interface**
The teleoperation interface is the hardware and software the human operator uses to control the robot. This includes VR headsets, haptic gloves, joysticks, force-feedback devices, and web interfaces. It is the bridge between human intention and robot action. A well-designed interface makes the robot feel like an extension of the operator's body; a poorly designed one makes the robot difficult to control, fatiguing to operate, and prone to error.

**9. Middleware**
Middleware is the software layer that allows all the robot's software components to communicate with each other. ROS 2 is the de facto standard, with DDS or Zenoh as the communication backbone. A modern robot has dozens or hundreds of software processes running simultaneously. Without middleware, each process would need custom code to talk to every other process; middleware provides standardized communication, discovery, and coordination.

**10. Perception and Autonomy**
Perception and autonomy is the software that helps the robot understand its environment and make decisions. This includes SLAM, object detection, navigation, motion planning, and manipulation planning. Teleoperation alone is limited by the operator's attention, reaction time, and cognitive load. Perception and autonomy assist the operator by handling low-level tasks so the operator can focus on high-level goals.

**11. Safety and Compliance**
Safety and compliance is the software and hardware that prevents harm to people, property, and the robot itself, and the documentation and processes that demonstrate compliance with regulations. A robot that can move and manipulate objects can also injure people, damage property, and destroy itself. Safety systems prevent accidents; compliance systems allow the robot to be deployed legally in workplaces and public spaces. Fail-safe design, redundancy, constraint enforcement, and traceability are all essential.

**12. Data Logging and Learning**
Data logging and learning is the pipeline that records teleoperation sessions, stores them as structured datasets, and trains AI models so the robot can learn to perform tasks more autonomously over time. Teleoperation is limited by the operator's availability and attention. Learning from teleoperation data allows the robot to acquire skills that can be executed autonomously, reducing the operator's workload and increasing the robot's usefulness. Data quality, synchronization, coverage, volume, and standardization determine whether learning succeeds.

---

# Part II: The Complete Software Stack

## 1. Design & Simulation

Before deploying physical hardware, simulation allows you to develop and test control logic safely, generate synthetic training data, and validate algorithms in a risk-free environment. Simulation is essential because it is cheaper, faster, and safer than building and testing on real hardware, and it allows you to test scenarios that would be dangerous or impossible in reality.

- **NVIDIA Isaac Sim** — High-fidelity robotics simulation on Omniverse. Open-source and integrates deeply with ROS 2. Best for large-scale RL, photorealistic sensor simulation, and synthetic data generation.
- **Gazebo (Ignition / Gazebo Harmonic)** — ROS-native open-source simulator. Robust physics engine and excellent ROS 2 integration. Best for rapid prototyping, Nav2 testing, and multi-robot system simulation.
- **Webots** — Open-source simulator with official ROS 2 integration. Fast startup, low resource consumption. Best for education and mobile-robot prototyping.
- **CoppeliaSim (V-REP)** — General-purpose simulator with strong manipulation focus and user-friendly scene editor.
- **PyBullet / MuJoCo** — Physics engines designed for speed and scalability. MuJoCo is the standard for contact-rich manipulation; PyBullet for Pythonic API and ease of use. Best for RL scalability.
- **AirSim** — Open-source simulator for drones and autonomous vehicles, often paired with Unreal Engine for photorealism.
- **Unity / Unreal Engine** — Game engines for high-fidelity digital twins, teleoperation training, and synthetic data.

## 2. Firmware & Autopilot (Low-Level Control)

Firmware and autopilot software runs on the robot's microcontrollers and flight controllers, handling sensor fusion, attitude estimation, and real-time control loops. It is necessary because high-level software cannot directly control motors at the required rate; the firmware translates commands into motor currents and stabilizes the robot in real time.

- **PX4 Autopilot** — Open-source flight control stack for drones (multirotor, fixed-wing, VTOL) and rovers. Modular, configurable, default for research and commercial integration. Uses MAVLink and supports MAVROS for ROS 2 integration.
- **ArduPilot** — Mature open-source autopilot suite supporting Copter, Plane, Rover, Sub, Tracker. Robust autonomous mission capabilities, extensive parameter set, MAVLink, Lua scripting.
- **MAVLink** — De-facto communication protocol for drone-to-GCS and drone-to-companion-computer messaging. Lightweight, standard message set. Libraries for Python (pymavlink), C++, and others.
- **DroneKit** — Python library for controlling MAVLink-compatible vehicles. Script custom flight behaviors, mission planning, telemetry monitoring.
- **Betaflight / iNAV** — FPV racing drone firmware optimized for low latency and manual flight performance.

## 3. Middleware & Communication

Middleware is the nervous system of the robot, allowing different software components to communicate with each other and across the network. It is necessary because a modern robot has many processes running simultaneously, and without a standardized communication layer, integrating them would be impossibly complex.

- **ROS 2 (Robot Operating System 2)** — De-facto standard middleware for modern robotics. Distributed framework of nodes, topics, services, actions. ROS 2 Jazzy is the current LTS release.
- **DDS (Data Distribution Service)** — Default communication backbone for ROS 2. Common implementations: **Fast DDS** and **Cyclone DDS**. Automatic discovery and rich QoS policies, but significant discovery overhead in large or wireless networks.
- **Zenoh** — Alternative ROS 2 middleware (RMW) preferred for limited-bandwidth networks (Wi-Fi, 4G/5G). Interest-based filtering reduces discovery overhead by up to 99% compared to DDS.
- **MAVROS** — ROS 2 package bridging MAVLink-enabled autopilots (PX4, ArduPilot) with ROS 2. Translates MAVLink messages into ROS topics and services.
- **MQTT** — Lightweight publish-subscribe messaging protocol for telemetry and control signaling. Ideal for constrained networks, often paired with WebRTC for video.
- **AimRT** — Self-developed middleware by AgiBot, supporting Protobuf and ROS2 Message formats, providing RPC, Topic, and other communication modes. Compatible with native ROS2 ecosystem.

## 4. Perception & Autonomy

Perception and autonomy software allows the robot to understand its environment, localize itself, and make decisions. It is necessary because teleoperation alone is limited by the operator's attention and reaction time; autonomy handles low-level tasks like obstacle avoidance and localization, freeing the operator to focus on high-level goals.

- **ORB-SLAM3** — State-of-the-art visual, visual-inertial, and multi-map SLAM. Works with monocular, stereo, and RGB-D cameras. Highly accurate but CPU-intensive.
- **VINS-Fusion** — Visual-inertial odometry and SLAM framework. Strong for aerial robotics where GPS is unreliable.
- **RTAB-Map** — Real-time appearance-based SLAM. Well-suited for large-scale mapping and loop closure.
- **SLAM Toolbox** — ROS 2 native SLAM package for 2D mapping and localization using laser scans.
- **MOLA** — Modular Optimization framework for Localization and mApping. C++ libraries and ROS 2 packages for SLAM and localization.
- **OpenCV** — Core library for image processing, feature detection, and camera calibration.
- **YOLO (v5–v12, YOLO26)** — Real-time object detection, segmentation, and pose estimation. YOLOv8-nano and YOLOv11-nano for edge deployment.
- **Nav2** — ROS 2 navigation stack. Behavior trees, costmaps, and controllers for mobile robots.
- **MoveIt 2** — ROS 2 motion planning framework for robotic arms. Collision-free trajectories for manipulation.
- **Aerostack2** — ROS 2 framework for autonomous multi-aerial-robot systems. Control, planning, perception for drone swarms.
- **PCL (Point Cloud Library)** — Processing and filtering of 3D point cloud data for object recognition and environment modeling.
- **Ceres Solver** — Nonlinear least squares optimization for SLAM bundle adjustment and sensor fusion.
- **GTSAM** — Factor-graph optimization for SLAM and sensor fusion.
- **Sophus** — Lie groups (SO(3), SE(3)) for kinematics and SLAM backends.

## 5. Teleoperation & Control Interfaces

Teleoperation software is how the operator controls the avatar. It is necessary because the goal is intuitive, low-latency control that makes the robot feel like an extension of the operator's body. The software must map operator input to robot motion and provide feedback to close the loop.

- **Quest2ROS2** — Open-source ROS 2 framework for bi-manual teleoperation using Meta Quest controllers. Relative motion control for intuitive, pose-independent operation.
- **BEAVR** — Open-source, bimanual, multi-embodiment VR teleoperation system. Zero-copy streaming architecture achieving ≤35 ms latency. Records synchronized multimodal demonstrations directly in LeRobot dataset schema.
- **NVIDIA Isaac Teleop** — Open-source framework providing a standardized interface for teleoperation devices (VR headsets, gloves, joysticks). Captures human demonstrations in standardized formats for imitation learning.
- **XRoboToolkit** — Cross-platform framework for robot teleoperation. Supports wheeled mobile robots and robotic arms such as UR5, with dexterous motion retargeting and full-body tracking via PICO Motion Trackers.
- **Phone2Act** — Framework that transforms a consumer smartphone into a 6-DoF robot controller via Google ARCore, built on a modular ROS 2 architecture.
- **GHOST** — Open-source VR teleoperation system enabling single-operator control of two mobile manipulators via direct low-level commands using only onboard sensing.
- **Haply Inverse3 / VerseGrip** — Haptic devices providing directional force feedback. Supported in Isaac Lab workflows.
- **DOGlove** — Low-cost, open-source haptic force feedback glove for dexterous manipulation.
- **TriPilot-FF** — Open-source whole-body teleoperation with foot-operated pedal and lidar-driven haptic feedback.
- **ModPack** — Modular teleoperation system with plug-and-play capability modules, including joint-level haptic feedback.
- **LiveKit Portal** — Transport layer for teleoperation using WebRTC. Human teleoperator and AI policy can join the same session and hand off control mid-session.
- **Teledex** — Lets you control robot frames using iOS device AR data, with optional 3D-printed holder to track finger motion.
- **TwinTouch** — Lightweight cable-driven wearable haptic system capable of delivering up to 7.5 N per finger without constraining arm movement.
- **Visuo-Haptic Teleoperation Platform** — Combines VR, optical tracking, and a vibrotactile operator tool within a ROS 2-based control architecture. Achieved end-to-end latency below 15 ms.

## 6. Video & Data Streaming

Video and data streaming software carries the robot's camera and sensor feeds back to the operator with minimal delay. It is necessary because the operator must see and hear what the robot does in real time to control it effectively. Low latency and high reliability are critical for a natural teleoperation experience.

- **WebRTC (via LiveKit)** — Most common choice for low-latency video streaming in teleoperation. UDP/SRTP, adaptive bitrate, data channels for control.
- **MQTT + WebRTC Hybrid** — Separates control pipeline (MQTT for real-time commands) from media pipeline (WebRTC for video).
- **RISE (Reliable Intelligent Stream Encoding)** — Predictive multi-link video stream system for fading channels in teleoperation.
- **RTSP** — Well-established protocol for streaming video from IP cameras. Used in VR-based teleoperation.
- **Tencent Cloud TRRO** — Commercial SDK offering ultra-low latency audio and video transmission (<30ms).

## 7. Data Collection & AI Training

Data collection and AI training software records teleoperation sessions and trains models so the robot can learn tasks. It is necessary because teleoperation is limited by the operator's availability; learning from demonstrations allows the robot to acquire skills that can be executed autonomously.

- **LeRobot (Hugging Face)** — Open-source framework for building, training, and deploying real-world robot learning systems. Coherent dataset format (LeRobot Dataset), shared training loop for ACT, Diffusion Policy, and VLA-adapter.
- **NVIDIA Isaac GR00T 1.7** — First open and commercially viable robot foundation model. Makes it easier to post-train and deploy models through LeRobot workflows.
- **NVIDIA Cosmos 3** — Frontier world foundation model for physical AI. Generates and augments robotics data, simulates scenarios, and supports policy development.
- **ClearML** — AI infrastructure platform for MLOps pipeline. Queues training jobs, logs runs, versions datasets.
- **FlagOS-Robo** — Open-source toolkit for embodied data loading, model training, inference, and evaluation.
- **DROID-Style Dataset Conventions** — Standardized dataset formats for robot manipulation, promoting interoperability.
- **Grabette** — Open-source toolkit for collecting robotic manipulation demonstrations and turning them into training-ready datasets.

## 8. Visualization & Debugging

Visualization and debugging tools allow engineers to see what the robot is thinking, monitor sensor data, and diagnose problems. They are necessary because a complex robot system is impossible to debug without tools that display its internal state.

- **RViz2** — Standard 3D visualization tool within ROS. Displays sensor data, robot models, TF transforms, and navigation paths.
- **Foxglove Studio** — Modern, browser-based visualization tool. MCAP-native, supports remote viewing and data analysis.
- **PlotJuggler** — Time-series data visualization. Drag-and-drop ROS topics onto plots for debugging control loops.
- **Lichtblick** — Browser-based visualization tool, similar to Foxglove, for remote monitoring.

## 9. Ground Control & Fleet Management

Ground control and fleet management software operates and monitors one or many robots. It is necessary for commercial deployment, where multiple robots must be coordinated, monitored, and updated remotely.

- **QGroundControl (QGC)** — Cross-platform, open-source ground control station for drones. Supports any MAVLink-compatible vehicle, default for PX4.
- **Mission Planner** — Desktop GCS primarily for ArduPilot. Advanced tools: terrain following, auto-survey grid generation, log analysis.
- **MAVProxy** — Command-line MAVLink proxy and GCS. No GUI, ideal for scripting and headless ground stations.
- **OpenRobOps (ORO)** — Open-source, self-hostable robot operations and fleet management layer. ISO 21423 reference implementation.
- **Transitive Robotics** — Open-source framework for full-stack robotics: fleet management, health monitoring, configuration management.
- **NVIDIA Isaac Mission Control** — Lightweight fleet manager coordinating Isaac Cloud microservices.
- **Frota 360** — On-premise, physically isolated fleet management framework for heterogeneous robots in offshore oil and gas facilities.
- **AMRF** — Three-layer software architecture for autonomous mobile robot fleet management in warehouses.
- **Commercial Platforms** — MiR Fleet, LocusONE, Geek+ Robotic Engine, Addverb Movect for large-scale warehouse and logistics fleets.

## 10. Networking & Remote Access

Networking and remote access software creates a secure, reliable path between the operator and the robot, traversing firewalls and NAT. It is necessary for teleoperation from anywhere, not just the same local network.

- **Tailscale** — WireGuard-based mesh VPN. Private network without opening firewall ports. Widely adopted in robotics for secure remote access over LTE/5G.
- **ZeroTier** — Decentralized mesh VPN. No mandatory cloud control plane.
- **OmniEdge** — Zero-config P2P mesh VPN for AI, robotics, and edge computing. Deterministic, jitter-controlled networking.

## 11. Development & Deployment

Development and deployment tools are the underlying software used to write, build, and deploy all the other software. They are necessary for reproducible, reliable, and maintainable robotics development.

- **Docker** — Containerizes ROS 2 nodes and other software for reproducible deployment across simulation and real hardware.
- **Git & GitHub** — Version control and collaboration on all software components.
- **Python & C++** — Primary programming languages. Python for prototyping and AI/ML; C++ for performance-critical control loops.
- **Linux (Ubuntu)** — Dominant operating system for robotics development.
- **`ros2_control`** — Real-time control framework for ROS 2. Standard interface for hardware components and controllers.
- **Colcon** — Build tool for ROS 2 workspaces.

## 12. Safety & Reliability Assurance

Safety and reliability software prevents harm to people, property, and the robot itself. It is necessary because a robot that can move and manipulate objects can also injure people, damage property, and destroy itself. Safety systems must detect faults and enter a safe state.

- **Jitter Buffering & Dynamic Path Switching** — Monitors communication quality, applies jitter buffering to smooth packet delay variance, and dynamically switches network paths to mitigate packet loss.
- **Safety-by-Design Controllers** — Safeguarding routines embedded in the motion controller. Detect failures via end-around and wraparound checks.
- **Verifiable Safety & Predictive Foresight** — Game-theoretic and predictive safety approaches providing mathematical guarantees of constraint satisfaction.
- **Robotic Intrusion Prevention Systems (RIPS)** — Detects and mitigates intrusions at the robotic communication level.
- **Space ROS** — Fork of the ROS 2 framework conforming to the ROS 2 API, hardened for safety-critical space robotics applications.

## 13. Regulatory Compliance & Governance

Regulatory compliance and governance software ensures the robot meets legal and safety standards. It is necessary for legal deployment in workplaces and public spaces.

- **vouch-protocol** — Machine-checkable reference profiles for ISO 10218, ISO/TS 15066, EU Machinery Regulation, and UL 3300.
- **Execution Governance for Humanoid Robotics (EG-H)** — Application profile synchronizing evidence across cumulative governance stages.
- **ISO/IEC 42001** — Framework for AI management systems. **RoboSafe** offers character-layer KPIs.
- **Saphira** — Automates HARA, TARA, and FMEA generation for robots, vehicles, and chips.
- **ISO 10218-1:2025** — Revised industrial robot safety standard specifying requirements for inherently safe design.

## 14. Data Privacy & Security

Data privacy and security software protects sensitive data collected by the robot. It is necessary because an avatar that sees, hears, and moves through a workplace collects information that must be protected.

- **DeCloakBrain** — Real-time de-identification, autonomous context awareness, and anomaly detection at the edge.
- **DeCloakFace** — Biometric protection for facial data.
- **DeCloakVision** — Specialized for medical and caregiving environments.
- **CogniCap Studio** — Open-source, locally-run application that preprocesses and anonymizes raw multimodal footage.
- **GDPR Compliance for ROS 2** — Methodologies and software to describe and mitigate the flow of personal data in ROS 2 systems.

## 15. Operator Training & Simulation

Operator training software builds skills in a safe virtual environment before controlling a physical avatar. It is necessary because mistakes on real hardware can be dangerous and expensive.

- **Digital Twin Training Platforms** — VR digital twin systems for training operators on robotic rovers with mechanical arms.
- **Nail It!** — Combines Unity, ROS, and the dVRK for teleoperation data collection and reinforcement learning.
- **Standardized Teleoperator Training Programs** — Focus on spatial awareness, emergency protocols, and human-robot interface proficiency.

## 16. Human Factors & Operator Ergonomics

Human factors software accounts for the operator's cognitive and physical limitations. It is necessary because the human operator is a critical system component, and ignoring their limitations leads to fatigue, errors, and accidents.

- **Cognitive Load & Vigilance Management** — Interface design must account for the vigilance decrement and elevated mental demand from task switching.
- **Shared Control & Assistance Systems** — Interface-aware, task-agnostic assistance systems handle unintended interface operation.
- **CaFe-TeleVision** — Coarse-to-fine immersive situated visualization to enhance ergonomics.
- **Delay-Compensating Interfaces** — Software-user interfaces designed to allow intuitive maneuvering under network delay.

## 17. Social Presence & Affective Expression

Social presence software gives the avatar a face and emotional expression. It is necessary if the avatar interacts with humans naturally, as people respond better to robots that can express emotion and make eye contact.

- **Vizij** — Open-source ecosystem for designing, animating, and deploying rendered robot faces.
- **SentiAvatar** — Real-time 3D avatar capable of conversation, expressive motion, and emotional delivery.
- **EmpaAva** — Open-source, agentic 3D-avatar empathetic chatbot.
- **Animatronic Face Control** — Integrated embedded systems combining OpenCV and MediaPipe FaceMesh for gaze estimation and servo-actuated facial mechanics.

## 18. Data Logging & Learning Pipelines

Data logging pipelines turn every teleoperation session into training data. They are necessary because learning from demonstration requires many episodes of synchronized multimodal data.

- **BEAVR Logger** — Dataset-native logger exporting demonstrations directly in LeRobot schema.
- **MiDAS** — Platform-agnostic system recording synchronized multimodal data during teleoperated surgery.
- **Isaac ROS Unitree G1 Recorder** — Records synchronized teleoperation demonstrations into MCAP rosbags.
- **EVA-Client** — Unified framework for deployment, evaluation, and data collection.
- **Dell/LeRobot MLOps Pipeline** — Automated robotic MLOps pipeline using LeRobot framework.

## 19. Edge Computing & On-Device AI

Edge computing software runs AI models directly on the robot without a GPU or cloud connection. It is necessary for low latency, privacy, and resilience when network connectivity is poor.

- **bitHuman** — Visual SDK for creating avatars running on Arm-based and x86 systems without a GPU.
- **RAVATAR** — Optimized Intel Core Ultra processors and Intel OpenVINO toolkit to run the entire AI Avatar Platform locally.
- **NVIDIA AI Blueprint for Digital Humans** — Deployable as a fully self-contained solution at the edge or scaled on-demand.

## 20. Power & Resource Management

Power management software manages power distribution and thermal constraints. It is necessary because a mobile avatar has finite energy, and inefficient power use shortens runtime.

- **Advanced Power Management** — Manages power distribution, transitions to low-power states, and powers down components.
- **Resource Manager Toolkits** — Manager for physical and virtual resources with graphical programming.
- **Power-Aware Compute Scheduling** — NVIDIA Jetson platforms require power settings configured appropriately.

## 21. Audio Processing & Voice Interaction

Audio processing software gives the avatar a voice and the ability to hear. It is necessary for natural communication with humans.

- **Virtual Human Toolkit** — Integrates audio-visual sensing, ASR, NLP, TTS, and nonverbal behavior generation.
- **@three-ws/voice** — Provides ASR, TTS, and Audio2Face lipsync in one import.
- **@omote/core** — Client-side AI inference for real-time lip sync, speech recognition, and avatar animation.

## 22. Latency Compensation & Time Alignment

Latency compensation software makes the avatar feel responsive despite network delay. It is necessary because even the best network has some latency, and without compensation, teleoperation feels sluggish and unnatural.

- **Lead-Compensate Playback** — Shifts recorded motion earlier per actuator on replay so physical motion lands on time.
- **Avatar Delay Graphs (ADG)** — Formal structure for encoding short idle animation sequences to obscure latencies.
- **Dual-Track Latency-Cover Architecture** — Reframes accumulated latency as a problem of perceived responsiveness.
- **Inverse Dynamics Tracking Control** — Uses Featherstone's algorithm and Newton-Euler formulation to eliminate tracking delays.

## 23. Humanoid-Specific Teleoperation Stacks

Humanoid-specific stacks are bringup packages for whole-body control and teleoperation. They are necessary for humanoid avatars, which have many more degrees of freedom than a simple arm or mobile base.

- **Isaac ROS Physical AI** — Application-level bringup packages for deploying whole-body control and teleoperation on humanoid robots.
- **OpenWBC** — XR-based robot teleoperation and data collection system for Unitree G1.
- **BEAVR** — Open-source, bimanual, multi-embodiment VR teleoperation system.
- **Party OS** — Humanoid system built by RoboParty_Lab. Connects data collection, motion retargeting, imitation learning, and unsupervised learning.

## 24. Simulation-to-Real (Sim2Real) Tools

Sim2Real tools transfer policies trained in simulation to real robots. They are necessary because training in simulation is faster and safer, but policies must work in reality.

- **Isaac Lab** — GPU-accelerated framework for robot learning built on Isaac Sim.
- **Isaac ROS** — Middleware for moving trained policies onto robots.
- **Domain Randomization** — Technique for training policies robust to real-world variation by randomizing simulation parameters.

---

# Part III: The Complete Hardware Stack

## 1. Actuators & Motors

Actuators are the muscles of the robot, converting electrical energy into motion. The choice of actuator determines torque, speed, precision, and backdrivability.

- **RobStride RS 00 / RS 06** — High-performance brushless servo motors. RS 00: 14 N·m max effort. Used in reBot Arm B601-RS. ~$199 kit.
- **Maxon HEJ 90** — Integrated actuator for high-torque joints in mobile and legged robots. Combines brushless motor, gearbox, encoder, servo drive, and thermal monitoring.
- **Myactuator RMD-X15** — DC brushless motor with peak torque of 320 N·m for humanoid robot joints.
- **CubeMars AKH70-16** — Electric servo-actuator. Continuous torque: 26 N·m; peak torque: 85 N·m.
- **CubeMars AK10-9 V2.0** — Robotic actuator with MIT and servo control modes. Peak torque: 38 Nm.
- **Dynamixel XL330** — Compact servo motors. 0.60 N·m torque, 18g weight. ~$25–$40 each.
- **Dynamixel MX-64** — High-torque servo motors. 5.5 N·m torque. ~$200–$300 each.
- **Dynamixel AX-12** — Low-torque servo motors. 1.5 N·m torque. ~$30–$50 each.
- **Hitec HS-7775MG** — Standard servo motors. 9.0 kg-cm torque, 0.10 s/60° speed. ~$50–$80 each.
- **Hitec HSG-5084MG** — Micro servo motors. 1.87 kg-cm torque, 0.05 s/60° speed. ~$30–$50 each.
- **Brushless DC (KY80AS0202)** — 200W consumption. Mobile base drive. ~$100–$200 each.
- **Custom 3D-Printed Cycloidal Actuators** — Modular, 3D-printed cycloidal gearboxes. Part of ~$5,000 Berkeley Humanoid Lite build.
- **Quasi-Direct Drive (QDD) Actuator** — Open-source actuator with 10:1 cycloidal reducer. Max torque: 8.8 Nm.
- **Damiao 4340P Actuator Motor** — Compact, CAN-enabled motor.
- **Moteus** — Open-source brushless servo actuator.
- **YHorizon-JM** — Open-source joint motor with CAN FD support.
- **NASA Penny-Sized 70-W Brushless Servomotor Controllers** — Networked on a bus-topology (CANopen DS-402 protocol).

## 2. Robotic Arms & Manipulators

Robotic arms are the limbs that perform physical work. They determine the robot's reach, payload, and manipulation capability.

- **reBot Arm B601-RS** — 6-DoF + gripper. 5 kg payload. 754 mm reach. Fully open-source. ±0.1 mm precision. ~$199 kit.
- **SO-ARM101 Pro** — 6-DoF, fully open-source. LeRobot-compatible. ~$100–$200.
- **OpenArm 1** — 8-DOF arm, $5,400 assembled or open-source BOM.
- **OpenEAI-Arm** — 6-DoF, fully open-source. ~$200–$400.
- **EduSCARA** — RRPR SCARA, fully open-source, 3D-printable. ~$150–$300.
- **Trossen WidowX 250** — 6-DoF commercial arm. ALOHA reference arm. ~$4,000 each.
- **ALOHA / Mobile ALOHA** — Bimanual teleoperation platform, full design published.
- **NexArm** — Open-source embodied AI robotic arm with ESP32 and AT32 dual-chip design. ~$639.99.
- **ROBOTIS** — 5 DOF + 1 gripper. Joint resolution: -π to π rad.

## 3. Dexterous Hands & Grippers

Dexterous hands and grippers allow the robot to manipulate objects with human-like dexterity. They are necessary for tasks that require fine motor skills.

- **ORCA Hand** — 17-DoF tendon-driven anthropomorphic hand with integrated tactile sensors. <2,000 CHF materials.
- **Shadow Dexterous Hand** — 24-DoF tendon-driven anthropomorphic hand. Fully open-source CAD, control software, firmware.
- **LEAP Hand** — 16-DOF dexterous hand, ~$2,000 BOM.
- **ISyHand** — Multi-finger, on-joint servo-driven. Open-source, low-cost.
- **CRAFT Hand** — Hybrid hard/soft materials, 3D-printed, human-like dexterity.
- **Aero Hand** — Open-source hand from Chestnut Robotics. 3D-printed.
- **PincOpen Gripper** — Open-source, cost-effective parallel gripper.
- **Robotiq 2F-85 Gripper** — 85mm stroke, 2.5kg payload. $5,825.
- **Belt-Finger** — Affordable soft belt-driven gripper for dexterous in-hand manipulation.
- **OnRobot 2FG7** — Compact finger gripper for flexible pre-assembly tasks.

## 4. Chassis, Frames & Structural Components

Chassis and frames are the structural elements that hold everything together. They determine the robot's shape, strength, and weight.

- **FDM 3D Printing** — PLA, PETG, TPU, ABS, CFRP. Precision: ±0.1–0.5 mm. Printer ~$300–$1,500.
- **SLA/DLP 3D Printing** — Photosensitive resin. High resolution. Printer ~$200–$1,000.
- **SLS 3D Printing** — Nylon PA12, glass-filled nylon. Dimensional accuracy. Industrial service.
- **CNC Machining** — Aluminum 6061-T6, 7075. Precision: ±0.003 mm. Service-based.
- **Continuous-Fiber CFRP (FDM)** — Carbon-fiber-reinforced polymer. Industrial.
- **Sheet Metal** — Aluminum, steel. Brackets, mounting plates, enclosures. Service-based.

## 5. Locomotion Systems

Locomotion systems determine how the robot moves through its environment. They are necessary for mobility.

- **Mecanum Wheels** — Omnidirectional wheels with 45° angled rollers. Full planar omnidirectionality.
- **Omni-Directional Base (ALOHA-style)** — Adds $10k–$15k for Mobile ALOHA base.
- **NimbRo Omnidirectional Base** — Four 8-inch mecanum wheels, RMD-X8 brushless motors.
- **LeKiwi Holonomic Base** — 3-wheel Kiwi drive with omni wheels.
- **Bipedal Legs** — LeRobot Humanoid ($2,500); Berkeley Humanoid Lite ($5,000).
- **Drone Propulsion** — PX4/ArduPilot and platforms like Mercury/MIRA.

## 6. Sensors & Perception Hardware

Sensors give the robot information about its own state and its environment. They are the foundation of feedback control and perception.

- **Intel RealSense D455** — RGB-D camera for depth acquisition. ~$400–$500.
- **Intel RealSense D435** — Depth camera. ~$300–$400.
- **Intel RealSense D405** — Depth camera, ALOHA reference camera. ~$300–$400.
- **Logitech Brio 4K** — USB camera. ~$150–$200.
- **ST/Leopard Imaging Multi-Sensor Module** — Integrates RGB-IR image sensor, dToF LiDAR, and 6-axis IMU.
- **LiDAR** — Laser scanner for navigation and obstacle detection. Centimeter-level.
- **IMU (Inertial Measurement Unit)** — Motion sensing fused with cameras for VINS-Fusion.
- **Tactile Sensors** — Touch sensing for dexterous manipulation feedback.
- **Force/Torque Sensors** — Force measurement for haptic feedback and safety.
- **SOS LAB Multimodal Platform** — Integrates 3D LiDAR, RGB camera, thermal imaging camera, and IMU.

## 7. Edge AI Compute Modules

Edge AI compute modules run perception, control, and AI inference onboard the robot. They are necessary for low latency and resilience.

- **NVIDIA Jetson Thor** — 2070 FP4 TFLOPS AI compute within a 130 W power envelope.
- **NVIDIA Jetson Orin Nano 2** — 78 TOPS AI compute, 8 GB memory.
- **NVIDIA Jetson Orin Nano SUPER** — Gesture recognition, YOLO inference, SLAM. ~$200–$300.
- **Raspberry Pi 5 + Hailo-8L** — 13 TOPS, 1.5–2.5W peak AI inference. ~$180–$200 total.
- **Raspberry Pi Zero 2W** — Low-power module. ~$15.
- **LattePanda 3 Delta** — Core of the REGO avatar robot. ~$200–$300.
- **RK3588 Industrial Motherboard** — Used in quadruped robot systems.
- **Teensy 4.0** — Motor control and PID loops. ~$25.

## 8. Power Systems

Power systems supply energy to all components. They determine runtime, peak power, and safety.

- **Akku Vision 48V NMC** — Lithium-ion battery. 25 Ah / 1,170 Wh. Industrial AMRs and drones.
- **Akku Vision 48V LFP** — Lithium iron phosphate battery. 20 Ah / 1,024 Wh. Long-life industrial applications.
- **Diamond 350Wh/kg Semi-Solid Li-Ion** — Semi-solid lithium battery. Extreme environments, long-endurance UAVs.
- **ESOX Solid-State Batteries** — Solid-state batteries with modular battery bays.
- **Factorial Solid-State** — Solid-state batteries for drones and mobile robotics.
- **MANLY Battery 2026 Program** — Humanoid robot battery packs.

## 9. Communication Modules

Communication modules connect the robot to the operator and the cloud. They are necessary for remote control.

- **olixLink C1** — Industrial 5G/WiFi6 RCU. Dual-SIM, VPN-integrated, rugged IP66.
- **Quectel RM500Q-GL** — 5G modem for multi-connectivity industrial automation.
- **Quectel 5G Series** — Millisecond-level response and AI compute support.
- **Fibocom FM160** — 5G module powered by Qualcomm Snapdragon X62.
- **5G RedCap Module** — Ultra-small (52×30×2.3mm), power consumption <2W.
- **Intel Wi-Fi 6 / MT7921K Wi-Fi 6E** — WiFi modules for local high-bandwidth access.
- **China Mobile "Lingtong" Board** — 5G + WiFi + Bluetooth three-in-one.
- **Quectel SH602HA-AP** — Smart robotic module with plug-in approach.

## 10. Operator Interface Hardware

Operator interface hardware is what the human uses to control the robot. It determines how intuitive and immersive teleoperation feels.

- **Meta Quest 3** — VR headset. Hand pose and button events mapped to end-effector. ~$500.
- **Apple Vision Pro** — XR headset for OpenWBC teleoperation. ~$3,500.
- **Pico Headset** — VR headset supporting OpenArmX VR teleoperation. ~$400–$800.
- **AgiBot A2 Teleoperation Kit** — Pico VR headset interface for AgiBot A2.
- **MANUS Gloves** — Motion-capture gloves. ~$8,000–$15,000.
- **Rokoko Gloves** — Motion-capture gloves. ~$8,000–$15,000.
- **Haply Inverse3 / VerseGrip** — Haptic devices. ~$2,000–$5,000.
- **DOGlove** — Low-cost, open-source haptic glove.
- **TriPilot-FF Pedal** — Foot-operated pedal with lidar-driven haptic feedback.
- **Workstation PC (RTX 4090, 64 GB RAM)** — Control workstation. ~$3,500–$5,000.

## 11. Wiring, Connectors & Assembly Hardware

Wiring and assembly hardware connects and mounts all components. It is necessary for reliable electrical and mechanical integration.

- **80/20 Aluminum Extrusion Frame** — Used in ALOHA rigs. ~$400–$800 per set.
- **Powered USB 3 Hub** — 10+ port hub. ~$80.
- **Task-Surface Table** — 36×24 inch table. ~$150–$400.
- **Cabling, Zip Ties, Mounting Plates** — ~$200–$400.
- **M5 Bolts** — Used for armoring PLA structures.
- **Custom PCB Design** — For integrated control boards.
- **3D-Printed Cycloidal Gearbox** — Open-source project with print parts and assembly instructions.
- **SPARC Spine Module** — Compact, open-source 3-DoF sagittal-plane spine module.

---

