# 🚜 ESP32 Wi-Fi Controlled Forklift Robot

A compact **ESP32-based Wi-Fi Controlled Forklift Robot** designed to demonstrate wireless robotic movement, motor control, and material-lifting operations.

The robot creates its own Wi-Fi access point, allowing users to control the forklift directly from a **smartphone, tablet, or computer web browser** without requiring an external Wi-Fi router.

The project combines **ESP32 programming, Wi-Fi communication, embedded systems, motor control, robotics, and IoT concepts** into a practical material-handling robotic model.



## 📌 Project Overview

The ESP32 Wi-Fi Controlled Forklift Robot can perform:

* Forward movement
* Backward movement
* Left and right turning
* Fork lifting
* Fork lowering
* Emergency stopping
* Automatic safety timeout stopping
* Wireless control through a web browser

The ESP32 hosts a simple web-based control interface. Once connected to the robot's Wi-Fi network, the user can open the control page and operate the robot in real time.

### 🌐 No External Router Required

The ESP32 works as a **Wi-Fi Access Point (AP)**.


Smartphone / Laptop
        │
        │ Wi-Fi
        ▼
   ESP32 Forklift
        │
   ┌────┴────┐
   ▼         ▼
Drive     Lift System
Motors      Motor

# ✨ Key Features

* 🔧 ESP32-based control system
* 📡 Built-in Wi-Fi Access Point
* 📱 Smartphone and computer control
* 🌐 Web-based control interface
* 🚜 Forward and backward movement
* ↔️ Left and right turning
* 🏗️ Motorized forklift lifting mechanism
* 🛑 Emergency stop function
* 🔄 Emergency reset function
* ⏱️ Automatic command timeout safety
* 🔋 Low-power ESP32 operation
* 📶 No external Wi-Fi router required
* 🎮 Hold-to-move control buttons
* 📊 Real-time robot status display
* 🔌 Dual L298N motor-driver system



# 🎯 Objectives

The main objective of this project is to provide students with hands-on experience in:

* ESP32 programming
* Wi-Fi communication
* Embedded systems
* DC motor control
* L298N motor drivers
* Robotics
* IoT applications
* Web-based robotic control
* Circuit assembly
* Mechanical integration
* Testing and troubleshooting

The project demonstrates how wireless robotic systems can be applied to **material-handling and industrial automation applications**.



# 🧰 Components Required

| Sl. No. | Component                            | Quantity |
| ------: | ------------------------------------ | -------: |
|       1 | ESP32 Development Board              |        1 |
|       2 | BO Motors                            |        5 |
|       3 | L298N Motor Driver                   |        2 |
|       4 | Pin Header & Socket Header           |    1 Set |
|       5 | Wheels                               |        4 |
|       6 | Connecting Wire                      |    1/2 m |
|       7 | Jumper Wires (Male-Female)           |    1 Set |
|       8 | 20 × 15 cm Double-Sided Dot Hole PCB |        1 |
|       9 | Battery                              |        1 |
|      10 | 3D-Printed Forklift Chassis/Case     |    1 Set |

> **Note:** The current control system uses two drive motors and one lift motor. The remaining motors/components can be used according to the mechanical design of the particular forklift model.



# 🔌 Pin Configuration

## L298N #1 — Drive System

The first L298N controls the left and right drive motors.

| ESP32 GPIO | L298N Pin | Function              |
| ---------: | --------- | --------------------- |
|    GPIO 25 | ENA       | Left Motor Speed      |
|     GPIO 5 | IN1       | Left Motor Direction  |
|    GPIO 18 | IN2       | Left Motor Direction  |
|    GPIO 14 | ENB       | Right Motor Speed     |
|    GPIO 12 | IN3       | Right Motor Direction |
|    GPIO 13 | IN4       | Right Motor Direction |

### Drive Motor Logic

| Command  | Left Motor | Right Motor |
| -------- | ---------- | ----------- |
| Forward  | Forward    | Forward     |
| Backward | Reverse    | Reverse     |
| Left     | Reverse    | Forward     |
| Right    | Forward    | Reverse     |
| Stop     | Stop       | Stop        |



## L298N #2 — Fork Lift System

The second L298N controls the forklift lifting motor.

| ESP32 GPIO | L298N Pin | Function             |
| ---------: | --------- | -------------------- |
|    GPIO 33 | ENA       | Lift Motor Speed     |
|    GPIO 32 | IN1       | Lift Motor Direction |
|     GPIO 4 | IN2       | Lift Motor Direction |

### Lift Motor Logic

| Command | Direction             |
| ------- | --------------------- |
| UP      | Lift mechanism raises |
| DOWN    | Lift mechanism lowers |
| STOP    | Lift motor stops      |



# ⚡ Motor Speed Configuration

The current program uses fixed PWM values:


const int DRIVE_SPEED = 130;
const int LIFT_SPEED = 100;


### Drive Motor
PWM = 130 / 255
### Lift Motor


PWM = 100 / 255


These values can be adjusted in the program according to the robot's motor, battery, load, and mechanical configuration.


# 🔋 Power Connections

Connect the battery to the L298N motor driver:

Battery (+)  →  L298N 12V
Battery (-)  →  L298N GND


The ESP32 and motor-driver system must share a **common ground**.

> ⚠️ Use the correct battery voltage for the selected motors and motor driver.



# 🌐 Wi-Fi Configuration

The ESP32 creates its own Wi-Fi network.

### Wi-Fi Details


Wi-Fi Name: Forklift_Robot_Pro
Password:   123456789


### Control URL

http://192.168.4.1
```

No external Wi-Fi router is required.

---

# 📱 How to Operate the Robot

## Step 1 — Power ON

Switch ON the forklift robot.

Wait for the ESP32 to create its Wi-Fi network.

---

## Step 2 — Connect to Wi-Fi

Using a smartphone or laptop:

1. Open Wi-Fi settings.
2. Search for:


Forklift_Robot_Pro

3. Enter:
123456789

4. Connect to the network.

## Step 3 — Open the Control Page

Open a web browser and enter:


http://192.168.4.1


The forklift control interface will appear.



## Step 4 — Control the Robot

### Drive Controls


             ┌─────────┐
             │   FWD   │
             └─────────┘

┌─────────┐ ┌─────────┐ ┌─────────┐
│  LEFT   │ │  STOP   │ │  RIGHT  │
└─────────┘ └─────────┘ └─────────┘

             ┌─────────┐
             │  BACK   │
             └─────────┘

### Lift Controls


┌─────────┐     ┌─────────┐
│   UP    │     │   DOWN  │
└─────────┘     └─────────┘


The movement buttons are designed as **hold-to-move controls**. When the button is released, the corresponding movement command is stopped.



# 🛑 Emergency Stop

The control interface includes an **EMERGENCY STOP** button.

When activated:


Emergency Stop
      ↓
Drive Motors OFF
      +
Lift Motor OFF
      ↓
Robot Locked


Normal operation can be restored using the **RESET** button.



# ⏱️ Safety Timeout

The program includes a command timeout:

const unsigned long TIMEOUT_MS = 1500;

If movement commands stop arriving for the specified time, the corresponding motor system is automatically stopped.

This provides an additional software safety mechanism in case communication is interrupted.

# 💻 Software Requirements

### Required Software

* Arduino IDE
* ESP32 Board Package
* USB cable
* ESP32 Development Board

### Required Libraries


#include <WiFi.h>
#include <WebServer.h>


These libraries are used for:

* Wi-Fi Access Point creation
* Web server operation
* Browser-based robot control



# 🧑‍💻 Uploading the Code

## 1. Install ESP32 Board Support

Install the ESP32 board package in Arduino IDE.

## 2. Connect ESP32

Connect the ESP32 to your computer using a USB cable.

## 3. Select the Board

Select the appropriate ESP32 board from:


Tools → Board

## 4. Select COM Port

Select the ESP32's connected COM port:

```text
Tools → Port


## 5. Upload

Upload the forklift robot program.

After successful upload, open the Serial Monitor at:


115200 baud


The ESP32 should display the Access Point IP address.

Example:


Open http://192.168.4.1




# 🏗️ Mechanical Assembly

## Step 1 — Prepare Components

Prepare:

* ESP32
* L298N motor drivers
* BO motors
* PCB
* Connecting wires
* Jumper wires
* Battery
* Forklift chassis
* 3D-printed components
* Wheels and motor hubs


## Step 2 — Motor Wiring

Solder wires to the BO motors and ensure that the connections are mechanically secure.



## Step 3 — Connect Drive Motors

Connect the drive motors to the first L298N motor driver.

Verify the motor output terminals before powering the circuit.



## Step 4 — Connect Lift Motor

Connect the lift motor to the second L298N motor driver.



## Step 5 — Assemble the PCB

Arrange the ESP32, motor-driver connections, and wiring securely on the PCB.

Avoid loose connections and exposed wires.



## Step 6 — Install Electronics

Place the assembled electronics inside the 3D-printed forklift case.

Ensure that:

* Wires are organized
* Motor-driver terminals are accessible
* No wires interfere with moving parts
* The ESP32 remains protected



## Step 7 — Install Drive Motors

Fix the two drive motors to the chassis.

Secure the motor mounts properly.


## Step 8 — Install Wheels

Attach the wheels and motor hubs to the drive motors.

Check that the wheels rotate freely.



## Step 9 — Install Lift Motor

Mount the third BO motor to the forklift lifting mechanism.

Check the lifting mechanism manually before powering the motor.



## Step 10 — Final Inspection

Before powering ON:

* Check all motor connections.
* Check battery polarity.
* Check common GND.
* Check ESP32 connections.
* Check that the wheels rotate freely.
* Check that the lift mechanism moves freely.
* Check that no wires touch moving components.



# 🧪 Testing Procedure

A recommended testing sequence is:


Power ON
   ↓
Check ESP32
   ↓
Connect to Wi-Fi
   ↓
Open Web Interface
   ↓
Test STOP
   ↓
Test Forward
   ↓
Test Backward
   ↓
Test Left / Right
   ↓
Test Lift UP
   ↓
Test Lift DOWN
   ↓
Test Emergency STOP


Test the robot **without a load first**.

After confirming correct operation, gradually introduce a suitable lightweight load.



# ⚠️ Safety Precautions

* Check all wiring before switching ON the power.
* Use the correct battery voltage.
* Keep hands and fingers away from moving wheels.
* Keep hands away from the lifting mechanism.
* Do not overload the forklift.
* Operate the robot on a stable and flat surface.
* Stop the robot before modifying or repairing connections.
* Do not touch motor terminals while power is ON.
* Keep the robot away from water and moisture.
* Do not exceed the mechanical limits of the lifting mechanism.
* Switch OFF the battery when the robot is not being used.
* Test the emergency stop before normal operation.
* Keep the robot away from people while testing.


# 🔧 Troubleshooting

| Problem                              | Possible Solution                                        |
| ------------------------------------ | -------------------------------------------------------- |
| Robot not moving                     | Check battery, L298N power supply, and motor connections |
| One motor not working                | Check BO motor wires and L298N output terminals          |
| Robot moves in wrong direction       | Reverse the corresponding motor wires                    |
| Robot turns incorrectly              | Check left/right motor orientation and wiring            |
| Lift not moving                      | Check second L298N, lift motor, and control connections  |
| Lift moves in wrong direction        | Swap the lift motor wires                                |
| Wi-Fi not visible                    | Check ESP32 power and confirm the program is uploaded    |
| Webpage not opening                  | Connect to `Forklift_Robot_Pro` and open `192.168.4.1`   |
| Robot stops suddenly                 | Check battery voltage and power connections              |
| Buttons not responding               | Refresh the webpage and reconnect to Wi-Fi               |
| Emergency mode active                | Press RESET after confirming the robot is safe           |
| Robot stops after communication loss | Check Wi-Fi connection and command timeout behavior      |



# 🧠 How the Software Works

The ESP32 operates as a Wi-Fi Access Point and Web Server.

```text
             ESP32
               │
        Wi-Fi Access Point
               │
        ┌──────┴──────┐
        │             │
    Smartphone     Laptop
        │             │
        └──────┬──────┘
               │
        Web Control Page
               │
        HTTP Commands
               │
          ESP32 WebServer
          ┌──────┴──────┐
          │             │
       Drive          Lift
          │             │
       L298N #1      L298N #2
          │             │
       BO Motors     Lift Motor



# 🔄 Command Flow

The web interface sends commands to the ESP32 using HTTP requests.

Examples:

/command?cmd=forward
/command?cmd=backward
/command?cmd=left
/command?cmd=right
/command?cmd=stop
/command?cmd=lift_up
/command?cmd=lift_down
/command?cmd=lift_stop
/command?cmd=emergency
/command?cmd=reset_emergency


The ESP32 receives the command and controls the appropriate L298N motor-driver outputs.


# 📊 Robot Status

The web interface periodically requests the robot status.

Example:
Drive: FORWARD | Lift: STOPPED
or:
Drive: STOPPED | Lift: UP
During an emergency:

EMERGENCY STOP

# ⚙️ Low-Power Operation

The current program reduces the ESP32 CPU frequency:
setCpuFrequencyMhz(80);
The Wi-Fi transmit power is also reduced:
WiFi.setTxPower(WIFI_POWER_11dBm);


This configuration is intended for nearby smartphone/laptop operation while reducing unnecessary power consumption.

# 📁 Suggested GitHub Repository Structure

Forklift-Robot/
│
├── README.md
│
├── Code/
│  
│
Manual.pdf

# 🎓 Learning Outcomes

By completing this project, students will learn:

* Basics of ESP32 programming
* Wi-Fi Access Point configuration
* Web-based robotic control
* HTTP communication
* DC motor control
* L298N motor-driver operation
* PWM-based speed control
* Motor direction control
* Embedded-system integration
* Basic robotics concepts
* Mechanical and electronic assembly
* Safety mechanisms
* Troubleshooting and testing



# 🚀 Possible Future Improvements

The project can be further upgraded with:

* 📷 ESP32-CAM live video
* 📱 Dedicated Android application
* 🎮 Joystick control
* 📦 Automatic load detection
* 📏 Ultrasonic obstacle detection
* 🤖 Autonomous navigation
* 🧭 Line-following capability
* 📍 Indoor positioning
* ⚖️ Load-weight measurement
* 🔋 Battery monitoring
* 📊 IoT dashboard
* 🧠 AI-based object detection
* 🚧 Automatic obstacle avoidance
* 🔐 Web interface authentication


# 📸 Project Demonstration

Add project images here:


![Forklift Robot](Images/forklift-front.jpg)

Example demonstration sections can include:

* Complete forklift model
* Electronics and wiring
* Motor-driver connections
* Lift mechanism
* ESP32 board
* Web control interface
* Robot carrying a lightweight load

# 🤖 Project Summary

The **ESP32 Wi-Fi Controlled Forklift Robot** is an educational robotics platform that demonstrates how an ESP32 can combine **Wi-Fi communication, web-based control, motor drivers, DC motors, and mechanical lifting systems** into a functional robotic application.

The project provides students with practical experience in **electronics, programming, robotics, IoT, mechanical assembly, testing, and troubleshooting**, while demonstrating the use of robotics in material-handling applications.

## 👨‍💻 Developed For

**Educational Robotics | IoT | Embedded Systems | STEM Learning | Robotics Projects**

**Platform:** ESP32
**Motor Driver:** L298N
**Communication:** Wi-Fi Access Point
**Control:** Web Browser
**Application:** Material Handling / Educational Robotics
