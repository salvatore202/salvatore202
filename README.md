<div align="center">
  
<img src="docs/Banner_mod_2.png" width="150%"/>

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

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                         I/O THREAD                          │
│       (ROS2 MultiThreadedExecutor — ROS2 callbacks)         │
│                                                             │
│   ZED Callback ──┐                                          │
│   LiDAR Callback─┼──► iotofe_landmarks_queue (SPSC)         │
│   Odom Callback ─┘──► iotofe_pose_queue      (SPSC)         │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                FRONT-END THREAD  (~400 Hz)                  │
│          (POSIX pthread — SCHED_FIFO priority 80)           │
│                                                             │
│   • ZED/LiDAR ↔ Odom Temporal Sync (buffer + match)         │
│   • ZED + LiDAR Sensor Fusion (frame merging)               │
│   • Dead Reckoning (EKF-like covariance propagation)        │
│   • Data Association (Mahalanobis + Euclidean fallback)     │
│   • Waiting List (N observations threshold before mapping)  │
│                                                             │
│   ──► fetobe_queue (SPSC) ──► Back-End                      │
│   ◄── betofe_updates_queue (SPSC) ◄── Back-End              │
└─────────────────────────────────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│                 BACK-END THREAD  (~40 Hz)                   │
│          (POSIX pthread — SCHED_FIFO priority 40)           │
│                                                             │
│   • iSAM2 (Cholesky factorization, relinearization skip)    │
│   • BetweenFactor (odometry with Huber robust noise)        │
│   • BearingRangeFactor (ZED σ_b=0.20, σ_r=0.80)             │
│   • BearingRangeFactor (LiDAR σ_b=0.02, σ_r=0.05)           │
│   • Pose + landmarks marginal covariance extraction         │
│                                                             │
│   ──► betofe_updates_queue (SPSC) ──► Front-End             │
│   ──► betoio_pose_queue       (SPSC) ──► I/O                │
│   ──► betoio_landmarks_queue  (SPSC) ──► I/O                │
└─────────────────────────────────────────────────────────────┘
```


## System Overview

<p align="center">
  <img src="docs/Schermata del 2026-06-29 12-27-36.png" width="120%"/>
</p>

System validation is carried out through **ROS bag playback in Foxglove Studio**, with a focus on landmark/cone position estimation, data association stability, camera–LiDAR extrinsic calibration, and loop-closure quality.

---

## 🛠️ Tech Stack

**Languages**

<p>
<img src="https://cdn.simpleicons.org/cplusplus/00599C" height="28" align="middle" alt="C++" />&nbsp;<b>C++</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/c/659AD2" height="28" align="middle" alt="C" />&nbsp;<b>C</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/python/3776AB" height="28" align="middle" alt="Python" />&nbsp;<b>Python</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/openjdk/ED8B00" height="28" align="middle" alt="Java" />&nbsp;<b>Java</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/matlab/0076A8" height="28" align="middle" alt="MATLAB" />&nbsp;<b>MATLAB</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/gnubash/4EAA25" height="28" align="middle" alt="Bash" />&nbsp;<b>Bash</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/mysql/4479A1" height="28" align="middle" alt="MySQL" />&nbsp;<b>MySQL</b>
</p>

**Robotics & Autonomous Systems**

<p>
<img src="https://cdn.simpleicons.org/ros/6C8EBF" height="28" align="middle" alt="ROS 2" />&nbsp;<b>ROS 2</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/ubuntu/E95420" height="28" align="middle" alt="Linux / Ubuntu" />&nbsp;<b>Linux / Ubuntu</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/git/F05032" height="28" align="middle" alt="Git" />&nbsp;<b>Git</b>
</p>

![Graph SLAM](https://img.shields.io/badge/Graph_SLAM-DCE1E7?style=flat-square)
![iSAM2](https://img.shields.io/badge/iSAM2-DCE1E7?style=flat-square)
![EKF](https://img.shields.io/badge/EKF-DCE1E7?style=flat-square)
![Sensor Fusion](https://img.shields.io/badge/Sensor_Fusion-DCE1E7?style=flat-square)
![LiDAR & Camera](https://img.shields.io/badge/LiDAR_%26_Camera-DCE1E7?style=flat-square)
![Foxglove Studio](https://img.shields.io/badge/Foxglove_Studio-DCE1E7?style=flat-square)

**Backend, Networking & Middleware**

<p>
<img src="https://cdn.simpleicons.org/flask/7D8590" height="28" align="middle" alt="Flask" />&nbsp;<b>Flask</b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
<img src="https://cdn.simpleicons.org/mongodb/47A248" height="28" align="middle" alt="MongoDB" />&nbsp;<b>MongoDB</b>
</p>

![gRPC](https://img.shields.io/badge/gRPC-DCE1E7?style=flat-square)
![ActiveMQ](https://img.shields.io/badge/ActiveMQ-DCE1E7?style=flat-square)
![STOMP](https://img.shields.io/badge/STOMP-DCE1E7?style=flat-square)
![WebSockets](https://img.shields.io/badge/WebSockets-DCE1E7?style=flat-square)
![HTTP/2](https://img.shields.io/badge/HTTP%2F2-DCE1E7?style=flat-square)

**Software Engineering & Control**

![UML](https://img.shields.io/badge/UML-DCE1E7?style=flat-square)
![Design Patterns](https://img.shields.io/badge/Design_Patterns-DCE1E7?style=flat-square)
![JUnit 5](https://img.shields.io/badge/JUnit_5-DCE1E7?style=flat-square)
![Real-Time Systems](https://img.shields.io/badge/Real--Time_Systems-DCE1E7?style=flat-square)
![Control Theory](https://img.shields.io/badge/Control_Theory-DCE1E7?style=flat-square)

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

<div align="center">
  <img src="https://github-profile-summary-cards.vercel.app/api/cards/profile-details?username=salvatore202&theme=transparent" alt="Profile details" />
  <br />
  <img src="https://github-profile-summary-cards.vercel.app/api/cards/stats?username=salvatore202&theme=transparent" alt="GitHub stats" /><img src="https://github-profile-summary-cards.vercel.app/api/cards/repos-per-language?username=salvatore202&theme=transparent" alt="Repos per language" />
</div>

---

## 📫 Let's Connect

- 📧 Email: `salvatoreraiola2002@gmail.com`
- 💼 LinkedIn: `https://it.linkedin.com/in/salvatore-raiola-0b7329363`


---

<div align="center">
<sub>Autonomous systems, robotics, and clean engineering.</sub>
</div>
