# GalbotRobot Python API Documentation
## Documentation Header
```bash
# Copyright (c) 2023 Galbot, Inc. All Rights Reserved.
#
# This software contains confidential and proprietary information of Galbot, Inc.
# ("Confidential Information"). You shall not disclose such Confidential Information
# and shall use it only in accordance with the terms of the license agreement you
# entered into with Galbot, Inc.
#
# UNAUTHORIZED COPYING, USE, OR DISTRIBUTION OF THIS SOFTWARE, OR ANY PORTION OR
# DERIVATIVE THEREOF, IS STRICTLY PROHIBITED. IF YOU HAVE RECEIVED THIS SOFTWARE IN
# ERROR, PLEASE NOTIFY GALBOT, INC. IMMEDIATELY AND DELETE IT FROM YOUR SYSTEM.
```
```bash
# Description: Python API Documentation for GalbotRobot
# Version: 1.0
# Date: 2023-09-20
# Author: Herman Ye
# Warning: This file is only for galbot developers, unauthorized use is prohibited.
```
```bash
# Revision History:
#
# Date       Version  Author       Description
# ---------- -------- ------------ -----------------------------------------------
# 2023-09-19 1.0.0      Herman Ye    Created
```

## Table of Contents
[TOC]


- [Introduction](#introduction)
- [Quick Start](#quick-start)
  - [Import](#import)
  - [Instantiate a robot object](#instantiate-a-robot-object)
  - [Get the hardware data](#get-the-hardware-data)
  - [Send commands](#send-commands)
    - [Send a command with JSON protocol (synchronous)](#1-send-a-command-with-json-protocol-synchronous)
    - [Send a command with JSON protocol (asynchronous)](#2-send-a-command-with-json-protocol-asynchronous)
- [Related libraries](#related-libraries)
  - [JSON](#json)
  - [Socket](#socket)
  - [Threading](#threading)
  - [asyncio](#asyncio)


## Introduction
This document describes the Python API for GalbotRobot. The GalbotRobot Python API is a Python module that provides a simple interface for communicating with GalbotRobot, getting hardware data, and sending commands to it. 
It allows you to easily send commands to GalbotRobot and receive responses from it. The Python API is designed to be easy to use and understand, and it is intended for use by developers who are familiar with Python.

## Quick Start

### Import
First, you need to import the GalbotRobot module.
```python
import GalbotRobot
``` 
### Instantiate a robot object
Then, you need to instantiate a robot object with the `robot name` and `IP address` in local area network.
```python
test_robot = GalbotRobot("Lucy","192.168.50.23")
```

Tips: You can find host IP address with the following command in Linux terminal, try it on your robot.
```bash
hostname -I
```


### Get the hardware data
You can get the hardware data directly from the robot object after instantiating it.

The hardware data is updated in real time by the background.
```python
# IMU
test_data_1 = test_robot.imu.angular_velocity.x
print(f"Current angular velocity in x direction from IMU is {test_data_1}")
```
```python
# Left arm
test_data_2 = test_robot.left_arm.joint_angle_1
print(f"Current joint angle 1 of left arm is {test_data_2}")
```

### Send commands
You can send a command to the robot with JSON protocol to control the robot. 

The details of the JSON protocol can be found in the `JSON Protocol Documentation`.
#### 1. Send a command with JSON protocol (synchronous)
The default `GalbotRobot.send()` method is synchronous, which means that the main thread will be blocked until the response is received.

For most commands, it won't take too long to get the response from the robot, so you can use the default `GalbotRobot.send()` method.

For example, you can send a command to set the target joint angle of the left arm and get the response from the robot.

- Command for setting the target joint angle of the left arm in JSON protocol
```json
{
    "hardware_type": "left_arm",
    "command": "set_arm_joint_angle",
    "parameters": {
        "speed": 0.3,
        "joint_angle_1": -0.160829,
        "joint_angle_2": 0.528468,
        "joint_angle_3": 1.326293,
        "joint_angle_4": -0.000454,
        "joint_angle_5": 1.221748,
        "joint_angle_6": -0.50052
    }
}
```
- Use the command in your Python code
```python
# Construct the left arm joint angle setting command
test_dictionary_data = {}
test_dictionary_data["hardware_type"] = "left_arm"
test_dictionary_data["command"] = "set_arm_joint_angle"
test_dictionary_data["parameters"] = {}
test_dictionary_data["parameters"]["speed"] = 0.3
test_dictionary_data["parameters"]["joint_angle_1"] = -0.160829
test_dictionary_data["parameters"]["joint_angle_2"] = 0.528468
test_dictionary_data["parameters"]["joint_angle_3"] = 1.326293
test_dictionary_data["parameters"]["joint_angle_4"] = -0.000454
test_dictionary_data["parameters"]["joint_angle_5"] = 1.221748
test_dictionary_data["parameters"]["joint_angle_6"] = -0.50052

# Send the command
left_arm_joint_angle_setting_result = test_robot.send(test_dictionary_data)

# Print the return value when the response is received
print(f"Hardware type: {left_arm_joint_angle_setting_result['hardware_type']}")
print(f"Command: {left_arm_joint_angle_setting_result['command']}")
print(f"Status code: {left_arm_joint_angle_setting_result['status_code']}")
print(f"Result description: {left_arm_joint_angle_setting_result['message']}")
```

#### 2. Send a command with JSON protocol (asynchronous)
But for some commands, you may not care about the response from the robot or you don't want to wait for the response from the robot because it may take a long time to get the response from the robot.

Let's take gripper setting command as an example, the main thread will be blocked until the `gripper_setting_result` is received.

```python
# Construct the gripper setting command
test_dictionary_data = {
    "hardware_type": "tool_gripper_hitbot",
    "command": "set_gripper_position",
    "parameters": {
        "position": 1.0,  
        "speed": 0.854,
        "force": 0.100,
        "continuous": False
    }
}

# Send the command and get the return value in a dictionary object
gripper_setting_result = test_robot.send(test_dictionary_data)

# Current thread will be blocked until the response gripper_setting_result is received

# Print the return value when the response is received
print(f"Hardware type: {gripper_setting_result['hardware_type']}")
print(f"Command: {gripper_setting_result['command']}")
print(f"Status code: {gripper_setting_result['status_code']}")
print(f"Status description: {gripper_setting_result['message']}")
print(f"Gripper stop position: {gripper_setting_result['data']['position']}")
```



In this case, you can use the `async_mode = True` parameter to send the command asynchronously, which means you don't need to wait for the execution result of the gripper setting command.

```python
# Construct the gripper setting command
test_dictionary_data = {
    "hardware_type": "tool_gripper_hitbot",
    "command": "set_gripper_position",
    "parameters": {
        "position": 1.0,  
        "speed": 0.854,
        "force": 0.100,
        "continuous": False
    }
}

# Send the command with async_mode = True
# You don't need to get the return value because it will be None
test_robot.send(test_dictionary_data, async_mode = True)

# send() method will return None immediately without waiting for the response
print("The gripper setting command has been sent.")
```






## Related libraries
### JSON
In Python, the `json` library is a built-in module that provides functions for working with JSON (JavaScript Object Notation) data. JSON is a lightweight data interchange format commonly used for data serialization and communication between a server and a client, as well as for storing configuration settings and data in a human-readable format.

For more information about JSON, please refer to the official documentation:
[Python JSON](https://docs.python.org/3/library/json.html).
### Socket
The `socket` library in Python is a core module that provides a low-level interface for network communication. It allows you to create network sockets, which are endpoints for sending and receiving data over a network. Sockets are commonly used for building networked applications, such as client-server applications, chat programs, web servers, and more.

For more information about Socket, please refer to the official documentation:
[Python Socket](https://docs.python.org/3/library/socket.html).
### Threading
The `threading` library in Python is a module that provides a way to create and manage threads, which are lightweight sub-processes within a Python process. Threads allow you to run multiple tasks concurrently, making it easier to manage and perform tasks in parallel. Python's threading library is part of the standard library and is built on top of the lower-level _thread module.

For more information about Threading, please refer to the official documentation:
[Python Threading](https://docs.python.org/3/library/threading.html).

### asyncio
The `asyncio` library in Python is a module that provides a way to create and manage asynchronous tasks, which are lightweight sub-processes within a Python process. Asynchronous tasks allow you to run multiple tasks concurrently, making it easier to manage and perform tasks in parallel. Python's asyncio library is part of the standard library and is built on top of the lower-level _asyncio module.

For more information about asyncio, please refer to the official documentation:
[Python asyncio](https://docs.python.org/3/library/asyncio.html).



**Note:**

If you have any questions, please contact @Herman Ye (hermanye233@icloud.com) 
