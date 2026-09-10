# A.D.A.M. v0.1 (Infrastructure-Independent Distributed Search Modules) 🤖

**A.D.A.M. v0.1** is a low-cost, mobile robotic sensor platform designed as a "forward eye" for search and rescue operations in disaster zones where critical infrastructure (GSM, GPS, Internet) has collapsed. This project represents the first hardware iteration (v0.1) of the "Soldier" mobile units, aimed at establishing a robust Proof-of-Concept (PoC) for life detection in hazardous and inaccessible environments.

---

### ⚠️ Project Status

| Parameter | Current Status |
| :--- | :--- |
| **Program** | TÜBİTAK 2209-A University Students Research Projects Support Program |
| **Stage** | Application / Approval Phase |
| **Funding Status** | Not yet officially funded; currently at the conceptual and preliminary prototyping stage. |

---

### 🌟 Key Features & Sensor Fusion

The platform utilizes a multi-modal sensor architecture to detect signs of life and navigate through debris:

| Sensor | Function | Technical Detail |
| :--- | :--- | :--- |
| **Thermal Camera** | Life Detection | MLX90640 for heat signature and hotspot identification. |
| **Microphone** | Acoustic Detection | Capturing cries for help or structural vibrations. |
| **ToF Laser** | Spatial Awareness | VL53L0X for active collision avoidance and ranging. |
| **IMU** | Stability | MPU-6050 for monitoring robot balance and orientation. |

---

### 🎯 Success Criterion: The "Blind Operator" Test

The success of the platform is validated through a specialized test scenario where the operator must complete a mission without a direct line-of-sight:

* **Scenario:** The robot is placed in an obstacle course outside the operator's visual field.
* **Navigation:** The operator guides the unit solely through real-time ToF distance data.
* **Detection:** The mission is considered successful once the hidden heat source (Thermal) and sound source (Acoustic) are identified and reported to the command center.

---

### 🏗️ System Architecture (v0.1)

The v0.1 iteration focuses on a wired communication backbone to ensure reliability in radio-silent or interference-heavy environments:

| Component | Detail |
| :--- | :--- |
| **Processor** | STM32 Microcontroller (Programmed with Embedded C/C++). |
| **Locomotion** | Tracked chassis for superior maneuverability over rubble and uneven terrain. |
| **Connectivity** | Wired serial communication (USB-UART) to a Command Center terminal. |
| **Monitoring** | Real-time terminal interface displaying thermal hotspots, distance, and audio levels. |

---

### 🗺️ Future Roadmap

| Version | Objective | Core Technology Focus |
| :--- | :--- | :--- |
| **v1.0 - v2.0** | Wireless Operations | Self-healing LoRa Mesh network architecture. |
| **v3.0** | Swarm Intelligence | Autonomous swarm algorithms and obstacle avoidance. |
| **v4.0** | Advanced Mobility | Dynamic chassis upgrades for complex vertical debris climbing. |

---

### 🛠️ Hardware Inventory

| Component | Role |
| :--- | :--- |
| **ST Nucleo-F411RE** | Main Control Unit (Processor) |
| **MLX90640** | Thermal Imaging Sensor |
| **VL53L0X** | Time-of-Flight (ToF) Laser Distance Sensor |
| **MPU-6050** | 6-Axis Accelerometer and Gyroscope (IMU) |
| **KY-038** | Audio Detection Sensor (Microphone) |
| **3S Lipo Battery** | 11.1V System Power Supply |

---

### 🛡️ Ethical & Safety Standards

* **Occupational Safety:** Development follows strict protocols, including the use of safety goggles and gloves during soldering, along with active workspace ventilation.
* **Social Impact:** Designed with a "human-first" philosophy to enable rescue teams (AFAD, Firefighters, etc.) to gather data during "golden hours" without risking their own lives.
* **Data Ethics:** Sensor data is processed strictly for life-saving purposes, adhering to ethical boundaries in autonomous decision-making.
