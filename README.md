<div align="center">
  
<img src="docs/Banner.png" width="150%"/>

# Hi, I'm Salvatore 

<img src="https://readme-typing-svg.demolab.com/?font=Fira+Code&size=20&pause=1000&color=2F80ED&center=true&vCenter=true&width=600&lines=Robotics/Automation+Engineering+Master+Student;Autonomous+Systems+%26+Robotics;Formula+Student+Driverless+%40+UniNa+Corse" alt="Typing SVG" />

[![UniNa Corse](https://img.shields.io/badge/UniNa_Corse-Autonomous_System-b30000?style=for-the-badge&logo=ros&logoColor=white)](https://uninacorse.com/)
[![Federico II](https://img.shields.io/badge/Federico_II-Naples_Italy-003366?style=for-the-badge)](https://www.unina.it/)

</div>

---

## About Me

I'm a Robotics/Automation Engineering Master student at the **Università degli Studi di Napoli Federico II**. I'm part of the **Autonomous System** division of **[UniNa Corse](https://uninacorse.com/)**, the university's official Formula Student racing team, which competes internationally in the Driverless class and has multiple podium finishes to its name.

My project work spans autonomous perception & localization, systems and network programming, software engineering, and control theory — with a growing focus on **SLAM, sensor fusion, and real-time robotics software**.

---

## 🏎️ Currently Building

A **multi-pipeline SLAM stack** for UniNa Corse's driverless race car, benchmarking three parallel approaches to cone-based track localization:

- **Graph SLAM** with an **iSAM2** backend for incremental factor-graph optimization

## Architettura del Sistema

```
┌─────────────────────────────────────────────────────────────┐
│                        I/O THREAD                           │
│  (ROS2 MultiThreadedExecutor — callbacks ROS2)              │
│                                                             │
│   ZED Callback ──┐                                          │
│   LiDAR Callback─┼──► iotofe_landmarks_queue (SPSC)         │
│   Odom Callback ─┘──► iotofe_pose_queue      (SPSC)         │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│               FRONT-END THREAD  (~400 Hz)                   │
│  (pthread POSIX — priorità SCHED_FIFO 80)                   │
│                                                             │
│   • Temporal Sync ZED/LiDAR ↔ Odom (buffer + match)         │
│   • Sensor Fusion ZED + LiDAR (merge frame)                 │
│   • Dead Reckoning (propagazione covarianza EKF-like)       │
│   • Data Association (Mahalanobis + fallback euclideo)      │
│   • Waiting List (soglia N osservazioni prima di mappare)   │
│                                                             │
│   ──► fetobe_queue (SPSC) ──► Back-End                      │
│   ◄── betofe_updates_queue (SPSC) ◄── Back-End              │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│               BACK-END THREAD  (~40 Hz)                     │
│  (pthread POSIX — priorità SCHED_FIFO 40)                   │
│                                                             │
│   • iSAM2 (fattorizzazione Cholesky, relinearize skip)      │
│   • BetweenFactor (odometria con Huber robust noise)        │
│   • BearingRangeFactor (ZED σ_b=0.20, σ_r=0.80)             │
│   • BearingRangeFactor (LiDAR σ_b=0.02, σ_r=0.05)           │
│   • Estrazione covarianza marginale pose + landmarks        │
│                                                             │
│   ──► betofe_updates_queue (SPSC) ──► Front-End             │
│   ──► betoio_pose_queue    (SPSC) ──► I/O                   │
│   ──► betoio_landmarks_queue (SPSC) ──► I/O                 │
└─────────────────────────────────────────────────────────────┘
```


## Presentazione del Sistema

<p align="center">
  <img src="docs/Schermata del 2026-06-29 12-27-36.png" width="120%"/>
</p>

System validation is carried out through **ROS bag playback in Foxglove Studio**, with a focus on landmark/cone position estimation, data association stability, camera–LiDAR extrinsic calibration, and loop-closure quality.

---

## 🛠️ Tech Stack

**Languages**

![C++](https://img.shields.io/badge/C%2B%2B-00599C?style=flat-square&logo=cplusplus&logoColor=white)
![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Java](https://img.shields.io/badge/Java-007396?style=flat-square&logo=openjdk&logoColor=white)
![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat-square&logo=matlab&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)

**Robotics & Autonomous Systems**

![ROS2](https://img.shields.io/badge/ROS_2-22314E?style=flat-square&logo=ros&logoColor=white)
![SLAM](https://img.shields.io/badge/SLAM-Graph_SLAM_%7C_iSAM2_%7C_EKF-orange?style=flat-square)
![Sensor Fusion](https://img.shields.io/badge/Sensor_Fusion-LiDAR_%7C_Camera-orange?style=flat-square)
![Foxglove](https://img.shields.io/badge/Foxglove_Studio-6E56CF?style=flat-square)

**Backend, Networking & Middleware**

![gRPC](https://img.shields.io/badge/gRPC-4285F4?style=flat-square)
![Flask](https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)
![Messaging](https://img.shields.io/badge/Messaging-ActiveMQ_%7C_STOMP-C13930?style=flat-square)
![Protocols](https://img.shields.io/badge/Protocols-WebSockets_%7C_HTTP%2F2-666666?style=flat-square)

**Software Engineering**

![UML](https://img.shields.io/badge/UML-Visual_Paradigm-blue?style=flat-square)
![Design Patterns](https://img.shields.io/badge/Design_Patterns-BCE_%7C_DAO-blue?style=flat-square)
![JUnit5](https://img.shields.io/badge/JUnit5-25A162?style=flat-square&logo=junit5&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat-square&logo=git&logoColor=white)

**Systems & Control**

![Linux](https://img.shields.io/badge/Linux%2FUbuntu-FCC624?style=flat-square&logo=linux&logoColor=black)
![Real-Time](https://img.shields.io/badge/Real_Time_Systems-333333?style=flat-square)
![Control Theory](https://img.shields.io/badge/Control_Theory-Multivariable_%26_State_Space-333333?style=flat-square)

---

## 📌 Featured Projects

| Repository | Description | Stack |
|---|---|---|
| **[ROS2_Practice_UninaCorse_AS](https://github.com/salvatore202/ROS2_Practice_UninaCorse_AS)** | ROS 2 foundations and hands-on exercises built for the Autonomous System division of UniNa Corse, ahead of the team's driverless perception & SLAM stack. | `Python` `ROS 2` |
| **[GLAM](https://github.com/salvatore202/GLAM)** | Full-lifecycle OOP system built for a Software Engineering exam: UML modeling in Visual Paradigm, BCE architecture, DAO-based MySQL persistence, and a JUnit 5 test suite. | `Java` `MySQL` `JUnit 5` `UML` |
| **[Real-Time-filtering](https://github.com/salvatore202/Real-Time-filtering)** | Real-time signal filtering assignment for the Real-Time Systems course, focused on deterministic execution and timing guarantees. | `C` `Python` `Shell` |
| **[Network-programming-and-middellware-technologies](https://github.com/salvatore202/Network-programming-and-middellware-technologies)** | Distributed-systems exercises: TCP/UDP sockets, gRPC services, STOMP/MOM messaging over ActiveMQ, and a Flask + MongoDB backend. | `Python` `gRPC` `Flask` `MongoDB` |
| **[Uninformed-Search-Algorithms-Analysis](https://github.com/salvatore202/Uninformed-Search-Algorithms-Analysis)** | Comparative analysis of BFS, DFS, Dijkstra, DLS and IDS — completeness, complexity, runtime and memory profiling on a real road-network graph. | `Python` |
| **[Advanced-and-Multivariable-Control](https://github.com/salvatore202/Advanced-and-Multivariable-Control)** | Exercises and exam material for the Advanced & Multivariable Control course. | `MATLAB` |
| **[Cpp](https://github.com/salvatore202/Cpp)** | OOP and data-structure fundamentals — linked lists, stacks, queues, priority queues, and classic algorithm exercises. | `C++` |
| **[Queens](https://github.com/salvatore202/Queens)** | Two algorithmic solutions to the N-Queens problem (array-based and stack/list-based). | `C++` |

---

## 📊 GitHub Stats

<p align="center">
  <img src="https://github-readme-stats.vercel.app/api?username=salvatore202&show_icons=true&theme=default&hide_border=true&count_private=false" width="48%" alt="Salvatore's GitHub stats" />
  <img src="https://github-readme-stats.vercel.app/api/top-langs/?username=salvatore202&layout=compact&theme=default&hide_border=true" width="48%" alt="Top languages" />
</p>

---

## 📫 Let's Connect

- 📧 Email: `salvatoreraiola2002@gmail.com`
- 💼 LinkedIn: `https://it.linkedin.com/in/salvatore-raiola-0b7329363`

> *(placeholders — swap in your real contact details)*

---

<div align="center">
<sub>Autonomous systems, robotics, and clean engineering.</sub>
</div>
