[中文版](README.md) | English
# DengFOC Minimalist FOC Library -- Open Source

# For course materials and library documentation, visit: dengfoc.com

- [DengFOC Minimalist FOC Library -- Open Source](#dengfoc-minimalist-foc-library----open-source)
- [For course materials and library documentation, visit: dengfoc.com](#for-course-materials-and-library-documentation-visit-dengfoccom)
  - [1 Overview](#1-overview)
  - [2 Important Links](#2-important-links)
  - [3 Community](#3-community)
  - [4 How to Write DengFOC Code](#4-how-to-write-dengfoc-code)
    - [4.1 Motor Force and Position Closed-Loop (Based on Current Torque Loop)](#41-motor-force-and-position-closed-loop-based-on-current-torque-loop)
  - [5 Free FOC Algorithm Principle Course](#5-free-foc-algorithm-principle-course)
  - [6 DengFOC Hobbyist Extension Support](#6-dengfoc-hobbyist-extension-support)

## 1 Overview

The DengFOC library, developed by Deng, is an open-source FOC library based on Arduino and ESP32 hardware.

![mainTitle](pic/mainTitle.png)

This FOC library is partially inspired by SimpleFOC but emphasizes minimalism and practicality, with simpler yet richer features for secondary development and application compared to SimpleFOC:

1. **Low resource usage**: Uses 2/3 less memory than SimpleFOC.
2. **High openness**: Provides basic FOC algorithm interfaces (such as electrical angle, Ialpha, and Ibeta) in a highly accessible manner, allowing users to directly engage with the entire FOC algorithm implementation, which facilitates learning and further algorithm development.
3. **Strong external connectivity**: Supports direct integration and cross-calling with various hardware and software, including OpenMV, Raspberry Pi, and Python, enabling faster development of BLDC motor applications compared to SimpleFOC.
4. **Plug-and-play without calibration**: Advanced parameter self-recognition feature allows users to run FOC without configuring any parameters, simply by plugging in the motor and encoder.
5. **Wireless control support**: High-speed UDP and ESPNow communication enables motor control without signal cables.
6. **Script support**: The library includes a powerful Lua scripting language, allowing rapid FOC application development without compilation.
7. **Robust toolchain support**: Enables direct communication with systems like Matlab, Simulink, and ROS, facilitating quick setup for robotic applications.
8. **High-precision reducer support**: Supports dual-encoder reducer applications.

## 2 Important Links

The DengFOC library, combined with DengFOC hardware, provides a complete FOC motor control solution. Key resources:

1. [Deng Open Source Taobao Shop -- One-stop shop for DengFOC boards](https://shop564514875.taobao.com/): Your support enables us to continue creating open-source content and courses. Project revenue will be used for further DengFOC development and courses.
2. [DengFOC Hardware on Github](https://github.com/ToanTech/Deng-s-foc-controller)
3. [DengFOC Official Website](http://dengfoc.com/#/): Contains text-based course notes, DengFOC documentation, and library usage instructions.

## 3 Community

The DengFOC community is primarily active on QQ groups. Feel free to join: **Open Source FOC BLDC Driver Community -- Deng Open Source** with group numbers 778255240 (Group 1), 735755513 (Group 2), 471832283 (Group 3), 834835665 (Group 4), and 247431752 (Group 5).

All usage and DIY questions will be directly discussed and answered in these groups.

## 4 How to Write DengFOC Code

Deng designed the DengFOC library with minimalism and low memory usage in mind. Therefore, all functional blocks are based on the principle of **use when needed, exclude when not**, to achieve minimal resource usage.

Currently available modules:

1. DengFOC Core Library -- Contains DengFOC core functions
2. AS5600 Encoder Library -- Sensor library
3. InlineCurrent Library -- INA240 current sensor library
4. Low-Pass Filter Library

Currently implemented features:

1. Open-loop speed
2. Closed-loop speed
3. AS5600 sensor reading
4. Online current detection
5. Angle closed-loop (force-position closed-loop)
6. Force-angle closed-loop (position + speed + force closed-loop)
7. Current-torque closed-loop
8. Dual-motor full three-loop drive (position, speed, torque)

Below is an example of DengFOC library code for force + position closed-loop (force-position closed-loop with current-torque loop as the inner loop and position as the outer loop). For more details on library functions, please visit the [DengFOC website](dengfoc.com).

### 4.1 Motor Force and Position Closed-Loop (Based on Current Torque Loop)
```c++
#include "DengFOC.h"

int Sensor_DIR = -1;    // Sensor direction
int Motor_PP = 7;       // Motor pole pairs

void setup() {
  Serial.begin(115200);
  DFOC_Vbus(12.6);   // Set the supply voltage for the driver
  DFOC_alignSensor(Motor_PP, Sensor_DIR);  // Align the sensor with motor pole pairs and direction
}

void loop() 
{
  // Set PID parameters
  DFOC_M0_SET_ANGLE_PID(0.5, 0, 0.003, 100000, 0.1);   // Angle PID
  DFOC_M0_SET_CURRENT_PID(1.25, 50, 0, 100000);        // Current loop PID
  DFOC_M0_set_Force_Angle(serial_motor_target());      // Force-position control command

  // Receive commands from serial input
  serialReceiveUserCommand();
}

```

## 5 Free FOC Algorithm Principle Course

To help everyone gain a deeper understanding of the FOC algorithm and principles, I've created a step-by-step FOC algorithm course. Through this course, you can quickly understand FOC from a theoretical perspective and even try writing your own basic FOC library. Video links:

1 [FOC Algorithm Basics 1: Origins, Brushless Motor Concepts, and Control Principles](https://www.bilibili.com/video/BV1dy4y1X7yx)

2 [FOC Algorithm Basics 2: Clarke Transformation and Simplified Motor Mathematical Model](https://www.bilibili.com/video/BV1x84y1V76u/)

3 [FOC Algorithm Basics 3: Equal Magnitude Transformation and Inverse Clarke Transformation](https://www.bilibili.com/video/BV13s4y1Z7Tg/)

4 [FOC Algorithm Basics 4: Park Transformation](https://www.bilibili.com/video/BV1t24y1u7do/)

5a. [FOC Algorithm Basics 5a: Prerequisites for Open-Loop Speed Code](https://www.bilibili.com/video/BV1Pc411s7mP/)

5b. [FOC Algorithm Basics 5b: Open-Loop Speed Code Programming + Hardware Debugging Tutorial](https://www.bilibili.com/video/BV16X4y167XZ/)

6a. [FOC Algorithm Basics 6a: Prerequisites for Closed-Loop Position Code](https://www.bilibili.com/video/BV1Rm4y1871K/)

6b. [FOC Algorithm Basics 6b: Closed-Loop Position Code Programming + Hardware Debugging Tutorial](https://www.bilibili.com/video/BV1yh4y1J7Xx/)

7a1. [FOC Algorithm Basics 7a1: Prerequisites for Closed-Loop Speed Code](https://www.bilibili.com/video/BV18m4y1v75j/?spm_id_from=333.788&vd_source=365d5478018f3e4c39b71b3dd6a7dd0a)

7a2. [FOC Algorithm Basics 7a2: Speed Low-Pass Filter](https://www.bilibili.com/video/BV16h4y1X7gZ/?spm_id_from=333.999.0.0)

7a3. [FOC Algorithm Basics 7a3: Speed PI Controller](https://www.bilibili.com/video/BV17s4y1y767/?spm_id_from=333.999.0.0)

7b. [FOC Algorithm Basics 7b: Closed-Loop Speed Code Programming + Hardware Debugging Tutorial](https://www.bilibili.com/video/BV1tu4y1o7WU/?spm_id_from=333.999.0.0)

8a. [FOC Algorithm Basics 8a: Prerequisites for Writing Current Loop Code](https://www.bilibili.com/video/BV1z14y1v7yS/?spm_id_from=333.999.0.0)

8b. [FOC Algorithm Basics 8b: Precise Current Torque Loop Code Programming + Hardware Debugging Tutorial](https://www.bilibili.com/video/BV1Sh4y1Q7ue/?spm_id_from=333.999.0.0)

9a. [FOC Algorithm Basics 9a: Improving Speed and Position Closed-Loop with Current Loop - Prerequisites](https://www.bilibili.com/video/BV1Zy4y1A7pL/)

9b. [FOC Algorithm Basics 9b: Improving Speed and Position Closed-Loop with Current Loop Code Programming + Hardware Debugging Tutorial](https://www.bilibili.com/video/BV1dy4y1N798/)

## 6 DengFOC Hobbyist Extension Support

DengFOC is an open-source project, and we welcome enthusiasts to engage in secondary development. Outstanding projects will be featured here. If you have created interesting projects based on DengFOC, feel free to contact me through the community.

Currently, a hobbyist has ported this project to the STM32 platform. Link:

https://github.com/haotianh9/DengFOC_on_STM32

