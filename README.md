# 🤖 Design and Implementation of a Novel Grasping System for a Humanoid Robot  

![License](https://img.shields.io/badge/license-MIT-green.svg)  
![Platform](https://img.shields.io/badge/platform-Pepper%20Robot-blue.svg)  
![Language](https://img.shields.io/badge/language-Python-yellow.svg)  
![Vision](https://img.shields.io/badge/CV-YOLO%20%7C%20DepthAnything-red.svg)  

---

## 📌 Overview  
This repository presents the **Master’s Thesis Project**:  
**“Design and Implementation of a Novel Grasping System for a Humanoid Robot”**,  
developed to enable **Pepper**, a humanoid robot by SoftBank Robotics, to **grasp unknown objects** without relying on external markers or additional sensors.  

The system combines **state-of-the-art computer vision** with **robotic motion planning**:  
- 🟦 **YOLO (You Only Look Once)** → real-time object detection.  
- 🟩 **Depth Anything** → monocular depth estimation for 3D perception.  
- 🟥 **Inverse Kinematics** → precise arm movement planning for grasp execution.  

Unlike traditional approaches, this work **excludes the use of fiducial markers, external depth sensors, or motion trackers**, relying solely on Pepper’s built-in cameras and actuators.  

---

## 🎯 Objectives  
- ✅ Enable Pepper to **detect and localize unknown objects** in its environment.  
- ✅ Estimate **distance and depth** using monocular vision.  
- ✅ Compute **inverse kinematics** to position the end-effector.  
- ✅ Execute grasping using Pepper’s **limited hand control** (open/close).  
- ✅ Achieve the above **within Pepper’s hardware and software constraints**.  

---

## 🦾 Background & Motivation  
Humans rely on **sight, touch, and proprioception** for grasping. Robots, however, depend on **cameras and sensors**. Grasping is particularly challenging for humanoid robots, where constraints in hardware and control limit precision.  

Humanoid robots are designed to **support or replace human tasks** in various fields, from assistance to entertainment.  
We distinguish between:  
- 🚶 **Biped humanoids** – full-body robots with arms and legs.  
- 🛞 **Upper-body mobile humanoids** – wheeled robots with torso, head, and arms.  

👉 Pepper belongs to the **second category**.  

---

## 🤖 Pepper Robot – Technical Specifications  
Pepper is a **humanoid robot** developed by **SoftBank Robotics** (formerly Aldebaran Robotics).  

### 📏 Physical Characteristics  
- Height: ~1.20 m  
- Mobility: **3-wheeled omnidirectional base**  
- Degrees of Freedom (DoF): **20 total**, with **5 per arm**  

### 👀 Vision System  
- 2 × RGB cameras (**640×480**) → located in mouth & forehead  
- Optional: stereo cameras or 3D depth sensor (our version: **stereo cameras**)  
- Frame rate: **15 fps @ 10 Hz** (non-simultaneous usage of the two RGB cameras)  

### 🖐 Hand & Sensors  
- Fingers: **cannot move individually** → hand is only fully open/closed  
- Max payload: **200 g**  
- Capacitive sensors → **upper part of the hands only**  

### 🧠 Embedded System  
- OS: **NAOqi 2.9**  
- Processor: **low-performance CPU**  
- Built-in gyroscopes & lasers (not used in this project)  
- Depth map reconstruction from stereo → **not precise with NAOqi API**  

---

## ⚙️ System Architecture  

```mermaid
flowchart TD
    A[RGB/ Stereo Cameras] -->|YOLO| B[Object Detection]
    B -->|Bounding Box| C[Depth Anything]
    C -->|Depth Estimation| D[3D Object Localization]
    D -->|IK Solver| E[Inverse Kinematics]
    E -->|Joint Angles| F[Pepper Arm Control]
    F --> G[Grasp Execution (Open/Close Hand)]

```
