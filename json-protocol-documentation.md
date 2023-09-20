# JSON Protocol Documentation

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
# Description: JSON protocol for galbot robots.
# Version: 1.2.0
# Date: 2023-09-20
# Author: Herman Ye @ Galbot, Inc.
```

```bash
# Revision History:
#
# Date       Version  Author       Description
# ---------- -------- ------------ -----------------------------------------------
# 2023-08-14 1.0.0    Herman Ye    Created
# 2023-08-21 1.0.1    Herman Ye    Added basic JSON commands
# 2023-08-25 1.0.2    Herman Ye    Added gripper and sucker commands
# 2023-09-01 1.1.0    Herman Ye    Added dual arm commands
# 2023-09-04 1.1.1    Herman Ye    Added robot commands description
# 2023-09-20 1.2.0    Herman Ye    Transfer documentation to gitbook format
```

## Complete JSON Protocol Hardware Type Table

| Index | Hardware Type      |
| ----- | ------------------ |
| 1     | robot              |
| 2     | body               |
| 3     | base               |
| 4     | head\_camera       |
| 5     | head\_yaw\_motor   |
| 6     | head\_pitch\_motor |
| 7     | left\_arm          |
| 8     | left\_arm\_camera  |
| 9     | left\_hand         |
| 10    | right\_arm         |
| 11    | right\_arm\_camera |
| 12    | right\_hand        |
| 13    | lidar              |
| 14    | imu                |
| 15    | tool\_gripper      |
| 16    | tool\_sucker       |

## 1. `robot`\[TODO]

| hardware\_type | command                              |
| -------------- | ------------------------------------ |
| robot          | get\_robot\_hardware\_software\_info |
| robot          | get\_robot\_status                   |
| robot          | check\_software\_updates             |
| robot          | factory\_reset                       |

TODO@HermanYe: Wait for the robot version to stabilize and for independent research and development of most hardware components to be completed before proceeding with this part.

### `get_robot_hardware_software_info` \[TODO]

Retrieve the current hardware and software information of the robot.

### `get_robot_status` \[TODO]

Fetch the current operational status of the robot.

| Status       | Description                                                         |
| ------------ | ------------------------------------------------------------------- |
| initializing | Robot is in the process of starting up.                             |
| idle         | Robot is in a standby mode, waiting for user input or tasks.        |
| navigating   | Robot is planning its movement to a new location.                   |
| interacting  | Robot is engaged in an interaction with the environment or users.   |
| charging     | Robot is currently recharging its battery.                          |
| error        | Robot has encountered an error or malfunction that needs attention. |

![Robot Statuses](https://img-blog.csdnimg.cn/63da159387db4d9487d5149d2f74afea.png)

### `check_software_updates` \[TODO]

Check the robot's software version and perform an update if an Over-The-Air (OTA) upgrade is available.

\->`Check Current Version` -> `Connect to Galbot Server` -> `Compare Versions` -> `Notify User` -> `Download Update` -> `Apply Update` -> `Reboot`

### `factory_reset` \[TODO]

Erase memory and restore the robot to its original factory settings.

## 2. `body`\[TESTED]

| hardware\_type | command                   |
| -------------- | ------------------------- |
| body           | set\_body\_back\_to\_zero |
| body           | set\_body\_position       |
| body           | get\_body\_status         |

**Body Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| Index                                                                                                        | Topic                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------- |
| 1                                                                                                            | /galbot\_body/command |
| 2                                                                                                            | /galbot\_body/status  |
| ## `set_body_back_to_zero`\[TESTED]                                                                          |                       |
| **Description:**                                                                                             |                       |
| The `set_body_back_to_zero` command is a function for repositioning the robot's body to its "zero" position. |                       |
| It is essential to perform this action before engaging the body lift function.                               |                       |
| By resetting the body to its zero position, you ensure that the robot starts from a known reference point.   |                       |

**Send:**

```yaml
{
  "hardware_type": "body",
  "command": "set_body_back_to_zero",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "body",
  "command": "set_body_back_to_zero",
  "message": "Body back to zero set successfully.",
  "data": null
  "status_code": 0,
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "body",    # Specifies the type of hardware or component the command is intended for (in this case, the robot's body).
  "command": "set_body_back_to_zero",   # The specific command to set the robot's body back to its zero position.
  "parameters": {}   # Any additional parameters or data needed for the command (none in this case).
}

# Received
{
  "hardware_type": "body",    # Indicates the type of hardware/component this response pertains to (the robot's body).
  "command": "set_body_back_to_zero",   # Confirms that the received response is related to the "set_body_back_to_zero" command.
  "message": "Body back to zero set successfully.",   # A descriptive message confirming the successful execution of the command.
  "data": null,    # Any relevant data associated with the command execution (none in this case).
  "status_code": 0,   # A status code indicating the success or failure of the command (0 signifies success).
}
```

### `set_body_position`\[TESTED]

**Description:** The `set_body_position` command is used to adjust the robot's body lift to a specific position. Available position range of `[0, 0.6]` meters. Prior to using the "set\_body\_position" command, it is essential to reset the body lift position to zero. **Send:**

```yaml
{
  "hardware_type": "body",
  "command": "set_body_position",
  "parameters": {
    "position": 0.6
  }
}
```

**Received:**

```yaml
{
  "hardware_type": "body",
  "command": "set_body_position",
  "message": "Body position set successfully.",
  "data": null,
  "status_code": 0
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "body",    # Specifies the type of hardware or component the command is intended for (in this case, the robot's body).
  "command": "set_body_position",   # The specific command to set the robot's body to a desired position.
  "parameters": {
    "position": 0.6   # Specifies the target position for the robot's body (in this case, 0.6 units).
  }
}

# Received
{
  "hardware_type": "body",    # Indicates the type of hardware/component this response pertains to (the robot's body).
  "command": "set_body_position",   # Confirms that the received response is related to the "set_body_position" command.
  "message": "Body position set successfully.",   # A descriptive message confirming the successful execution of the command.
  "data": null,    # Any relevant data associated with the command execution (none in this case).
  "status_code": 0,   # A status code indicating the success or failure of the command (0 signifies success).
}
```

### `get_body_status`\[TESTED]

**Description:** The `get_body_status` command is utilized to fetch the status of the robot's body lift part, offering information regarding whether the body has returned to the zero position, the current action's in-place status, and its present position.

**Send:**

```yaml
{
  "hardware_type": "body",
  "command": "get_body_status",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "body",
  "command": "get_body_status",
  "data": {
    "back_to_zero": true,
    "in_place": true,
    "position": 0.6
  },
  "message": "Body status retrieved successfully.",
  "status_code": 0
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "body",    # Specifies the type of hardware or component the command is intended for (in this case, the robot's body).
  "command": "get_body_status",   # The specific command to retrieve the body's status.
  "parameters": {}   # Any additional parameters or data needed for the command (none in this case).
}

# Received
{
  "hardware_type": "body",    # Indicates the type of hardware/component this response pertains to (the robot's body).
  "command": "get_body_status",   # Confirms that the received response is related to the "get_body_status" command.
  "data": {
    "back_to_zero": true,   # Indicates whether the body is back to its zero position (true in this case).
    "in_place": true,       # Indicates whether the body is in place (true in this case).
    "position": 0.6         # Provides the current position of the body (0.6 in this case).
  },
  "message": "Body status retrieved successfully.",   # A descriptive message confirming the successful execution of the command.
  "status_code": 0   # A status code indicating the success of the command (0 signifies success).
}
```

## 3. `base` \[TESTED]

| hardware\_type | command             |
| -------------- | ------------------- |
| base           | set\_base\_velocity |
| base           | get\_base\_velocity |

**Base Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| Index                                                                                         | Topic                                       |
| --------------------------------------------------------------------------------------------- | ------------------------------------------- |
| 1                                                                                             | /xtopic\_comm/com\_recv\_xstd\_arm          |
| 2                                                                                             | /xtopic\_comm/com\_recv\_xstd\_comm         |
| 3                                                                                             | /xtopic\_comm/com\_recv\_xstd\_conveyer     |
| 4                                                                                             | /xtopic\_comm/com\_recv\_xstd\_custom       |
| 5                                                                                             | /xtopic\_comm/com\_recv\_xstd\_driver       |
| 6                                                                                             | /xtopic\_comm/com\_recv\_xstd\_light        |
| 7                                                                                             | /xtopic\_comm/com\_recv\_xstd\_power        |
| 8                                                                                             | /xtopic\_comm/com\_recv\_xstd\_sandbox      |
| 9                                                                                             | /xtopic\_comm/com\_recv\_xstd\_sensor\_dist |
| 10                                                                                            | /xtopic\_comm/com\_recv\_xstd\_sensor\_gps  |
| 11                                                                                            | /xtopic\_comm/com\_recv\_xstd\_sensor\_imu  |
| 12                                                                                            | /xtopic\_comm/com\_recv\_xstd\_sensor\_line |
| 13                                                                                            | /xtopic\_comm/com\_recv\_xstd\_switch       |
| 14                                                                                            | /xtopic\_comm/com\_recv\_xstd\_vehicle      |
| 15                                                                                            | /xtopic\_comm/com\_send\_xstd               |
| 16                                                                                            | /xtopic\_comm/device\_list\_json            |
| 17                                                                                            | /xtopic\_vehicle/ctrl\_json                 |
| 18                                                                                            | /xtopic\_vehicle/device\_state\_json        |
| 19                                                                                            | /tf                                         |
| 20                                                                                            | /base/cmd\_vel                              |
| 21                                                                                            | /base/odom                                  |
| 22                                                                                            | /base/vehicle\_path                         |
| ## `set_base_velocity`\[TESTED]                                                               |                                             |
| **Description:**                                                                              |                                             |
| The `set_base_velocity` command is used to set the velocity of the base vehicle of the robot. |                                             |
| This allows for controlling the robot's movement in both linear and angular directions.       |                                             |
| The base vehicle can be controlled through JSON commands via the keyboard node of Galbot.     |                                             |

```bash
roslaunch galbot_bringup user_interface_keyboard.launch json_control:=true
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/bab1e20a40034469b4252d8a0903ad08.png)

**Send:**

```yaml
{
  "hardware_type": "base",
  "command": "set_base_velocity",
  "parameters": {
    "linear_velocity": {
      "x": 0.1,  
      "y": 0.0,  
      "z": 0.0  
    },
    "angular_velocity": {
      "x": 0.0,  
      "y": 0.0, 
      "z": 0.25  
    }
  }
}
```

**Received:**

```yaml
{
  "hardware_type": "base",
  "command": "set_base_velocity",
  "status_code": 0,
  "message": "Velocity settings applied successfully.",
  "data": {}
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "base",   # Specifies the type of hardware or component the command is intended for (the robot's base).
  "command": "set_base_velocity",   # The specific command to set the velocity of the robot's base.
  "parameters": {
    "linear_velocity": {
      "x": 0.1,   # Linear velocity along the x-axis (in meters per second).
      "y": 0.0,   # Linear velocity along the y-axis (in meters per second).
      "z": 0.0    # Linear velocity along the z-axis (in meters per second).
    },
    "angular_velocity": {
      "x": 0.0,   # Angular velocity around the x-axis (in radians per second).
      "y": 0.0,   # Angular velocity around the y-axis (in radians per second).
      "z": 0.25    # Angular velocity around the z-axis (in radians per second).
    }
  }
}

# Received
{
  "hardware_type": "base",   # Indicates the type of hardware/component this response pertains to (the robot's base).
  "command": "set_base_velocity",   # Confirms that the received response is related to the "set_base_velocity" command.
  "status_code": 0,   # A status code indicating the success or failure of the command (0 signifies success).
  "message": "Velocity settings applied successfully.",   # A descriptive message confirming the successful execution of the command.
  "data": {}   # Any relevant data associated with the command execution (none in this case).
}
```

### `get_base_velocity`\[TESTED]

**Description:** The `get_base_velocity` command is used to retrieve the current velocity of the base vehicle of the robot. This information is crucial for monitoring the robot's movement and orientation.

**Send:**

```yaml
{
  "hardware_type": "base",
  "command": "get_base_velocity",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "base",
  "command": "get_base_velocity",
  "status_code": 0,
  "message": "Velocity data retrieved successfully.",
  "data": {
    "linear_velocity": {
      "x": 0.5,
      "y": 0.0,
      "z": 0.0
    },
    "angular_velocity": {
      "x": 0.0,
      "y": 0.0,
      "z": 1.2
    }
  }
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "base",    # Specifies the type of hardware or component the command is intended for (in this case, the robot's base).
  "command": "get_base_velocity",   # The specific command to retrieve the base velocity.
  "parameters": {}   # Any additional parameters or data needed for the command (none in this case).
}

# Received
{
  "hardware_type": "base",    # Indicates the type of hardware/component this response pertains to (the robot's base).
  "command": "get_base_velocity",   # Confirms that the received response is related to the "get_base_velocity" command.
  "status_code": 0,   # A status code indicating the success or failure of the command (0 signifies success).
  "message": "Velocity data retrieved successfully.",   # A descriptive message confirming the successful execution of the command.
  "data": {
    "linear_velocity": {
      "x": 0.5,   # Current linear velocity along the x-axis (m/s)
      "y": 0.0,   # Current linear velocity along the y-axis (m/s)
      "z": 0.0    # Current linear velocity along the z-axis (m/s)
    },
    "angular_velocity": {
      "x": 0.0,   # Current angular velocity around the x-axis (rad/s)
      "y": 0.0,   # Current angular velocity around the y-axis (rad/s)
      "z": 1.2    # Current angular velocity around the z-axis (rad/s)
    }
  }
}
```

## 4. `head_camera`\[TESTED]

| hardware\_type | command           |
| -------------- | ----------------- |
| head\_camera   | get\_camera\_info |

**Head Camera Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| Index | Topic                                                                           |
| ----- | ------------------------------------------------------------------------------- |
|       | **/head\_camera/depth**                                                         |
| 1     | /head\_camera/depth/camera\_info                                                |
| 2     | /head\_camera/depth/image\_raw                                                  |
| 3     | /head\_camera/depth/image\_raw/compressed                                       |
| 4     | /head\_camera/depth/image\_raw/compressed/parameter\_descriptions               |
| 5     | /head\_camera/depth/image\_raw/compressed/parameter\_updates                    |
| 6     | /head\_camera/depth/image\_raw/compressedDepth                                  |
| 7     | /head\_camera/depth/image\_raw/compressedDepth/parameter\_descriptions          |
| 8     | /head\_camera/depth/image\_raw/compressedDepth/parameter\_updates               |
| 9     | /head\_camera/depth/image\_raw/theora                                           |
| 10    | /head\_camera/depth/image\_raw/theora/parameter\_descriptions                   |
| 11    | /head\_camera/depth/image\_raw/theora/parameter\_updates                        |
|       | **/head\_camera/depth\_to\_rgb**                                                |
| 12    | /head\_camera/depth\_to\_rgb/camera\_info                                       |
| 13    | /head\_camera/depth\_to\_rgb/image\_raw                                         |
| 14    | /head\_camera/depth\_to\_rgb/image\_raw/compressed                              |
| 15    | /head\_camera/depth\_to\_rgb/image\_raw/compressed/parameter\_descriptions      |
| 16    | /head\_camera/depth\_to\_rgb/image\_raw/compressed/parameter\_updates           |
| 17    | /head\_camera/depth\_to\_rgb/image\_raw/compressedDepth                         |
| 18    | /head\_camera/depth\_to\_rgb/image\_raw/compressedDepth/parameter\_descriptions |
| 19    | /head\_camera/depth\_to\_rgb/image\_raw/compressedDepth/parameter\_updates      |
| 20    | /head\_camera/depth\_to\_rgb/image\_raw/theora                                  |
| 21    | /head\_camera/depth\_to\_rgb/image\_raw/theora/parameter\_descriptions          |
| 22    | /head\_camera/depth\_to\_rgb/image\_raw/theora/parameter\_updates               |
|       | **/head\_camera/imu**                                                           |
| 23    | /head\_camera/imu                                                               |
|       | **/head\_camera/ir**                                                            |
| 24    | /head\_camera/ir/camera\_info                                                   |
| 25    | /head\_camera/ir/image\_raw                                                     |
| 26    | /head\_camera/ir/image\_raw/compressed                                          |
| 27    | /head\_camera/ir/image\_raw/compressed/parameter\_descriptions                  |
| 28    | /head\_camera/ir/image\_raw/compressed/parameter\_updates                       |
| 29    | /head\_camera/ir/image\_raw/compressedDepth                                     |
| 30    | /head\_camera/ir/image\_raw/compressedDepth/parameter\_descriptions             |
| 31    | /head\_camera/ir/image\_raw/compressedDepth/parameter\_updates                  |
| 32    | /head\_camera/ir/image\_raw/theora                                              |
| 33    | /head\_camera/ir/image\_raw/theora/parameter\_descriptions                      |
| 34    | /head\_camera/ir/image\_raw/theora/parameter\_updates                           |
|       | **/head\_camera/joint\_states**                                                 |
| 35    | /head\_camera/joint\_states                                                     |
|       | **/head\_camera/points2**                                                       |
| 36    | /head\_camera/points2                                                           |
|       | **/head\_camera/rgb**                                                           |
| 37    | /head\_camera/rgb/camera\_info                                                  |
| 38    | /head\_camera/rgb/image\_raw                                                    |
| 39    | /head\_camera/rgb/image\_raw/compressed                                         |
| 40    | /head\_camera/rgb/image\_raw/compressed/parameter\_descriptions                 |
| 41    | /head\_camera/rgb/image\_raw/compressed/parameter\_updates                      |
| 42    | /head\_camera/rgb/image\_raw/compressedDepth                                    |
| 43    | /head\_camera/rgb/image\_raw/compressedDepth/parameter\_descriptions            |
| 44    | /head\_camera/rgb/image\_raw/compressedDepth/parameter\_updates                 |
| 45    | /head\_camera/rgb/image\_raw/theora                                             |
| 46    | /head\_camera/rgb/image\_raw/theora/parameter\_descriptions                     |
| 47    | /head\_camera/rgb/image\_raw/theora/parameter\_updates                          |
|       | **/head\_camera/rgb\_to\_depth**                                                |
| 48    | /head\_camera/rgb\_to\_depth/camera\_info                                       |
| 49    | /head\_camera/rgb\_to\_depth/image\_raw                                         |
| 50    | /head\_camera/rgb\_to\_depth/image\_raw/compressed                              |
| 51    | /head\_camera/rgb\_to\_depth/image\_raw/compressed/parameter\_descriptions      |
| 52    | /head\_camera/rgb\_to\_depth/image\_raw/compressed/parameter\_updates           |
| 53    | /head\_camera/rgb\_to\_depth/image\_raw/compressedDepth                         |
| 54    | /head\_camera/rgb\_to\_depth/image\_raw/compressedDepth/parameter\_descriptions |
| 55    | /head\_camera/rgb\_to\_depth/image\_raw/compressedDepth/parameter\_updates      |
| 56    | /head\_camera/rgb\_to\_depth/image\_raw/theora                                  |
| 57    | /head\_camera/rgb\_to\_depth/image\_raw/theora/parameter\_descriptions          |
| 58    | /head\_camera/rgb\_to\_depth/image\_raw/theora/parameter\_updates               |

### `get_camera_info`\[TESTED]

**Description:** The `get_camera_info` command is used to retrieve essential information from cameras of the robot's head. This information includes camera calibration parameters, distortion coefficients, and other metadata. **Send:**

```yaml
{
  "hardware_type": "head_camera",
  "command": "get_camera_info",
  "parameters": {}
}
```

**Received:**

```yaml
{
   "hardware_type":"head_camera",
   "command":"get_camera_info",
   "status_code":0,
   "message":"Camera info retrieved successfully.",
   "data":{
      "head_camera_depth":{
         "D":[
            3.894296407699585,
            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{
            "frame_id":"depth_camera_link",
            "seq":0,
            "stamp":1693207028.365125
         },
         "height":1024,
         "width":1024
      },
      "head_camera_depth2rgb":{
         "D":[
            -0.36101600527763367,
            -2.2728121280670166,
            9.10795497475192e-05,
            -0.00020006165141239762,
            1.8348795175552368,
            -0.4750489294528961,
            -2.046283006668091,
            1.7131365537643433
         ],
         "K":[
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0,
            0.0
         ],
         "P":[
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0
         ],
         "R":[
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{
            "frame_id":"rgb_camera_link",
            "seq":0,
            "stamp":1693207028.365125
         },
         "height":1536,
         "width":2048
      },
      "head_camera_ir":{
         "D":[
            3.894296407699585,
            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{
            "frame_id":"depth_camera_link",
            "seq":0,
            "stamp":1693207028.365125
         },
         "height":1024,
         "width":1024
      },
      "head_camera_rgb":{
         "D":[
            -0.36101600527763367,
            -2.2728121280670166,
            9.10795497475192e-05,
            -0.00020006165141239762,
            1.8348795175552368,
            -0.4750489294528961,
            -2.046283006668091,
            1.7131365537643433
         ],
         "K":[
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0,
            0.0
         ],
         "P":[
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0
         ],
         "R":[
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{
            "frame_id":"rgb_camera_link",
            "seq":0,
            "stamp":1693207028.3649921
         },
         "height":1536,
         "width":2048
      },
      "head_camera_rgb2depth":{
         "D":[
            3.894296407699585,


            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{
            "frame_id":"depth_camera_link",
            "seq":0,
            "stamp":1693207028.3649921
         },
         "height":1024,
         "width":1024
      }
   }
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "head_camera",     # Specifies the type of hardware component being interacted with (e.g., "head_camera").
  "command": "get_camera_info",        # Specifies the specific command being sent (requesting camera information).
  "parameters": {}                     # Additional parameters or data needed for the command (none in this case).
}


# Received
{
   "hardware_type":"head_camera",  // Type of hardware: head-mounted camera
   "command":"get_camera_info",  // Command to retrieve camera information
   "status_code":0,  // Status code indicating success
   "message":"Camera info retrieved successfully.",  // Status message
   "data":{
      "head_camera_depth":{  // Depth camera information
         "D":[
            3.894296407699585,  // Distortion coefficients
            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[  // Camera matrix (intrinsic parameters)
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[  // Projection matrix
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[  // Rectification matrix
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{  // Metadata about the camera frame
            "frame_id":"depth_camera_link",  // Reference frame ID
            "seq":0,  // Sequence number
            "stamp":1693207028.365125  // Timestamp of the data
         },
         "height":1024,  // Height of the camera frame
         "width":1024  // Width of the camera frame
      },
      "head_camera_depth2rgb":{  // Transformation from depth camera to RGB camera
         "D":[
            -0.36101600527763367,  // Distortion coefficients
            -2.2728121280670166,
            9.10795497475192e-05,
            -0.00020006165141239762,
            1.8348795175552368,
            -0.4750489294528961,
            -2.046283006668091,
            1.7131365537643433
         ],
         "K":[  // Camera matrix (intrinsic parameters)
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0,
            0.0
         ],
         "P":[  // Projection matrix
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0
         ],
         "R":[  // Rectification matrix
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{  // Metadata about the camera frame
            "frame_id":"rgb_camera_link",  // Reference frame ID
            "seq":0,  // Sequence number
            "stamp":1693207028.365125  // Timestamp of the data
         },
         "height":1536,  // Height of the camera frame
         "width":2048  // Width of the camera frame
      },
      "head_camera_ir":{  // Infrared (IR) camera information
         "D":[
            3.894296407699585,  // Distortion coefficients
            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[  // Camera matrix (intrinsic parameters)
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[  // Projection matrix
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[  // Rectification matrix
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{  // Metadata about the camera frame
            "frame_id":"depth_camera_link",  // Reference frame ID
            "seq":0,  // Sequence number
            "stamp":1693207028.365125  // Timestamp of the data
         },
         "height":1024,  // Height of the camera frame
         "width":1024  // Width of the camera frame
      },
      "head_camera_rgb":{  // RGB camera information
         "D":[
            -0.36101600527763367,  // Distortion coefficients
            -2.2728121280670166,
            9.10795497475192e-05,
            -0.00020006165141239762,
            1.8348795175552368,
            -0.4750489294528961,
            -2.046283006668091,
            1.7131365537643433
         ],
         "K":[  // Camera matrix (intrinsic parameters)
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0,
            0.0
         ],
         "P":[  // Projection matrix
            972.0284423828125,
            0.0,
            1026.5511474609375,
            0.0,
            0.0,
            971.6822509765625,
            773.4451904296875,
            0.0
         ],
         "R":[  // Rectification matrix
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{  // Metadata about the camera frame
            "frame_id":"rgb_camera_link",  // Reference frame ID
            "seq":0,  // Sequence number
            "stamp":1693207028.3649921  // Timestamp of the data
         },
         "height":1536,  // Height of the camera frame
         "width":2048  // Width of the camera frame
      },
      "head_camera_rgb2depth":{  // Transformation from RGB camera to depth camera
         "D":[
            3.894296407699585,  // Distortion coefficients
            2.415846347808838,
            -1.6856218280736357e-05,
            0.00011720717884600163,
            0.12231486290693283,
            4.226081371307373,
            3.6851999759674072,
            0.6471517086029053
         ],
         "K":[  // Camera matrix (intrinsic parameters)
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0,
            0.0
         ],
         "P":[  // Projection matrix
            503.08038330078125,
            0.0,
            516.7129516601563,
            0.0,
            0.0,
            503.1611328125,
            509.8543395996094,
            0.0
         ],
         "R":[  // Rectification matrix
            1.0,
            0.0,
            0.0,
            0.0,
            1.0,
            0.0,
            0.0,
            0.0
         ],
         "header":{  // Metadata about the camera frame
            "frame_id":"depth_camera_link",  // Reference frame ID
            "seq":0,  // Sequence number
            "stamp":1693207028.3649921  // Timestamp of the data
         },
         "height":1024,  // Height of the camera frame
         "width":1024  // Width of the camera frame
      }
   }
}

```

## 5. `head_yaw_motor`\[TODO]

TODO@HermanYe: Waiting for the robot's head hardware to complete.

## 6. `head_pitch_motor`\[TODO]

TODO@HermanYe: Waiting for the robot's head hardware to complete.

## 7. `left_arm`\[TODO]

TODO@HermanYe: Waiting for the realman robot's dual-arm code bug to be fixed in the future, after which the left arm part will be completed.

## 8. `left_arm_camera`\[TESTED]

| hardware\_type    | command           |
| ----------------- | ----------------- |
| left\_arm\_camera | get\_camera\_info |

**Left Arm Camera Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| Index | Topic                                                                             |
| ----- | --------------------------------------------------------------------------------- |
| 1     | /left\_arm\_camera/color/camera\_info                                             |
| 2     | /left\_arm\_camera/color/image\_raw                                               |
| 3     | /left\_arm\_camera/color/image\_raw/compressed                                    |
| 4     | /left\_arm\_camera/color/image\_raw/compressed/parameter\_descriptions            |
| 5     | /left\_arm\_camera/color/image\_raw/compressed/parameter\_updates                 |
| 6     | /left\_arm\_camera/color/image\_raw/compressedDepth                               |
| 7     | /left\_arm\_camera/color/image\_raw/compressedDepth/parameter\_descriptions       |
| 8     | /left\_arm\_camera/color/image\_raw/compressedDepth/parameter\_updates            |
| 9     | /left\_arm\_camera/color/image\_raw/theora/parameter\_descriptions                |
| 10    | /left\_arm\_camera/color/image\_raw/theora/parameter\_updates                     |
| 11    | /left\_arm\_camera/color/metadata                                                 |
| 12    | /left\_arm\_camera/depth/camera\_info                                             |
| 13    | /left\_arm\_camera/depth/image\_rect\_raw                                         |
| 14    | /left\_arm\_camera/depth/image\_rect\_raw/compressed                              |
| 15    | /left\_arm\_camera/depth/image\_rect\_raw/compressed/parameter\_descriptions      |
| 16    | /left\_arm\_camera/depth/image\_rect\_raw/compressed/parameter\_updates           |
| 17    | /left\_arm\_camera/depth/image\_rect\_raw/compressedDepth                         |
| 18    | /left\_arm\_camera/depth/image\_rect\_raw/compressedDepth/parameter\_descriptions |
| 19    | /left\_arm\_camera/depth/image\_rect\_raw/compressedDepth/parameter\_updates      |
| 20    | /left\_arm\_camera/depth/image\_rect\_raw/theora/parameter\_descriptions          |
| 21    | /left\_arm\_camera/depth/image\_rect\_raw/theora/parameter\_updates               |
| 22    | /left\_arm\_camera/depth/metadata                                                 |
| 23    | /left\_arm\_camera/extrinsics/depth\_to\_color                                    |
| 24    | /left\_arm\_camera/realsense2\_camera\_manager/bond                               |
| 25    | /left\_arm\_camera/rgb\_camera/auto\_exposure\_roi/parameter\_descriptions        |
| 26    | /left\_arm\_camera/rgb\_camera/auto\_exposure\_roi/parameter\_updates             |
| 27    | /left\_arm\_camera/rgb\_camera/parameter\_descriptions                            |
| 28    | /left\_arm\_camera/rgb\_camera/parameter\_updates                                 |
| 29    | /left\_arm\_camera/stereo\_module/auto\_exposure\_roi/parameter\_descriptions     |
| 30    | /left\_arm\_camera/stereo\_module/auto\_exposure\_roi/parameter\_updates          |
| 31    | /left\_arm\_camera/stereo\_module/parameter\_descriptions                         |
| 32    | /left\_arm\_camera/stereo\_module/parameter\_updates                              |

### `get_camera_info`\[TESTED]

**Description:** The `get_camera_info` command is used to retrieve essential information from both the depth and color cameras of a specific hardware component, in this case, the left arm camera. **Send:**

```yaml
{
  "hardware_type": "left_arm_camera",
  "command": "get_camera_info",
  "parameters": {}
}
```

**Received:**

```yaml
{
    "hardware_type": "left_arm_camera",
    "command": "get_camera_info",
    "data": {
        "left_arm_camera_color": {
            "header": {
                "frame_id": "left_arm_camera_color_optical_frame",
                "seq": 0,
                "stamp": 1693190609.8870819
            },
            "height": 720,
            "width": 1280,
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],
            "K": [911.8970947265625, 0.0, 631.782470703125, 0.0, 911.279296875],
            "P": [911.8970947265625, 0.0, 631.782470703125, 0.0, 0.0],
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]
        },
        "left_arm_camera_depth": {
            "header": {
                "frame_id": "left_arm_camera_depth_optical_frame",
                "seq": 0,
                "stamp": 1693190609.6361418
            },
            "height": 720,
            "width": 1280,
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],
            "K": [884.7277221679688, 0.0, 642.0751953125, 0.0, 884.7277221679688],
            "P": [884.7277221679688, 0.0, 642.0751953125, 0.0, 0.0],
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]
        }
    },
    "message": "Camera info retrieved successfully.",
    "status_code": 0
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "left_arm_camera",     # Specifies the type of hardware component being interacted with (e.g., "left_arm_camera").
  "command": "get_camera_info",            # Specifies the specific command being sent.
  "parameters": {}                         
}

# Received
{
    "hardware_type": "left_arm_camera",     // Hardware type is specified as "left_arm_camera"
    "command": "get_camera_info",            // Command is set to "get_camera_info"
    "data": {                                // Data section contains camera information
        "left_arm_camera_color": {           // Camera information for color camera
            "header": {                       // Header information for color camera
                "frame_id": "left_arm_camera_color_optical_frame", // Frame ID for color camera
                "seq": 0,                       // Sequence number
                "stamp": 1693190609.8870819     // Timestamp
            },
            "height": 720,                     // Image height
            "width": 1280,                     // Image width
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],     // Distortion coefficients
            "K": [911.8970947265625, 0.0, 631.782470703125, 0.0, 911.279296875], // Camera matrix K
            "P": [911.8970947265625, 0.0, 631.782470703125, 0.0, 0.0],            // Projection matrix P
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]       // Rectification matrix R
        },
        "left_arm_camera_depth": {           // Camera information for depth camera
            "header": {                       // Header information for depth camera
                "frame_id": "left_arm_camera_depth_optical_frame", // Frame ID for depth camera
                "seq": 0,                       // Sequence number
                "stamp": 1693190609.6361418     // Timestamp
            },
            "height": 720,                     // Image height
            "width": 1280,                     // Image width
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],     // Distortion coefficients
            "K": [884.7277221679688, 0.0, 642.0751953125, 0.0, 884.7277221679688], // Camera matrix K
            "P": [884.7277221679688, 0.0, 642.0751953125, 0.0, 0.0],            // Projection matrix P
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]       // Rectification matrix R
        }
    },
    "message": "Camera info retrieved successfully.", // Status message
    "status_code": 0                          // Status code indicating success
}
```

## 9. `left_hand`\[TODO]

TODO@HermanYe: Waiting for the robot's hand hardware to complete.

## 10. `right_arm`

TODO@HermanYe: Fixing bug for dual arm, under testing, not available now.

## 11. `right_arm_camera`\[TESTED]

| hardware\_type     | command           |
| ------------------ | ----------------- |
| right\_arm\_camera | get\_camera\_info |

**Right Arm Camera Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| Index | Topic                                                                              |
| ----- | ---------------------------------------------------------------------------------- |
| 1     | /right\_arm\_camera/color/camera\_info                                             |
| 2     | /right\_arm\_camera/color/image\_raw                                               |
| 3     | /right\_arm\_camera/color/image\_raw/compressed                                    |
| 4     | /right\_arm\_camera/color/image\_raw/compressed/parameter\_descriptions            |
| 5     | /right\_arm\_camera/color/image\_raw/compressed/parameter\_updates                 |
| 6     | /right\_arm\_camera/color/image\_raw/compressedDepth                               |
| 7     | /right\_arm\_camera/color/image\_raw/compressedDepth/parameter\_descriptions       |
| 8     | /right\_arm\_camera/color/image\_raw/compressedDepth/parameter\_updates            |
| 9     | /right\_arm\_camera/color/image\_raw/theora/parameter\_descriptions                |
| 10    | /right\_arm\_camera/color/image\_raw/theora/parameter\_updates                     |
| 11    | /right\_arm\_camera/color/metadata                                                 |
| 12    | /right\_arm\_camera/depth/camera\_info                                             |
| 13    | /right\_arm\_camera/depth/image\_rect\_raw                                         |
| 14    | /right\_arm\_camera/depth/image\_rect\_raw/compressed                              |
| 15    | /right\_arm\_camera/depth/image\_rect\_raw/compressed/parameter\_descriptions      |
| 16    | /right\_arm\_camera/depth/image\_rect\_raw/compressed/parameter\_updates           |
| 17    | /right\_arm\_camera/depth/image\_rect\_raw/compressedDepth                         |
| 18    | /right\_arm\_camera/depth/image\_rect\_raw/compressedDepth/parameter\_descriptions |
| 19    | /right\_arm\_camera/depth/image\_rect\_raw/compressedDepth/parameter\_updates      |
| 20    | /right\_arm\_camera/depth/image\_rect\_raw/theora/parameter\_descriptions          |
| 21    | /right\_arm\_camera/depth/image\_rect\_raw/theora/parameter\_updates               |
| 22    | /right\_arm\_camera/depth/metadata                                                 |
| 23    | /right\_arm\_camera/extrinsics/depth\_to\_color                                    |
| 24    | /right\_arm\_camera/realsense2\_camera\_manager/bond                               |
| 25    | /right\_arm\_camera/rgb\_camera/auto\_exposure\_roi/parameter\_descriptions        |
| 26    | /right\_arm\_camera/rgb\_camera/auto\_exposure\_roi/parameter\_updates             |
| 27    | /right\_arm\_camera/rgb\_camera/parameter\_descriptions                            |
| 28    | /right\_arm\_camera/rgb\_camera/parameter\_updates                                 |
| 29    | /right\_arm\_camera/stereo\_module/auto\_exposure\_roi/parameter\_descriptions     |
| 30    | /right\_arm\_camera/stereo\_module/auto\_exposure\_roi/parameter\_updates          |
| 31    | /right\_arm\_camera/stereo\_module/parameter\_descriptions                         |
| 32    | /right\_arm\_camera/stereo\_module/parameter\_updates                              |

### `get_camera_info`\[TESTED]

**Description:** The `get_camera_info` command is used to retrieve essential information from both the depth and color cameras of a specific hardware component, in this case, the right arm camera. **Send:**

```yaml
{
  "hardware_type": "right_arm_camera",
  "command": "get_camera_info",
  "parameters": {}
}
```

**Received:**

```yaml
{
    "hardware_type": "right_arm_camera",
    "command": "get_camera_info",
    "data": {
        "right_arm_camera_color": {
            "header": {
                "frame_id": "right_arm_camera_color_optical_frame",
                "seq": 0,
                "stamp": 1693190609.8870819
            },
            "height": 720,
            "width": 1280,
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],
            "K": [911.8970947265625, 0.0, 631.782470703125, 0.0, 911.279296875],
            "P": [911.8970947265625, 0.0, 631.782470703125, 0.0, 0.0],
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]
        },
        "right_arm_camera_depth": {
            "header": {
                "frame_id": "right_arm_camera_depth_optical_frame",
                "seq": 0,
                "stamp": 1693190609.6361418
            },
            "height": 720,
            "width": 1280,
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],
            "K": [884.7277221679688, 0.0, 642.0751953125, 0.0, 884.7277221679688],
            "P": [884.7277221679688, 0.0, 642.0751953125, 0.0, 0.0],
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]
        }
    },
    "message": "Camera info retrieved successfully.",
    "status_code": 0
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "right_arm_camera",     # Specifies the type of hardware component being interacted with (e.g., "right_arm_camera").
  "command": "get_camera_info",            # Specifies the specific command being sent.
  "parameters": {}                         
}

# Received
{
    "hardware_type": "right_arm_camera",     // Hardware type is specified as "right_arm_camera"
    "command": "get_camera_info",            // Command is set to "get_camera_info"
    "data": {                                // Data section contains camera information
        "right_arm_camera_color": {           // Camera information for color camera
            "header": {                       // Header information for color camera
                "frame_id": "right_arm_camera_color_optical_frame", // Frame ID for color camera
                "seq": 0,                       // Sequence number
                "stamp": 1693190609.8870819     // Timestamp
            },
            "height": 720,                     // Image height
            "width": 1280,                     // Image width
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],     // Distortion coefficients
            "K": [911.8970947265625, 0.0, 631.782470703125, 0.0, 911.279296875], // Camera matrix K
            "P": [911.8970947265625, 0.0, 631.782470703125, 0.0, 0.0],            // Projection matrix P
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]       // Rectification matrix R
        },
        "right_arm_camera_depth": {           // Camera information for depth camera
            "header": {                       // Header information for depth camera
                "frame_id": "right_arm_camera_depth_optical_frame", // Frame ID for depth camera
                "seq": 0,                       // Sequence number
                "stamp": 1693190609.6361418     // Timestamp
            },
            "height": 720,                     // Image height
            "width": 1280,                     // Image width
            "D": [0.0, 0.0, 0.0, 0.0, 0.0],     // Distortion coefficients
            "K": [884.7277221679688, 0.0, 642.0751953125, 0.0, 884.7277221679688], // Camera matrix K
            "P": [884.7277221679688, 0.0, 642.0751953125, 0.0, 0.0],            // Projection matrix P
            "R": [1.0, 0.0, 0.0, 0.0, 1.0]       // Rectification matrix R
        }
    },
    "message": "Camera info retrieved successfully.", // Status message
    "status_code": 0                          // Status code indicating success
}
```

## 12. `right_hand`\[TODO]

TODO@HermanYe: Waiting for the robot's hand hardware to complete.

## 13. `lidar`\[TESTED]

| hardware\_type | command               |
| -------------- | --------------------- |
| lidar          | set\_lidar\_frequency |
| lidar          | get\_lidar\_frequency |

**Lidar Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.'

| index | topic        |
| ----- | ------------ |
| 1     | /livox/imu   |
| 2     | /livox/lidar |

### `set_lidar_frequency`\[TESTED]

**Description:**

The `set_lidar_frequency` command is utilized to configure and adjust the operational frequency of the lidar sensor. This command allows for the customization of the lidar sensor's sampling rate or frequency, specified in Hertz (Hz), to meet specific operational requirements or environmental conditions. **Send:**

```yaml
{
  "hardware_type": "lidar",
  "command": "set_lidar_frequency",
  "parameters": {
    "frequency": 10
  }
}
```

**Received:**

```yaml
{
    "hardware_type": "lidar",
    "command": "set_lidar_frequency",
    "data": null,
    "message": "Lidar frequency set successfully.",
    "status_code": 0
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "lidar",     # Specifies the type of hardware component being interacted with (e.g., "lidar").
  "command": "set_lidar_frequency",  # Specifies the specific command being sent.
  "parameters": {
    "frequency": 10            # Desired frequency for the lidar sensor (in Hz).
  }
}

# Received
{
    "hardware_type": "lidar",              // The type of hardware, which is lidar in this case
    "command": "set_lidar_frequency",      // The command to set the lidar frequency
    "data": null,                          // The data for setting the lidar frequency (null in this case)
    "message": "Lidar frequency set successfully.",   // A success message indicating that the lidar frequency was set successfully
    "status_code": 0                       // The status code, where 0 typically indicates success
}
```

### `get_lidar_frequency`\[TESTED]

**Description:** The `get_lidar_frequency` command is used to get the frequency of the lidar sensor. **Send:**

```yaml
{
  "hardware_type": "lidar",
  "command": "get_lidar_frequency",
  "parameters": {}
}
```

**Received:**

```yaml
{
    "command": "get_lidar_frequency",
    "data": "10.0",
    "hardware_type": "lidar",
    "message": "Lidar frequency get successfully.",
    "status_code": 0
}

```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "lidar",     # Specifies the type of hardware component being interacted with (e.g., "lidar").
  "command": "get_lidar_frequency",  # Specifies the specific command being sent.
  "parameters": {}              # No additional parameters required.
}

# Received
{
    "command": "get_lidar_frequency",  // The command to get the lidar frequency
    "data": "10.0",                    // The lidar frequency data (in Hertz)
    "hardware_type": "lidar",           // The type of hardware, which is lidar in this case
    "message": "Lidar frequency get successfully.",  // A success message indicating that the lidar frequency was retrieved successfully
    "status_code": 0                   // The status code, where 0 typically indicates success
}
```

## 14. `imu`\[TESTED]

| hardware\_type | command        |
| -------------- | -------------- |
| imu            | get\_imu\_data |

**IMU Topic List** High-frequency topics that are unilaterally received/published lead to enhanced efficiency by allowing direct subscription and publication mechanisms.

| index                      | topic             |
| -------------------------- | ----------------- |
| 1                          | /head\_camera/imu |
| 2                          | /livox/imu        |
| ## `get_imu_data`\[TESTED] |                   |

The `get_imu_data` command is used to retrieve the current data from the IMU sensor. Default IMU sensor is Livox Mid360 Lidar IMU. If you want to use IMU, please launch Livox lidar first.

**Send:**

```yaml
{
  "hardware_type": "imu",
  "command": "get_imu_data",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "imu",
  "command": "get_imu_data",
  "message": "IMU data retrieved successfully.",
  "status_code": 0
  "data": {
    "angular_velocity": {
      "x": 0.00963599793612957,
      "y": -0.007521912455558777,
      "z": -0.015086531639099121
    },
    "angular_velocity_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
    "header": {
      "frame_id": "livox_frame",
      "seq": 2792,
      "stamp": {
        "nsecs": 137065411,
        "secs": 1694254725
      }
    },
    "linear_acceleration": {
      "x": 0.010030509904026985,
      "y": 0.009747406467795372,
      "z": -0.9936594367027283
    },
    "linear_acceleration_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],
    "orientation": {
      "w": 0.0,
      "x": 0.0,
      "y": 0.0,
      "z": 0.0
    },
    "orientation_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
  },
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "imu",
  "command": "get_imu_data",
  "parameters": {}
}

# Received
{
  "hardware_type": "imu",
  "command": "get_imu_data",
  "status_code": 0,
  "message": "IMU data retrieved successfully.",
  "data": {
    "angular_velocity": {
      "x": 0.00963599793612957,   # Current angular velocity around the x-axis (rad/s)
      "y": -0.007521912455558777, # Current angular velocity around the y-axis (rad/s)
      "z": -0.015086531639099121  # Current angular velocity around the z-axis (rad/s)
    },
    "angular_velocity_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],  # Covariance matrix for angular velocity data
    "header": {
      "frame_id": "livox_frame",
      "seq": 2792,
      "stamp": {
        "nsecs": 137065411,
        "secs": 1694254725
      }
    },
    "linear_acceleration": {
      "x": 0.010030509904026985,   # Current linear acceleration along the x-axis (m/s^2)
      "y": 0.009747406467795372,   # Current linear acceleration along the y-axis (m/s^2)
      "z": -0.9936594367027283     # Current linear acceleration along the z-axis (m/s^2)
    },
    "linear_acceleration_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0],  # Covariance matrix for linear acceleration data
    "orientation": {
      "w": 0.0,   # Quaternion component w
      "x": 0.0,   # Quaternion component x
      "y": 0.0,   # Quaternion component y
      "z": 0.0    # Quaternion component z
    },
    "orientation_covariance": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0]  # Covariance matrix for orientation data
  }
}
```

## 15. `tool_gripper`\[TESTED]

### `inspire`\[TESTED]

The gripper of Inspire will automatically launch when the Realman arm is launched. It relies on the RS485 port of the end joint on the Realman arm. In the future, the gripper of inspire will be able to be used directly without needing the Realman arm's port.

| hardware\_type                                                                                                                                                                                                                                                                                  | command                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
| tool\_gripper\_inspire                                                                                                                                                                                                                                                                          | set\_gripper\_close          |
| tool\_gripper\_inspire                                                                                                                                                                                                                                                                          | set\_gripper\_open\_max      |
| tool\_gripper\_inspire                                                                                                                                                                                                                                                                          | set\_gripper\_open\_position |
| ### `set_gripper_close`\[TESTED]                                                                                                                                                                                                                                                                |                              |
| **Description:**                                                                                                                                                                                                                                                                                |                              |
| The `set_gripper_close` command is used to instruct a gripper tool on an Inspire robotic system to close with specific speed and force parameters. This command is especially useful when you need precise control over the gripping action to manipulate objects. Here are the key parameters: |                              |

* `speed` (Range: \[0, 1]): This parameter determines the desired speed at which the gripper should close. The range is from 0 (slowest) to 1 (fastest).
* `force` (Range: \[0.050, 1]): Specifies the desired force with which the gripper should close. The range is between 0.050 (minimal force) and 1 (maximum force). **Send:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_close",
  "parameters": {
    "speed": 0.854,
    "force": 0.100
  }
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_close",
  "status_code": 0,
  "message": "Gripper closed successfully.",
  "data": {}
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_gripper_inspire",        # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_close",         # Command to close the gripper to a specified position.
  "parameters": {
    "speed": 0.854,  # The desired speed to which the gripper should be closed. range:(0,1]
    "force": 0.100  # The desired force to which the gripper should be closed. range:(0.050,1]
  }
}

# Received
{
  "hardware_type": "tool_gripper_inspire",        # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_close",         # Command issued to close the gripper.
  "status_code": 0,                       # Status code: 0 indicates success.
  "message": "Gripper closed successfully.",  # Description message indicating the action was successful.
  "data": {}                               # No additional data provided.
}
```

#### `set_gripper_open_max`\[TESTED]

**Description:** The `set_gripper_open_max` command is utilized to instruct a gripper tool on an Inspire robotic system to fully open to its maximum position. This command is particularly useful when you need to ensure that the gripper is fully open before initiating a task or releasing an object. Here is the command breakdown:

* `speed` (Range: \[0.050, 1]): This optional parameter specifies the desired speed at which the gripper should open. The range is from 0.050 (slowest) to 1 (fastest). If not provided, the gripper will open at its default speed.

By sending this command, you can guarantee that the gripper opens to its maximum extent, providing a clear and open workspace for subsequent tasks or object release. The gripper will respond by fully opening, and you will receive a confirmation message upon successful execution.

**Send:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_open_max",
  "parameters": {
    "speed": 0.854
  }
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_open_max",
  "status_code": 0,
  "message": "Gripper opened to maximum position.",
  "data": {}
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_gripper_inspire",      # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_open_max",    # Command to fully open the gripper to its maximum position.
  "parameters": {}                      # No additional parameters required.
}

# Received
{
  "hardware_type": "tool_gripper_inspire",      # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_open_max",    # Command issued to fully open the gripper.
  "status_code": 0,                     # Status code: 0 indicates success.
  "message": "Gripper opened to maximum position.",  # Description message indicating the action was successful.
  "parameters": {
    "speed": 0.854  # The desired speed to which the gripper should be opened. range:(0.050,1]
  }
}
```

#### `set_gripper_open_position`\[TESTED]

**Description:** The `set_gripper_open_position` command is designed to open a gripper tool on an Inspire robotic system to a specified position. This command provides precise control over the gripper's opening action, allowing you to set the desired position to which the gripper should open. Here is a breakdown of the command:

* `position` (Range: \[0, 1]): This parameter specifies the desired position to which the gripper should be opened. The range is from 0 (fully closed) to 1 (fully open).

By using this command, you can ensure that the gripper opens to a specific position, providing fine-grained control for various gripping tasks. The gripper will respond by opening to the specified position, and you will receive a confirmation message upon successful execution.

**Send:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_open_position",
  "parameters": {
    "position": 0.500
  }
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_gripper_inspire",
  "command": "set_gripper_open_position",
  "status_code": 0,
  "message": "Gripper opened to the specified position.",
  "data": {}
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_gripper_inspire",          # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_open_position",   # Command to open the gripper to a specified position.
  "parameters": {
    "position": 0.500  # The desired position to which the gripper should be opened. range:(0,1]
  }
}

# Received
{
  "hardware_type": "tool_gripper_inspire",          # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_open_position",   # Command issued to open the gripper to a specified position.
  "status_code": 0,                         # Status code: 0 indicates success.
  "message": "Gripper opened to the specified position.",  # Description message indicating the action was successful.
  "data": {}                                # No additional data provided.
}
```

### `hitbot`

The code for Hitbot's gripper has already been completed and is ready for use. However, due to time constraints, the received JSON messages have not been documented in the JSON protocol book. If you want to test the gripper, please use a JSON client to receive feedback on the commands sent to the Hitbot gripper.

| hardware\_type        | command                              |
| --------------------- | ------------------------------------ |
| tool\_gripper\_hitbot | set\_gripper\_position\_speed\_force |
| tool\_gripper\_hitbot | get\_gripper\_position               |

#### set\_gripper\_position\_speed\_force

**Description:** The `set_gripper_position_speed_force` command allows precise control over a gripper tool on a Hitbot robot. This command enables you to set the gripper's position, speed, and force for a controlled gripping action. Here's a breakdown of the parameters:

* `position` (Range: \[0, 1]): Specifies the desired position to which the gripper should close. The range is between 0 (fully open) and 1 (fully closed).
* `speed` (Range: (0, 1]): Indicates the desired speed at which the gripper should close. The range is between 0 (slowest) and 1 (fastest).
* `force` (Range: (0, 1]): Specifies the desired force with which the gripper should close. The range is between 0 (minimal force) and 1 (maximum force).

Using this command, you can precisely control the gripper's behavior, making it suitable for various gripping tasks that require specific force, speed, and position settings. To execute this command, send a request with the `hardware_type` set to "tool\_gripper\_hitbot," the `command` as "set\_gripper\_position\_speed\_force," and provide the desired parameters as described above. Upon execution, the gripper will close to the specified position, speed, and force settings, allowing for accurate and controlled manipulation of objects.

**Send:**

```yaml
{
  "hardware_type": "tool_gripper_hitbot",
  "command": "set_gripper_position_speed_force",
  "parameters": {
    "position": 1.0,  
    "speed": 0.854,
    "force": 0.100
  }
}
```

**Received:**

```yaml
TODO
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_gripper_hitbot",        # Type of hardware being controlled (gripper tool).
  "command": "set_gripper_position_speed_force",         # Command to close the gripper to a specified position.
  "parameters": {
    "position": 1.0,  # The desired position to which the gripper should be closed.range:(0,1]
    "speed": 0.854,  # The desired speed to which the gripper should be closed. range:(0,1]
    "force": 0.100  # The desired force to which the gripper should be closed. range:(0,1]
  }
}

# Received
TODO
```

#### `get_gripper_position`

**Description:** The `get_gripper_position` command is used to retrieve the position of the robot's hitbot gripper, providing information about the current position of the gripper.

**Send:**

```yaml
{
  "hardware_type": "tool_gripper_hitbot",
  "command": "get_gripper_position",
  "parameters": {}
}
```

**Received:**

```yaml
TODO
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_gripper_hitbot",    # Specifies the type of hardware or component the command is intended for (in this case, tool_gripper_hitbot).
  "command": "get_gripper_position",   # The specific command to retrieve the gripper's position.
  "parameters": {}   # Any additional parameters or data needed for the command (none in this case).
}

# Received
TODO
```

## 16. `tool_sucker` \[TESTED]

| hardware\_type | command               |
| -------------- | --------------------- |
| tool\_sucker   | set\_sucker\_active   |
| tool\_sucker   | set\_sucker\_inactive |
| tool\_sucker   | get\_sucker\_status   |

### `set_sucker_active`\[TESTED]

**Description:** The `set_sucker_active` command is designed to activate the tool sucker hardware on the robot. This command is utilized to enable the tool sucker, which is typically used for gripping or holding objects.

**Send:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "set_sucker_active",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "set_sucker_active",
  "status_code": 0,
  "message": "Sucker activated successfully.",
  "data": null
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_sucker",   # Type of hardware being controlled
  "command": "set_sucker_active",   # Command to activate the tool sucker
  "parameters": {},                 # No additional parameters required
}

# Received
{
  "hardware_type": "tool_sucker",     # Type of hardware being controlled
  "command": "set_sucker_active",     # Command issued to the hardware
  "status_code": 0,                   # Status code: 0 indicates success
  "message": "Sucker activated successfully.",  # Description message indicating the action was successful
  "data": {}                          # No additional data provided
}
```

### `set_sucker_inactive`\[TESTED]

**Description:** The `set_sucker_inactive` command is designed to deactivate the tool sucker hardware component.

**Send:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "set_sucker_inactive",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "set_sucker_inactive",
  "status_code": 0,
  "message": "Sucker deactivated successfully.",
  "data": {}
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_sucker",     # Type of hardware being controlled
  "command": "set_sucker_inactive",   # Command to deactivate the tool sucker
  "parameters": {},                   # No additional parameters required
}

# Received
{
  "hardware_type": "tool_sucker",       # Type of hardware being controlled
  "command": "set_sucker_inactive",     # Command issued to the hardware
  "status_code": 0,                     # Status code: 0 indicates success
  "message": "Sucker deactivated successfully.",  # Description message indicating the action was successful
  "data": {}                            # No additional data provided
}
```

### `get_sucker_status`\[TESTED]

**Description:** The `get_sucker_status` command is used to query the status of a tool sucker hardware component. This command provides information about whether the suction activation process was successful and the current state of the tool sucker. The status is conveyed through a numeric status code and a corresponding status description for easy interpretation. Here is a breakdown of the possible status codes and their meanings:

* `0`: Object was successfully sucked during the suction activation process.
* `1`: No object was sucked during the suction activation process.
* `2`: Currently in release state, suction activation is not active.
* `3`: Object that was sucked during the suction activation process has accidentally fallen off.
* `4`: Suction activation has been successfully executed.
* `5`: Suction deactivation has been successfully executed.

This command is particularly useful for monitoring the performance and state of the tool sucker hardware, allowing you to take appropriate actions based on the reported status. To use this command, you send a request specifying the `hardware_type` as "tool\_sucker" and the `command` as "get\_sucker\_status" with no additional parameters. Upon successful execution, you will receive a response containing the hardware type, command issued, status code, a descriptive message, and detailed data, including the status code and description for easy integration into your robotic system.

| sucker\_status\_code | sucker\_status\_description                                                              |
| -------------------- | ---------------------------------------------------------------------------------------- |
| 0                    | Object was successfully sucked during the suction activation process                     |
| 1                    | No object was sucked during the suction activation process                               |
| 2                    | Currently in release state, suction activation is not active                             |
| 3                    | Object that was sucked during the suction activation process has accidentally fallen off |
| 4                    | Suction activation has been successfully executed                                        |
| 5                    | Suction deactivation has been successfully executed                                      |

**Send:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "get_sucker_status",
  "parameters": {}
}
```

**Received:**

```yaml
{
  "hardware_type": "tool_sucker",
  "command": "get_sucker_status",
  "status_code": 0,
  "message": "Sucker status retrieved successfully.",
  "data": {
    "sucker_status_description": "Object was successfully sucked during the suction activation process", 
    "sucker_status_code":0
  }
}
```

**Explanation:**

```yaml
# Send
{
  "hardware_type": "tool_sucker",     # Type of hardware being controlled
  "command": "get_sucker_status",     # Command to retrieve the tool sucker status
  "parameters": {}                    # No additional parameters required
}

# Received
{
  "hardware_type": "tool_sucker",       # Type of hardware being controlled
  "command": "get_sucker_status",       # Command issued to the hardware
  "status_code": 0,                     # Status code: 0 indicates success
  "message": "Sucker status retrieved successfully.",  # Description message indicating the action was successful
  "data": {
    "sucker_status_description": "Object was successfully sucked during the suction activation process",
    "sucker_status_code":0  # Check the description list of sucker_status_code
  }
}
```

## Reference

[sensor\_msgs/JointState.msg](http://docs.ros.org/en/melodic/api/sensor\_msgs/html/msg/JointState.html) [trajectory\_msgs/JointTrajectory.msg](http://docs.ros.org/en/noetic/api/trajectory\_msgs/html/msg/JointTrajectory.html) [control\_msgs/FollowJointTrajectoryAction.msg](http://docs.ros.org/en/electric/api/control\_msgs/html/msg/FollowJointTrajectoryAction.html) [geometry\_msgs/Pose.msg](http://docs.ros.org/en/noetic/api/geometry\_msgs/html/msg/Pose.html) [geometry\_msgs/PoseStamped.msg](http://docs.ros.org/en/noetic/api/geometry\_msgs/html/msg/PoseStamped.html)
