 

# 1. Overview

CR robots now support two remote control modes: **remote I/O mode** and **remote Modbus mode**. For details about the control mode, please see Software Usage Instructions > Settings > Remote Control in *Dobot-CR-Series Robot-App-User-Guide*. 

The above two modes are mainly for the **remote control of running scripts**. As the communication based on TCP/IP has high reliability, strong practicability and high performance with low cost, many industrial automation projects have a wide demand for control robots that support TCP/IP protocol. So CR robots, designed on the basis of TCP/IP protocol, provide rich interfaces for interaction with external devices.

Controller version requirements to support TCP/IP protocol, CR series controller version should be V3.5.1.19 or above, MG400/M1Pro controller version should be V1.5.5.0 or above.



# 2. Message Format

According to the design, CR robots will open 29999 and 30003 server ports; 29999 server port (hereinafter referred to as Dashboard port) is responsible for receiving some simple instructions by sending and receiving one by one. That is, after receiving the agreed message format from the client, the Dashboard port will give feedback to the client. 30003 server port (hereinafter referred to as the real-time feedback port) feeds back the robot information. It only receives the agreed message format from the client but does not give feedback. Port 30003 is split into port 30003 and port 30004. It is expected to be implemented in controller version 3.5.2. Controller version 3.5.1 currently has only one 30003 port for real-time feedback or sending motion commands.

30004 port (hereinafter referred to as real-time feedback port) feeds robot information every 8ms. 30005 port provides robot information every 200ms. 30006 port is a configurable port for feedback robot information. By default, 30006 port provides feedback every 50ms.

For example, the ENABLEROBOT() command, ENABLEROBOT() command, and ENABLEROBOT() command represent the same command and are executed as the enabling command.



## 2.1 Format for sending messages

​	`Message name(Param1,Param2,Param3……Paramn)`

The message format is shown as above. It consists of a message name, parameters in a bracket. Each parameter is separated by an English comma “,”. A  complete message ends up with right parenthesis. 

Both message commands and message responses are in ASCII format (string).



## 2.2 Format for Feedback

`"ErrorID,{value,...,valuen},`Message name(Param1,Param2,Param3……Paramn);"`

The message format is shown as above. If the ErrorID is 0, the command is received successfully.
A non-zero return indicates an error in the command. See Section 6 for a detailed description of the error.
{value1,value2,value3,...Valuen} represents the return value. If there is no return value, {} is returned.
Message name (Param1,Param2,Param3......Paramn) indicates the content delivered.

For example:

```
MovL(-500,100,200,150,0,90)
```


Return: 0,{},MovL(-500,100,200,150,0,90);   //0: received successfully. No return value.

```
Mov(-500,100,200,150,0,90)
```

Return: -10000,{},Mov(-500,100,200,150,0,90);   //-10000: command error no return value.



# 3. Communication Protocol—Dashboard Port

The upper computer can directly send some simple instructions to the robot through 29999 port. These instructions are defined by CR, and these functions are called Dashboard. The table below is the Dashboard instruction list. The Dashboard commands can be used to control the robot, including enabling/disabling and resetting the robot.

The commands related to robot setting are as follows: 

| Commands          | Supported products | Description                                                  |
| ----------------- | ------------------ | ------------------------------------------------------------ |
| EnableRobot       | CR\MG\M1Pro        | enable the robot                                             |
| DisableRobot      | CR\MG\M1Pro        | disable the robot                                            |
| ClearError        | CR\MG\M1Pro        | clear the error of the robot                                 |
| ResetRobot        | CR\MG\M1Pro        | the robot stops its current action, and receives enabling again |
| SpeedFactor       | CR\MG\M1Pro        | set the global speed ratio                                   |
| User              | CR\MG\M1Pro        | select the identified user coordinate system                 |
| Tool              | CR\MG\M1Pro        | select the identified tool coordinate system                 |
| RobotMode         | CR\MG\M1Pro        | mode of robot                                                |
| PayLoad           | CR\MG\M1Pro        | Set the load                                                 |
| DO                | CR\MG\M1Pro        | set the status of digital output port                        |
| DOExecute         | CR                 | set the status of digital output port (immediate command)    |
| ToolDO            | CR                 | Set terminal digital output port status                      |
| ToolDOExecute     | CR                 | Set terminal digital output port state (immediate instruction) |
| AO                | CR                 | Set the analog output port status                            |
| AOExecute         | CR                 | Set analog output port state (immediate instruction)         |
| AccJ              | CR\MG\M1Pro        | set the joint acceleration rate. This command is valid only when the motion mode is MovJ, MovJIO, MovJR,  JointMovJ |
| AccL              | CR\MG\M1Pro        | set the Cartesian acceleration rate. This command is valid only when the motion mode is MovL, MovLIO, MovLR, Jump, Arc, Circle |
| SpeedJ            | CR\MG\M1Pro        | set the joint velocity rate. This command is valid only when the motion mode is MovJ, MovJIO, MovJR,  JointMovJ |
| SpeedL            | CR\MG\M1Pro        | set the Cartesian velocity rate. This command is valid only when the motion mode is MovL, MovLIO, MovLR, Jump, Arc, Circle |
| Arch              | CR\MG\M1Pro        | set the index of arc parameters  (StartHeight, zLimit, EndHeight) in the Jump mode |
| CP                | CR\MG\M1Pro        | set the continuous path rate during movement                 |
| SetArmOrientation | CR                 | set the orientation of the arm                               |
| PowerOn           | CR                 | power on the robot                                           |
| RunScript         | CR\MG\M1Pro        | run the script                                               |
| StopScript        | CR\MG\M1Pro        | stop the script                                              |
| PauseScript       | CR\MG\M1Pro        | pause the script                                             |
| ContinueScript    | CR\MG\M1Pro        | continue the script                                          |
| SetSafeSkin       | CR                 | set the status of safe skin switch                           |
| GetTraceStartPose | CR                 | obtain the first point in trajectory fitting                 |
| GetPathStartPose  | CR                 | get the first point of the trajectory                        |
| PositiveSolution  | CR                 | Positive solution                                            |
| InverseSolution   | CR                 | inverse solution                                             |
| SetCollisionLevel | CR\MG\M1Pro        | Set collision level                                          |
| HandleTrajPoints  | CR                 | Track file preprocessing                                     |
| GetSixForceData   | CR                 | Obtain six dimensional force data                            |
| GetAngle          | CR\MG\M1Pro        | get the pose in the joint coordinate system                  |
| GetPose           | CR\MG\M1Pro        | get the pose in the Cartesian coordinate system              |
| EmergencyStop     | CR\MG\M1Pro        | emergency stop                                               |
| ModbusCreate      | CR\MG\M1Pro        | create the Modbus master and connect it to the slave         |
| ModbusClose       | CR\MG\M1Pro        | disconnect from Modbus slave                                 |
| GetInBits         | CR\MG\M1Pro        | Read discrete input function                                 |
| GetInRegs         | CR\MG\M1Pro        | Read input register                                          |
| GetCoils          | CR\MG\M1Pro        | Write coil register function                                 |
| SetCoils          | CR\MG\M1Pro        | Write coil register function                                 |
| GetHoldRegs       | CR\MG\M1Pro        | Read save register                                           |
| SetHoldRegs       | CR\MG\M1Pro        | Write save register                                          |
| GetErrorID        | CR\MG\M1Pro        | Get error ID                                                 |
| DI                | CR\MG\M1Pro        | Gets the status of the digital input port                    |
| ToolDI            | CR                 | Gets the status of the digital input port                    |
| AI                | CR                 | Get analog input port voltage                                |
| ToolAI            | CR                 | Get terminal analog input port voltage                       |
| DIGroup           | CR                 | Gets the state of a grop of digital output ports             |
| DOGroup           | CR                 | Sets the state of a grop of digital output ports             |
| BrakeControl      | CR                 | The brake control                                            |
| StartDrag         | CR                 | Start dragging                                               |
| StopDrag          | CR                 | Stop dragging                                                |
| SetCollideDrag    | CR                 | Force into drag mode                                         |
| SetTerminalKeys   | CR                 | Set the terminal key to enable                               |
| SetTerminal485    | CR                 | Set the parameters of terminal 485                           |
| GetTerminal485    | CR                 | Get the parameters of terminal 485                           |
| LoadSwitch        | CR                 | Sets the load setting state                                  |



## 3.1 EnableRobot

- Function: EnableRobot()

- Description: enable the robot

- Optional parameters: 

  Number of optional parameters: 0/1/4, indicating that no parameters, one parameter and four parameters. One parameter is the default load weight parameter, and four parameters are filled respectively indicating load, centerX, centerY, and centerZ.

| Parameter |  Type  | Description                                                  |
| --------- | :----: | ------------------------------------------------------------ |
| load      | double | Load weight kg.<br/>CR3/CR3L load range: 0~3kg;<br/>CR5/CR5D load range: 0~5kg;<br/>CR7 load range: 0~7kg;<br/>CR10 load range: 0~10kg;<br/>CR12 load range: 0~12kg;<br/>CR16 load range: 0~16kg;<br/>MG400 load range: 0~ 0.5kg;<br/>M1 Pro load range: 0~ 1.5kg; |
| centerX   | double | Eccentric distance in X direction, range: -500mm~500mm       |
| centerY   | double | Eccentric distance in Y direction, range: -500mm~500mm       |
| centerZ   | double | Eccentric distance in Z direction, range: -500mm~500mm       |

- Return: ErrorID, if the command operation is normal, the ErrorID returns 0. If the command operation fails, return an error code. Refer to Chapter 6.

- Supporting port: 29999

- Example
    ```
  EnableRobot()
  ```



## 3.2 DisableRobot

- Function: DisableRobot()

- Description: disable the robot

- Parameters: None

- Supporting port: 29999

- Example
    ```
  DisableRobot()
  ```



## 3.3 ClearError

- Function: ClearError()

- Description: clear the error of the robot. After clearing the alarm, the user can judge whether the robot is still in the alarm state according to the RobotMode. For the alarm that cannot be cleared, restart the control cabinet. (Refer to GetErrorID)

- Parameters: null

- Supporting port: 29999

- Example 
    ```
  ClearError()
  ```



## 3.4 ResetRobot

- Function: ResetRobot()

- Description: stop the robot

- Parameters: None

- Supporting port: 29999

- Example
    ```
  ResetRobot()
  ```



## 3.5 SpeedFactor

- Function: SpeedFactor(ratio)

- Description: set the global speed ratio

- Parameters: 

  | Parameter | Type | Description                                       |
  | --------- | ---- | ------------------------------------------------- |
  | ratio     | int  | speed ratio, range: 0~100, exclusive of 0 and 100 |

- Supporting port: 29999
  
- Example
  
  ```
  SpeedFactor(80)
  ```



## 3.6 User

- Function: User(index)

- Description: select the identified user coordinate system

- Parameters: 

  | Parameter | Type | Description                                              |
  | --------- | ---- | -------------------------------------------------------- |
  | index     | int  | select the identified user coordinate system, range: 0~9 |

- Return: ErrorID: -1 indicates that the user coordinate index does not exist.
  
- Supporting port: 29999
  
- Example
  
  ```
  User(1)
  ```



## 3.7 Tool

- Function: Tool(index)

- Description: select the identified tool coordinate system

- Parameters: 

  | Parameter | Type | Description                                              |
  | --------- | ---- | -------------------------------------------------------- |
  | index     | int  | select the identified tool coordinate system, range: 0~9 |

- ErrorID: -1 indicates that the tool coordinate index does not exist.
  
- Supporting port: 29999
  
- Example
  
  ```
  Tool(1)
  ```



## 3.8 **RobotMode**

- Function: RobotMode()

- Description: mode of robot. 

    In order to maintain compatibility with controller version 3.5.1, the return value of  robot status has not been modified. Such as: idle, drag, running, alarm state.

- Parameters: None

- Supporting port: 29999


- Return:                              

| Mode | Description           | NOTE                             |
| ---- | --------------------- | -------------------------------- |
| 1    | ROBOT_MODE_INIT       | initialization                   |
| 2    | ROBOT_MODE_BRAKE_OPEN | The brake release                |
| 3    |                       | Reserved                         |
| 4    | ROBOT_MODE_DISABLED   | Disabled (brake is not released) |
| 5    | ROBOT_MODE_ENABLE     | Enable (idle)                    |
| 6    | ROBOT_MODE_BACKDRIVE  | Dragging state                   |
| 7    | ROBOT_MODE_RUNNING    | Running state                    |
| 8    | ROBOT_MODE_RECORDING  | Dragging recording               |
| 9    | ROBOT_MODE_ERROR      | Alarm state                      |
| 10   | ROBOT_MODE_PAUSE      | pause state                      |
| 11   | ROBOT_MODE_JOG        | jogging state                    |

- Example
    ```
  RobotMode()
  ```
  



## 3.9 PayLoad

- Function: PayLoad(weight,inertia)

- Description: set the current load

- Parameters: 

  | Parameter | Type   | Description                                                  |
  | --------- | ------ | ------------------------------------------------------------ |
  | weight    | double | Load weight.<br/>CR3/CR3L load range: 0~3kg;<br/>CR5/CR5D load range: 0~5kg;<br/>CR7 load range: 0~7kg;<br/>CR10 load range: 0~10kg;<br/>CR12 load range: 0~12kg;<br/>CR16 load range: 0~16kg;<br/>MG400 load range: 0~ 0.5kg;<br/>M1 Pro load range: 0~ 1.5kg; |
  | inertia   | double | load inertia kgm²                                            |

- Supporting port: 29999
  
- Example
  
  ```
  PayLoad(3,0.4)
  ```
  
  Using LoadSet is the same as calling PayLoad. PayLoad is used with the LoadSwitch directive



## 3.10 DO

- Function: DO(index,status)

- Description: set the status of digital output port (queue command)

- Parameters: 

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | index     | int  | digital output index, range: 1 to 16 or 100 to 1000. The value ranges from 100 to 1000 when the hardware of the EXPANDED I/O module is supported |
  | status    | bool | status of the digital output port. 1: High level; 0: Low level |

- Supporting port: 29999
  
- Example
  
  ```
  DO(1,1)
  ```



## 3.11 DOExecute

- Function: DOExecute(index,status)

- Description: set the status of digital output port (immediate command)  

- Parameters: 

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | index     | int  | digital output index, range: 1 to 16 or 100 to 1000. The value ranges from 100 to 1000 when the hardware of the EXPANDED I/O module is supported |
  | status    | boo  | status of the digital output port. 1: High level; 0: Low level |

- Supporting port: 29999
  
- Example
  
  ```
  DOExecute(1,1)
  ```



## 3.12 ToolDO

- Function: ToolDO(index,status)

- Description: Set terminal digital output port state (queue instruction)

- Parameters: 

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | index     | int  | digital output index, range: 1 to 16 or 100 to 1000. The value ranges from 100 to 1000 when the hardware of the EXPANDED I/O module is supported |
  | status    | bool | status of digital output port. 1: high level, 0: low level   |

- Supporting port: 29999
  
- Example
  
  ```
  ToolDO(1,1)
  ```



## 3.13 ToolDOExecute

- Function: ToolDOExecute(index,status)

- Description: Set terminal digital output port state (immediate instruction)

- Parameters: 

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | index     | int  | digital output index, range: 1 or 2                          |
  | 0/1       | bool | status of the digital output port. 1: high level; 0: low level |

- Supporting port: 29999
  
- Example
  
  ```
  ToolDOExecute(1,1)
  ```



## 3.14 AO

- Function: AO(index,value)

- Description: set the voltage of analog output port of controller (queue command)

- Parameters: 

  | Parameter | Type   | Description                                 |
  | --------- | ------ | ------------------------------------------- |
  | index     | int    | analog output index, range: 1 or 2          |
  | value     | double | voltage of corresponding index, range: 0~10 |

- Supporting port: 29999
  
- Example
  
  ```
  AO(1,2)
  ```



## 3.15 AOExecute

- Function: AOExecute(index,value)

- Description: set the voltage of analog output port of controller (immediate command)

- Parameters: 

  | Parameter | Type   | Description                                 |
  | --------- | ------ | ------------------------------------------- |
  | index     | int    | analog output index, range: 1 or 2          |
  | value     | double | voltage of corresponding index, range: 0~10 |

- Supporting port: 29999
  
- Example
  
  ```
  AOExecute(1,2)
  ```



## 3.16 AccJ

- Function: AccJ(R)

- Description: set the joint acceleration rate. This command is valid only when the motion mode is MovJ, MovJIO, MovJR,  JointMovJ

- Parameters: 

  | Parameter | Type | Description                           |
  | --------- | ---- | ------------------------------------- |
  | R         | int  | joint acceleration rate, range: 1~100 |

- Supporting port: 29999
  
- Example
  
  ```
  AccJ(50)
  ```



## 3.17 AccL

- Function: AccL(R)

- Description: set the Cartesian acceleration rate. This command is valid only when the motion mode is MovL, MovLIO, MovLR, Jump, Arc, Circle

- Parameters: 

  | Parameter | Type | Description                               |
  | --------- | ---- | ----------------------------------------- |
  | R         | int  | Cartesian acceleration rate, range: 1~100 |

- Supporting port: 29999
  
- Example
  
  ```
  AccL(50)
  ```



##  3.18 SpeedJ

- Function: SpeedJ(R)

- Description: set the joint velocity rate. This command is valid only when the motion mode is MovJ, MovJIO, MovJR,  JointMovJ

- Parameters: 

  | Parameter | Type | Description                       |
  | --------- | ---- | --------------------------------- |
  | R         | int  | joint velocity rate, range: 1~100 |

- Supporting port: 29999
  
- Example
  
  ```
  SpeedJ(50)
  ```



## 3.19 SpeedL

- Function: SpeedL(R)

- Description: set the Cartesian velocity rate. This command is valid only when the motion mode is MovL, MovLIO, MovLR, Jump, Arc, Circle

- Parameters: 

  | Parameter | Type | Description                           |
  | --------- | ---- | ------------------------------------- |
  | R         | int  | Cartesian velocity rate, range: 1~100 |

- Supporting port: 29999
  
- Example
  
  ```
  SpeedL(50)
  ```



## 3.20 Arch

- Function: Arch(Index)

- Description: set the index of arc parameters  (StartHeight, zLimit, EndHeight) in the Jump mode

- Parameters: 

  | Parameter | Type | Description                      |
  | --------- | ---- | -------------------------------- |
  | Index     | int  | arc parameters index, range: 0~9 |

- Supporting port: 29999
  
- Example
  
  ```
  Arch(1)
  ```



## 3.21 CP

- Function: CP(R)

- Description: set CP rate. CP means continuous path, that is, when the robot arm reaches the end point from the starting point through the intermediate point, it passes through the intermediate point in a right angle or in a curve. This command is invalid for Jump mode.

- Parameters: 

  | Parameter | Type | Description                        |
  | --------- | ---- | ---------------------------------- |
  | R         | int  | continuous path rate, range: 1~100 |

- Supporting port: 29999
  
- Example
  
  ```
  CP(50)
  ```



## 3.22 LimZ

- Function: LimZ(zValue)
- Description: set the maximum lifting height in Jump mode


- Parameters: 

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | zValue    | int  | maximum lifting height which cannot exceed the Z-axis limiting position of the robot |

- Supporting port: 29999
  
- Example
  
  ```
  LimZ(80)
  ```



## 3.23 SetArmOrientation

- Function: SetArmOrientation(LorR,UorD,ForN,Config6)
- Description: set the orientation of the arm


- Parameters: 

  Number of optional parameters: 1 or 4. One parameter indicates that it is M1Pro model, indicating that it is right-handed. Four parameters indicate CR series parameters. If other parameters are specified, an error message is displayed.

  | Parameter | Type | Description                                                  |
  | --------- | ---- | ------------------------------------------------------------ |
  | LorR      | int  | Arm direction: forward/backward (1/-1)<br />1: Forward (for M1Pro, you need to set 0 for left hand system)<br/>-1: backward (for M1Pro, you need to set 1 for right hand system) |
  | UorD      | int  | Arm direction: up the elbow/down the elbow (1/-1)<br />1: up the elbow<br />-1: down the elbow |
  | ForN      | int  | Whether the wrist is reversed (1/-1)<br />1: wrist is not reversed<br />-1: wrist is reversed |
  | Config6   | int  | Sixth axis Angle sign<br />-1,-2...<br />-1: Axis 6 Angle is [0,-90],  Config6 is -1;<br/>-2: Axis 6 Angle is [90, 180], and so on<br/> 1,2...<br/>1: axis 6 Angle is [0,90], Config6 is 1;<br/>2: axis 6 Angle is [90180], Config6 is 2, and so on<br/> |

- Supporting port: 29999
  
- Example
  
  Set the orientation of CR product
  
  ```
  SetArmOrientation(1,1,-1,1)
  ```
  
  Set the orientation of M1Pro
  
  ```
  SetArmOrientation(0)
  ```
  
  
  
## 3.24 PowerOn

- Function: PowerOn()
- Description: Power on the robot. After the robot is powered on, wait about 10 seconds before enabling it.
- Parameters: None
- Supporting port: 29999

  **Note: Once the robot is powered on, you can enable the robot after about 10 seconds.**


- Example
    ```
  PowerOn()
  ```



## 3.25 RunScript

- Function: RunScript(projectName)

- Description: run the script 

- Parameters: 

  | Parameter   | Type   | Description |
  | ----------- | ------ | ----------- |
  | projectName | string | script name |


- Supporting port: 29999
  
- Example
  
  ```
  RunScript(demo)
  ```



## 3.26 StopScript

- Function: StopScript()

- Description: stop the script

- Parameters: None

- Supporting port: 29999

- Example
    ```
  StopScript()
  ```



## 3.27 PauseScript

- Function: PauseScript()

- Description: pause the script

- Parameters: None

- Supporting port: 29999

- Example
    ```
  PauseScript()
  ```



## 3.28 ContinueScript

- Function: ContinueScript()

- Description: continue the script

- Parameters: None

- Supporting port: 29999

- Example
    ```
  ContinueScript()
  ```



## 3.29 SetSafeSkin

- Function: SetSafeSkin(status)

- Description: Set the state of safe skin 

- Parameters：

  | Parameter | Parameter | Description                                                  |
  | --------- | --------- | ------------------------------------------------------------ |
  | status    | int       | status: safe skin status, 0: Turn off safe skin; 1: Open safe skin |


- Supporting port: 29999

- Example

  Open safe skin

  ```
  SetSafeSkin (1)
  ```

  

## 3.30 GetTraceStartPose

- Function: GetTraceStartPose(traceName)

- Description: get the first point of the trajectory. This instruction is supported in CR controller version.

- Parameters：

  | Parameter | Parameter | Description                                   |
  | --------- | --------- | --------------------------------------------- |
  | traceName | string    | name of the trajectory file (with the suffix) |


- Supporting port:29999  

- Example

  ```
  GetTraceStartPose(recv_string)
  ```



## 3.31 GetPathStartPose

- Function: GetPathStartPose(traceName)

- Description: get the first point in trajectory playback. This instruction is supported in CR controller version. 3.5.2 and above.

- Parameters：

  | Parameter | Type   | Description                                   |
  | --------- | ------ | --------------------------------------------- |
  | traceName | string | name of the trajectory file (with the suffix) |


- Supporting port: 29999

- Example

  ```
  GetPathStartPose(recv_string)
  ```

  

## 3.32 PositiveSolution

- Function: PositiveSolution(J1,J2,J3,J4,J5,J6,User,Tool)

- Description: Positive solution. Given the angle of each joint of the robot, the spatial position of the end of the robot is calculated. The arm direction of the robot is required to be known by SetArmOrientation

- Parameters：

  | Parameter | Type   | Description                                  |
  | --------- | ------ | -------------------------------------------- |
  | J1        | double | Position of axis J1 in degrees               |
  | J2        | double | Position of axis J2 in degrees               |
  | J3        | double | Position of axis J3 in degrees               |
  | J4        | double | Position of axis J4 in degrees               |
  | J5        | double | Position of axis J5 in degrees               |
  | J6        | double | Position of axis J6 in degrees               |
  | User      | int    | Select the calibrated user coordinate system |
  | Tool      | int    | Select the calibrated tool coordinate system |

- Return: Spatial position

- Supporting port: 29999

- Example

  ```
  PositiveSolution(0,0,-90,0,90,0,1,1)
  ```

  

  ## 3.33 InverseSolution

- Function: InverseSolution(X,Y,Z,Rx,Ry,Rz,User,Tool,isJointNear,JointNear)

- Description:  The inverse solution. The position and attitude of the end of the robot are known, and the angle values of each joint of the robot are calculated.

- Parameters：

  - Necessary parameters

  | Parameter | Description                                  | Type   |
  | --------- | -------------------------------------------- | ------ |
  | X         | X-axis position, unit: mm                    | double |
  | Y         | Y-axis position, unit: mm                    | double |
  | Z         | Z-axis position, unit: mm                    | double |
  | Rx        | Position of the Rx axis, units: degree       | double |
  | Ry        | Position of the Ry axis, units: degree       | double |
  | Rz        | Position of the Rz axis, units: degree       | double |
  | User      | Select the calibrated user coordinate system | int    |
  | Tool      | Select the calibrated tool coordinate system | int    |

    - Optional parameters：

  | Parameter   | Description                                                  | Type   |
  | ----------- | ------------------------------------------------------------ | ------ |
  | isJointNear | Whether to choose the Angle solution. If the value is 1, JointNear data is valid. If the value is 0, JointNear data is invalid. The algorithm selects solutions according to the current Angle. The default value is 0. | int    |
  | JointNear   | Select  the Angle values of six joints                       | string |

- Return：angle values of each joint

- Supporting port: 29999

- Example

  Deliver the Cartesian coordinate value without the selected joint angle to return the joint angle value of the robot.

  ```
  InverseSolution(473.000000,-141.000000,469.000000,-180.000000,0.000,-90.000,0,0)
  ```

  return: 0,{0,0,-90,0,90,0},InverseSolution(473.000000,-141.000000,469.000000,

  -180.000000,0.000,-90.000,0,0);

  Deliver the Cartesian coordinate value of the selected joint angle to return the joint angle value of the robot:

  ```
  InverseSolution(473.000000,-141.000000,469.000000,-180.000000,0.000,-90.000,0,0,1,{0,0,-90,0,90,0})
  ```

  return: 0,{0,0,-90,0,90,0},InverseSolution(0,-247,1050,-90,0,180,0,0,1,{0,0,-90,0,90,0});

  


  ## 3.34 SetCollisionLevel

  - Function: SetCollisionLevel(level)

  - Description: set the collision level. 

  - Parameters

    | Parameters | Type | Description                                                  |
    | ---------- | ---- | ------------------------------------------------------------ |
    | level      | int  | level: collision level<br /> 0: switch off collision detection<br /> 1~5: more sensitive with higher level |


  - Supporting port: 29999

  - Example

    ```
    SetCollisionLevel(1)
    ```

    

  ## 3.35 HandleTrajPoints

  - Function: HandleTrajPoints(traceName)

  - Description: Preprocessing of trajectory files. This instruction is supported in CR controller version 3.5.2 and above.

  - Parameters：

    | Parameters | Type   | Description                                   |
    | ---------- | ------ | --------------------------------------------- |
    | traceName  | string | name of the trajectory file (with the suffix) |

  - Return：ErrorID

  - Supporting port: 29999

  - Example

    Recv_string is delivered for preprocessing, and the preprocessing result is queried at a certain period.

    ```
    HandleTrajPoints(recv_string)
    HandleTrajPoints()
    ```



## 3.36 GetSixForceData

- Function: GetSixForceData()

- Description: get six-axis force data

- Parameters: None

- Return: six-axis force data

- Supporting port: 29999

- Example

  ```
  GetSixForceData()
  ```



## 3.37 GetAngle

- Function: GetAngle()

- Description: get the current pose of the robot under the Joint coordinate system

- Parameters: None

- Return: joint coordinate of the current pose

- Supporting port: 29999

- Example

  ```
  GetAngle()
  ```

  

## 3.38 GetPose

- Function: GetPose()

- Description: get the current pose of the robot under the Cartesian coordinate system

  If you have set the User or Tool coordinate system, the current pose is under the current User or Tool coordinate system

- Parameters：None

- Return: pose of the robot in the joint coordinate system

- Supporting port: 29999

- Example

  ```
  GetPose()
  ```

  

## 3.39 EmergencyStop

- Function: EmergencyStop()

- Description: Emergency Stop

- Parameters：None

- Supporting port: 29999

- Example

  ```
  EmergencyStop()
  ```

  


## 3.40 ModbusCreate

- Function: ModbusCreate(ip,port,slave_id,isRTU)

- Description: create Modbus master station, and establish connection with the slave station. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters：

  | Parameters | Type   | Description                                                  |
  | ---------- | ------ | ------------------------------------------------------------ |
  | ip         | string | IP address of slave station                                  |
  | port       | int    | slave station port                                           |
  | slave_id   | int    | ID of slave station                                          |
  | isRTU      | int    | This parameter is optional. The value range is 0/1.<br/>If the value is null or 0, establish modbusTCP communication.<br/>If it is 1, establish modbusRTU communication. |


- Return: ErrorID, {index}

  ErrorID: 0 indicates that the Modbus master station is created successfully. -1 indicates that the Modbus master station fails to be created. For other values, refer to the error code description

  index: master station index. Supports a maximum of 5 devices. The value ranges from 0 to 4.

- Supporting port: 29999

- Example

  Establish RTU communication master station (60000 terminal transparent port)

  ```
  ModbusCreate(127.0.0.1,60000,1,true)
  ```

  

## 3.41 ModbusClose

- Function: ModbusClose(index)

- Description: disconnect with Modbus slave station. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters：

  | Parameters | Type   | Description    |
  | ---------- | ------ | -------------- |
  | index      | string | Internal index |


- Supporting port: 29999

- Example

  ```
  ModbusClose(0)
  ```

  

## 3.42 GetInBits

- Function: GetInBits(index,addr,count)

- Description: Read discrete input data. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters：

  | Parameters | Type | Description                                  |
  | ---------- | ---- | -------------------------------------------- |
  | index      | int  | Internal index                               |
  | addr       | int  | Depending on the slave station configuration |
  | count      | int  | The value ranges from 1 to 16                |


- Return: get results by bit

- Supporting port: 29999

- Example

  ```
  GetInBits(0,3000,5)
  ```

  Normal: 0,{1,0,1,1,0},GetInBits(0,3000,5);

  Error：-1,{},GetInBits(0,3000,5);



## 3.43 GetInRegs

- Function: GetInRegs(index,addr,count,valType)

- Description: read the input register value. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type   | Description                                                  |
  | ---------- | ------ | ------------------------------------------------------------ |
  | index      | int    | Internal index                                               |
  | addr       | int    | Depending on the slave station configuration                 |
  | count      | int    | The value ranges from 1 to 4                                 |
  | valType    | string | Optional parameters<br />U16: read 16-bit unsigned integer ( two bytes, occupy one register)<br />U32: read 32-bit unsigned integer (four bytes, occupy two registers)<br />F32: read 32-bit single-precision floating-point number (four bytes, occupy two registers)<br />F64: read 64-bit double-precision floating-point number (eight bytes, occupy four registers) |


- Return: Returns by variable type {value1,value2...,valuen}

- Supporting port: 29999

- Example

  ```
  GetInRegs(0,4000,3)
  ```

  Normal: 0,{5,18,12},GetInRegs(0,4000,3);

  Error: -1,{},GetInRegs(0,4000,3);

  

## 3.44 GetCoils

- Function: GetCoils(index,addr,count)

- Description: read the coil register. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type | Description                                  |
  | ---------- | ---- | -------------------------------------------- |
  | index      | int  | Internal index                               |
  | addr       | int  | Depending on the slave station configuration |
  | count      | int  | The value ranges from 1 to 16                |


- Return: Returns by variable type {value1,value2...,valuen}

- Supporting port: 29999 

- Example

  ```
  GetCoils(0,1000,3)
  ```

  Normal: 0,{1,1,0},GetCoils(0,1000,3);

  Error: -1,{},GetCoils(0,1000,3);

  

## 3.45 SetCoils

- Function: SetCoils(index,addr,count,valTab)

- Description: write the coil register. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type   | Description                                  |
  | ---------- | ------ | -------------------------------------------- |
  | index      | int    | Internal index                               |
  | addr       | int    | Depending on the slave station configuration |
  | count      | int    | The value ranges from 1 to 16                |
  | valTab     | string | address of the coils                         |


- Return: ErrorID, 0 indicates normal, and -1 indicates that the configuration fails.

- Supporting port: 29999

- Example

  ```
  SetCoils(0,1000,3,{1,0,1})
  ```

  Normal: 0,{},SetCoils(0,1000,3,{1,0,1});

  Error: -1,{},SetCoils(0,1000,3,{1,0,1});

  

## 3.46 GetHoldRegs

- Function: GetHoldRegs(index,*addr*, *count*,valType)

- Description: read the holding register. This instruction is supported in CR controller version 3.5.2 and above.

- Example

  | Parameters | Type   | Description                                                  |
  | ---------- | ------ | ------------------------------------------------------------ |
  | index      | int    | Internal index, supporting at most five devices, range: 0~4  |
  | addr       | int    | address of the holding registers. Depending on the slave station configuration |
  | count      | int    | number of the holding registers                              |
  | valType    | string | U16: read 16-bit unsigned integer ( two bytes, occupy one register)<br />U32: read 32-bit unsigned integer (four bytes, occupy two registers)<br />F32: read 32-bit single-precision floating-point number (four bytes, occupy two registers)<br />F64: read 64-bit double-precision floating-point number (eight bytes, occupy four registers) |


- Return: Returns by variable type {value1,value2...,valuen}

- Supporting port: 29999

- Example

  Reads a 16-bit unsigned integer starting at address 3095

  ```
  GetHoldRegs(0,3095,1)
  ```

    Normal: 0,{13},GetHoldRegs(0,3095,1);

    Error: -1,{},GetHoldRegs(0,3095,1);

  

## 3.47 SetHoldRegs

- Function: SetHoldRegs(index,*addr*, *count*,*valTab*,valType)

- Description: write the holding register. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type   | Description                                                  |
  | ---------- | ------ | ------------------------------------------------------------ |
  | index      | int    | Internal index, supporting at most five devices, range: 0~4  |
  | addr       | int    | address of the holding registers. Depending on the slave station configuration |
  | count      | int    | number of the holding registers to read. The value ranges from 1 to 4. |
  | valTab     | string | number of the holding registers                              |
  | valType    | string | U16: read 16-bit unsigned integer ( two bytes, occupy one register)<br />U32: read 32-bit unsigned integer (four bytes, occupy two registers)<br />F32: read 32-bit single-precision floating-point number (four bytes, occupy two registers)<br />F64: read 64-bit double-precision floating-point number (eight bytes, occupy four registers) |


- Return: ErrorID, 0 indicates normal, and -1 indicates that the configuration fails.

- Supporting port: 29999

- Example

  Starting at address 3095, write two 16-bit unsigned integer values 6000,300

  ```
  SetHoldRegs(0,3095,2,{6000,300}, U16)
  ```

  Normal: 0,{},SetHoldRegs(0,3095,2,{6000,300}, U16);

  Error: -1,{},SetHoldRegs(0,3095,2,{6000,300}, U16);

  

## 3.48 GetErrorID

- Function: GetErrorID()
- Description: Get the robot error code.  This instruction is supported in CR controller version 3.5.2 and above.
- Parameters：None
- Supporting port: 29999
- Return: controller and algorithm alarm information, collision detection value is -2, electronic skin collision detection value is -3;
  The last six represent the alarm information of six servos respectively.


- Example

  ```
  GetErrorID()
  ```

  Return：

   0,{[[-2],[],[],[],[],[]]},GetErrorId();

  


## 3.49 DI

- Function: DI(index)

- Description: get the status of the digital input port.

- Parameters

  | Parameters | Type | Description                                                  |
  | ---------- | ---- | ------------------------------------------------------------ |
  | index      | int  | digital input index, range: 1~32  or 100 - 1000. The value range is 100-1000 only when you configure the extended I/O module |


- Return: the current index value status. The value range is 0/1.

- Supporting port: 29999

- Example

  ```
  DI(1)
  ```

  Return: digital input port 1 is low level

  0,{0},DI(1);



## 3.50 ToolDI

- Function: ToolDI(index)

- Description: gets terminal digital quantity input port status.

- Parameters

  | Parameters | Type | Description                        |
  | ---------- | ---- | ---------------------------------- |
  | index      | int  | digital input index, range: 1 or 2 |


- Return: status input port corresponding to the index, range: 1 or 0

- Supporting port: 29999

- Example

  ```
  ToolDI(2)
  ```

  

## 3.51 AI

- Function: AI(index)

- Description: get the voltage of analog input port of controller (immediate command).

- Parameters

  | Parameters | Type | Description                        |
  | ---------- | ---- | ---------------------------------- |
  | index      | int  | index of controller, range: 1 or 2 |


- Return: voltage of corresponding index

- Supporting port: 29999

- Example

  Get the voltage of analog input port 2 on the controller.

  ```
  AI(2)
  ```

  

## 3.52 ToolAI

- Function: ToolAI(index)

- Description: get the voltage of terminal analog input  (immediate command).

  | Parameters | Type | Description                                   |
  | ---------- | ---- | --------------------------------------------- |
  | index      | int  | index of terminal analog input, range: 1 or 2 |



- Return: voltage of corresponding index

- Supporting port: 29999

- Example

  ```
  ToolAI(1)
  ```

  Return: voltage of terminal analog input 1 is 1.5V:

  0,{1.5},ToolAI(1);



## 3.53 DIGroup

- Function: DIGroup(index1,index2,...,indexn)

- Description: get the state of a group of digital input ports

- Parameters

  | Parameters | Type | Description                                                  |
  | ---------- | ---- | ------------------------------------------------------------ |
  | index1     | int  | index of digital input port, range: 1~32  or 100 - 1000. The value range is 100-1000 only when you configure the extended I/O module |
  | ...        | ...  | ...                                                          |
  | indexn     | int  | index of digita input port, range: 1~32  or 100 - 1000. The value range is 100-1000 only when you configure the extended I/O module |


- Return: the current voltage value from index1 to Indexn

- Supporting port: 29999

- Example

  ```
  DIGroup(4,6,2,7)
  ```

  Gets the level of input ports 4, 6, 2, 7

  

## 3.54 DOGroup

- Function: DOGroup(index1,value1,index2,value2,...,indexn,valuen)

- Description: set the state of a group of digital output ports

- Parameters: the maximum number of parameters is 64

  The value range is 100-1000 only when you configure the extended I/O module

  | Parameters | Type | Description                                                |
  | ---------- | ---- | ---------------------------------------------------------- |
  | index1     | int  | index of digita output port, range: 1~16  or 100 - 1000.   |
  | value1     | int  | the status of the digital output port. The value is 0 or 1 |
  | ...        | ...  | ...                                                        |
  | indexn     | int  | index of digita output port, range: 1~16  or 100 - 1000.   |
  | valuen     | int  | the status of the digital output port. The value is 0 or 1 |


- Supporting port: 29999

- Example

  Set output ports 4, 6, 2, and 7 to 1, 0, 1, and 0 respectively

  ```
  DOGroup(4,1,6,0,2,1,7,0)
  ```

  


## 3.55 BrakeControl

- Function: BrakeControl(axisID,value)     

- Description: Control brake. The control of the brake should be carried out under the condition that the robot is enabled. This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type | Description                                                |
  | ---------- | ---- | ---------------------------------------------------------- |
  | axisID     | int  | ID of the joint axis                                       |
  | value      | int  | state of brake. 0: disables the brake. 1: enable the brake |

- Return: ErrorID, the control of locking brake should be carried out under the condition that the robot is enabled, otherwise the robot will return -1 by error.

- Supporting port: 29999

- Example

  Open the brake on joint 1.

  ```
  BrakeControl(1,1)
  ```

  

## 3.56 StartDrag

- Function: StartDrag()

- Description: Enter drag (in error state, can not enter drag). This instruction is supported in CR controller version 3.5.2 and above.

- Parameters: None

- Supporting port: 29999

- Example

  ```
  StartDrag()
  ```

  

## 3.57 StopDrag

- Description: Stop Drag. This instruction is supported in CR controller version 3.5.2 and above.

- Function: StopDrag()

- Parameters: None

- Supporting port: 29999

- Example

  ```
  StopDrag()
  ```

  

## 3.58 SetCollideDrag

- Function: SetCollideDrag(status)

- Description: Set whether drag is forced to enter (can enter drag even in error state). This instruction is supported in CR controller version 3.5.2 and above.

- Parameters

  | Parameters | Type | Description                                                  |
  | ---------- | ---- | ------------------------------------------------------------ |
  | status     | int  | status: force drag switch state, 0: disables the brake. 1: enable the brake |


- Supporting port: 29999

- Example

  Force entry drag.

  ```
  SetCollideDrag(0)
  ```

  

## 3.59 SetTerminalKeys

- Function: SetTerminalKeys(status)

- Description: Set the terminal button to enable. This command is supported only in certain versions.

- Parameters: 

  | Parameters | Type | Description                                      |
  | ---------- | ---- | ------------------------------------------------ |
  | status     | int  | status of terminal button. 0: disable, 1: enable |


- Supporting port: 29999

- Example

  The terminal button is disabled

  ```
  SetTerminalKeys(0)
  ```



## 3.60 SetTerminal485

- Function: SetTerminal485(baudRate, dataLen, parityBit, stopBit)

- Description: set the terminal 485 parameter. This command is supported only in certain versions.

- Parameters

  | Parameters | Type   | Description                                       |
  | ---------- | ------ | ------------------------------------------------- |
  | baudRate   | int    | baudRate：baud rate                               |
  | dataLen    | int    | dataLen：data bit, currently fixed to 8           |
  | parityBit  | string | parityBit: it is fixed to N, indicating no parity |
  | stopBit    | int    | stopBit: stop bit, currently fixed to 1           |


- 

- Example

  Set the baud rate to 115200

  ```
  SetTerminal485(115200, 8, N, 1)
  ```

  

## 3.62 GetTerminal485

- Function: GetTerminal485()

- Description: get the terminal 485 parameter. This command is supported only in certain versions.

- Parameters: None

- Return: {baud rate, data bit, parity bit, stop bit}

- Supporting port: 29999

- Example

  ```
  GetTerminal485()
  ```

  


## 3.63 LoadSwitch

- Function: LoadSwitch(status)

- Description: set the load setting state.

- Parameters

  | Parameters | Type | Description                                                  |
  | ---------- | ---- | ------------------------------------------------------------ |
  | status     | int  | status: set the load setting state, 0: off; 1: on. Enabling load Settings increases collision sensitivity |

- Supporting port: 29999

- Example

  ```
  LoadSwitch(1) 
  ```

   


# 4. Communication Protocol—Real- time Feedback Port

30004 port is the real-time feedback port (30004, 30005, and 30006 ports are supported by controller 3.5.2 or later). The slave can receive information from the robot every 8ms, as shown in the following table.
30005 port feedback robot information every 200ms. 30006 port  is a configurable port for robot  information feedback (default: 50ms feedback). Each packet received through the real-time feedback port has 1440 bytes, which are arranged in a standard format. The following table shows the order of the bytes.



|       Meaning        |      Type      | Number of values | Size in bytes | Byte position value |                            Notes                             | Supported products |
| :------------------: | :------------: | :--------------: | :-----------: | :-----------------: | :----------------------------------------------------------: | ------------------ |
|     MessageSize      | unsigned short |        1         |       2       |     0000 ~ 0001     |                Total message length in bytes                 | CR\MG\M1Pro        |
|                      | unsigned short |        3         |       6       |     0002 ~ 0007     |                           Reserved                           | CR\MG\M1Pro        |
|    DigitalInputs     |     uint64     |        1         |       8       |     0008 ~ 0015     |             Current state of the digital inputs.             | CR\MG\M1Pro        |
|    DigitalOutputs    |     uint64     |        1         |       8       |     0016 ~ 0023     |                        Digital output                        | CR\MG\M1Pro        |
|      RobotMode       |     uint64     |        1         |       8       |     0024 ~ 0031     |                          Robot mode                          | CR\MG\M1Pro        |
|      TimeStamp       |     uint64     |        1         |       8       |     0032 ~ 0039     |                       Time stamp (ms)                        | CR                 |
|                      |     uint64     |        1         |       8       |     0040 ~ 0047     |                           Reserved                           | CR\MG\M1Pro        |
|      TestValue       |     uint64     |        1         |       8       |     0048 ~ 0055     |                     test standard value                      | CR\MG\M1Pro        |
|                      |     double     |        1         |       8       |     0056 ~ 0063     |                           Reserved                           | CR\MG\M1Pro        |
|     SpeedScaling     |     double     |        1         |       8       |     0064 ~ 0071     |           Speed scaling of the trajectory limiter            | CR\MG              |
|  LinearMomentumNorm  |     double     |        1         |       8       |     0072 ~ 0079     | Norm of Cartesian linear momentum(specific hardware version ) | CR                 |
|        VMain         |     double     |        1         |       8       |     0080 ~ 0087     |                  Masterboard: Main voltage                   | CR                 |
|        VRobot        |     double     |        1         |       8       |     0088 ~ 0095     |               Masterboard: Robot voltage (48V)               | CR                 |
|        IRobot        |     double     |        1         |       8       |     0096 ~ 0103     |                  Masterboard: Robot current                  | CR                 |
|                      |     double     |        1         |       8       |     0104 ~ 0111     |                           Reserved                           | CR\MG\M1Pro        |
|                      |     double     |        1         |       8       |     0112 ~ 0119     |                           Reserved                           | CR\MG\M1Pro        |
|  ToolAcceleroMeter   |     double     |        3         |      24       |     0120 ~ 0143     | Tool x,y and z accelerometer values(specific hardware version) | CR                 |
|    ElbowPosition     |     double     |        3         |      24       |     0144 ~ 0167     |          Elbow position(specific hardware version)           | CR                 |
|    ElbowVelocity     |     double     |        3         |      24       |     0168 ~ 0191     |          Elbow velocity(specific hardware version)           | CR                 |
|       QTarget        |     double     |        6         |      48       |     0192 ~ 0239     |                    Target joint positions                    | CR\MG              |
|       QDTarget       |     double     |        6         |      48       |     0240 ~ 0287     |                   Target joint velocities                    | CR\MG              |
|      QDDTarget       |     double     |        6         |      48       |     0288 ~ 0335     |                  Target joint accelerations                  | CR\MG              |
|       ITarget        |     double     |        6         |      48       |     0336 ~ 0383     |                    Target joint currents                     | CR\MG              |
|       MTarget        |     double     |        6         |      48       |     0384 ~ 0431     |                Target joint moments (torques)                | CR\MG              |
|       QActual        |     double     |        6         |      48       |     0432 ~ 0479     |                    Actual joint positions                    | CR\MG              |
|       QDActual       |     double     |        6         |      48       |     0480 ~ 0527     |                   Actual joint velocities                    | CR\MG              |
|       IActual        |     double     |        6         |      48       |     0528 ~ 0575     |                    Actual joint currents                     | CR\MG              |
|    ActualTCPForce    |     double     |        6         |      48       |     0576 ~ 0623     |        CP sensor value (calculated by six-axis force)        | CR\MG              |
|   ToolVectorActual   |     double     |        6         |      48       |     0624 ~ 0671     | Actual Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation | CR\MG              |
|    TCPSpeedActual    |     double     |        6         |      48       |     0672 ~ 0719     |   Actual speed of the tool given in Cartesian coordinates    | CR\MG              |
|       TCPForce       |     double     |        6         |      48       |     0720 ~ 0767     |        TCP force value (calculated by joint current）        | CR\MG              |
|   ToolVectorTarget   |     double     |        6         |      48       |     0768 ~ 0815     | Target Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation | CR\MG              |
|    TCPSpeedTarget    |     double     |        6         |      48       |     0816 ~ 0863     |   Target speed of the tool given in Cartesian coordinates    | CR\MG              |
|  MotorTemperatures   |     double     |        6         |      48       |     0864 ~ 0911     |         Temperature of each joint in degrees celsius         | CR                 |
|      JointModes      |     double     |        6         |      48       |     0912 ~ 0959     |                     Joint control modes                      | CR                 |
|       VActual        |     double     |        6         |      48       |     960  ~ 1007     |                    Actual joint voltages                     | CR                 |
|       HandType       |      char      |        4         |       4       |     1008 ~ 1011     |                          Hand Type                           | CR\M1Pro           |
|         User         |      char      |        1         |       1       |        1012         |                       User coordinate                        | CR                 |
|         Tool         |      char      |        1         |       1       |        1013         |                       Tool coordinate                        | CR                 |
|     RunQueuedCmd     |      char      |        1         |       1       |        1014         |                      Queue running flag                      | CR                 |
|     PauseCmdFlag     |      char      |        1         |       1       |        1015         |                       Queue pause flag                       | CR                 |
|    VelocityRatio     |      char      |        1         |       1       |        1016         |                 Joint velocity ratio(0~100)                  | CR                 |
|  AccelerationRatio   |      char      |        1         |       1       |        1017         |               Joint acceleration ratio(0~100)                | CR                 |
|      JerkRatio       |      char      |        1         |       1       |        1018         |                   Joint jerk ratio(0~100)                    | CR                 |
|   XYZVelocityRatio   |      char      |        1         |       1       |        1019         |           Cartesian position velocity ratio(0~100)           | CR                 |
|    RVelocityRatio    |      char      |        1         |       1       |        1020         |             Cartesian pose velocity ratio(0~100)             | CR                 |
| XYZAccelerationRatio |      char      |        1         |       1       |        1021         |         Cartesian position acceleration ratio(0~100)         | CR                 |
|  RAccelerationRatio  |      char      |        1         |       1       |        1022         |         Cartesian attitude acceleration ratio(0~100)         | CR                 |
|     XYZJerkRatio     |      char      |        1         |       1       |        1023         |             Cartesian position jerk ratio(0~100)             | CR                 |
|      RJerkRatio      |      char      |        1         |       1       |        1024         |               Cartesian pose jerk ratio(0~100)               | CR                 |
|     BrakeStatus      |      char      |        1         |       1       |        1025         |                         Brake status                         | CR                 |
|     EnableStatus     |      char      |        1         |       1       |        1026         |                        Enable status                         | CR                 |
|      DragStatus      |      char      |        1         |       1       |        1027         |                         Drag status                          | CR                 |
|    RunningStatus     |      char      |        1         |       1       |        1028         |                        Running status                        | CR                 |
|     ErrorStatus      |      char      |        1         |       1       |        1029         |                         Alarm status                         | CR                 |
|     JogStatusCR      |      char      |        1         |       1       |        1030         |                        Jogging status                        | CR                 |
|     CRRobotType      |      char      |        1         |       1       |        1031         |                          Robot type                          | CR                 |
|   DragButtonSignal   |      char      |        1         |       1       |        1032         |                         Drag signal                          | CR                 |
|  EnableButtonSignal  |      char      |        1         |       1       |        1033         |                        Enable signal                         | CR                 |
|  RecordButtonSignal  |      char      |        1         |       1       |        1034         |                      Record the signal                       | CR                 |
| ReappearButtonSignal |      char      |        1         |       1       |        1035         |                      Repetition signal                       | CR                 |
|   JawButtonSignal    |      char      |        1         |       1       |        1036         |                     Grip control signal                      | CR                 |
|    SixForceOnline    |      char      |        1         |       1       |        1037         |                Six - axis force online status                | CR                 |
|     Reserve2[82]     |      char      |        1         |      82       |      1038-1119      |                           Reserved                           | CR\MG\M1Pro        |
|      MActual[6]      |     double     |        6         |      48       |     1120 ~ 1167     |                        Actual torque                         | CR                 |
|         Load         |     double     |        1         |       8       |      1168-1175      |                        Payload weight                        | CR\MG\M1Pro        |
|       CenterX        |     double     |        1         |       8       |      1176-1183      |              Eccentric distance in X direction               | CR\MG\M1Pro        |
|       CenterY        |     double     |        1         |       8       |      1184-1191      |              Eccentric distance in Y direction               | CR\MG\M1Pro        |
|       CenterZ        |     double     |        1         |       8       |      1192-1199      |              Eccentric distance in Z direction               | CR\MG\M1Pro        |
|       User[6]        |     double     |        6         |      48       |      1200-1247      |                       User coordinate                        | CR                 |
|       Tool[6]        |     double     |        6         |      48       |      1248-1295      |                       Tool coordinate                        | CR                 |
|      TraceIndex      |     double     |        1         |       8       |      1296-1303      |                 Track playback running index                 | CR                 |
|   SixForceValue[6]   |     double     |        6         |      48       |      1304-1351      |               Six - axis force original value                | CR                 |
| TargetQuaternion[4]  |     double     |        4         |      32       |      1352-1383      |               Target quaternion [qw,qx,qy,qz]                | CR                 |
| ActualQuaternion[4]  |     double     |        4         |      32       |      1384-1415      |                Actual quaternion[qw,qx,qy,qz]                | CR                 |
|     Reserve3[24]     |      char      |        1         |      24       |     1416 ~ 1440     |                           Reserved                           | CR\MG\M1Pro        |
|        TOTAL         |                |                  |     1440      |                     |                       1440byte package                       |                    |

Robot Mode returns the mode of robo as follows:                          

| Mode | Description           | Note                             |
| ---- | --------------------- | -------------------------------- |
| 1    | ROBOT_MODE_INIT       | Initialization                   |
| 2    | ROBOT_MODE_BRAKE_OPEN | Brake release                    |
| 3    |                       | Reserved                         |
| 4    | ROBOT_MODE_DISABLED   | Disabled (brake is not released) |
| 5    | ROBOT_MODE_ENABLE     | Enable (idle)                    |
| 6    | ROBOT_MODE_BACKDRIVE  | Drag                             |
| 7    | ROBOT_MODE_RUNNING    | Run                              |
| 8    | ROBOT_MODE_RECORDING  | Drag record                      |
| 9    | ROBOT_MODE_ERROR      | Alarm                            |
| 10   | ROBOT_MODE_PAUSE      | Pause state                      |
| 11   | ROBOT_MODE_JOG        | Jogging                          |

- Note：

  - [ ] If the brake is released, the mode is 2.
  - [ ] If the robot is powered on but not enabled, the mode is 4.
  - [ ] If the robot is enabled successfully, the mode is 5.
  - [ ] If the robot runs, the mode is 7.
  - [ ] If the robot pauses, the mode is 10.
  - [ ] If the robot enters drag mode (enabled state), the mode is 6.
  - [ ] If the robot is dragging and recording, the mode is 8.
  - [ ] If the robot is jogging, the mode is 11.
  - [ ] Alarm is the top priority. When other modes exist simultaneously, if there is an alarm, the mode is set to 9 first.

- BrakeStatus：

  0x01: indicates that the sixth axle brake is released.

  0x02: indicates that the fifth axle brake is released.

  0x03: indicates that the fifth and sixth axles brake is released.

  0x04: indicates that the forth axle brake is released.

  ...

  The following bits indicate the locking state:

  | 7        | 6        | 5       | 4       | 3       | 2       | 1       | 0       |
  | -------- | -------- | ------- | ------- | ------- | ------- | ------- | ------- |
  | Reserved | Reserved | Joint 1 | Joint 2 | Joint 3 | Joint 4 | Joint 5 | Joint 6 |

- JointModes：

  ​	8: position mode.

  ​	10: torque mode.

- HandType contains four char parameters, which are LorR, UorD, ForN, and Config6 for the CR series.
  The M1Pro product uses only the first parameter LorR, 0 means left-handed system, and 1 means right-handed system. Refer to the SetArmOrientation in section 3.23 for details.

- RobotType：

  | RobotType | Product |
  | --------- | ------- |
  | 3         | CR3     |
  | 31        | CR3L    |
  | 5         | CR5     |
  | 7         | CR7     |
  | 10        | CR10    |
  | 12        | CR12    |
  | 16        | CR16    |
  | 1         | MG400   |
  | 2         | M1Pro   |

  

# 5. Communication Protocol—motion port

The following table shows the motion command supported by the 30003 port. 

MG400/M1Pro is a four-axis product and CR series is a six-axis product. The parameters in the motion command are agreed according to the six coordinate values of CR series robot;
When users use MG400/M1Pro, you need to fill in four coordinate.



| Parameter    | Product     | Description                                                  |
| ------------ | ----------- | ------------------------------------------------------------ |
| MovJ         | CR\MG\M1Pro | point to point movement, the target point is Cartesian point |
| MovL         | CR\MG\M1Pro | linear movement, the target point is Cartesian point         |
| JointMovJ    | CR\MG\M1Pro | point to point movement, the target point is joint point     |
| MovLIO       | CR\MG\M1Pro | set the status of digital output port in straight line movement (can set several groups) |
| MovJIO       | CR\MG\M1Pro | set the status of digital output port in point-to-point movement, and the target point is Cartesian point |
| Arc          | CR\MG\M1Pro | arc movement, needs to combine with other motion commands    |
| ServoJ       | CR          | dynamic following command based on joint space               |
| ServoP       | CR          | dynamic following command based on Cartesian space           |
| MoveJog      | CR\MG\M1Pro | Jogging                                                      |
| StartTrace   | CR          | Trajectory fitting                                           |
| StartPath    | CR          | Trajectory playback                                          |
| StartFCTrace | CR          | Trajectory fitting with force control                        |
| Sync         | CR\MG\M1Pro | Blocking program execution                                   |
| RelMovJTool  | CR          | Relative motion is performed along the tool coordinate system, and the end motion is joint motion |
| RelMovLTool  | CR          | Relative motion is performed along the tool coordinate system, and the end motion is linear motion |
| RelMovJUser  | CR\MG\M1Pro | Relative motion is performed along the user coordinate system, and the end motion mode is the joint motion |
| RelMovLUser  | CR\MG\M1Pro | Relative motion performed along the user coordinate system, and the end motion mode is a linear motion |
| RelJointMovJ | CR\MG\M1Pro | Relative motion instruction is conducted along the joint coordinate system of each axis, and the end motion mode is joint motion |



## 5.1 MovJ

- Function: MovJ(X,Y,Z,Rx,Ry,Rz,User=index,Tool=index,SpeedJ=R,AccJ=R)

- Description: point to point movement, the target point is Cartesian point

- Parameter

  | Parameter | Type   | Description                  |
  | --------- | ------ | ---------------------------- |
  | X         | double | X-axis coordinates, unit: mm |
  | Y         | double | Y-axis coordinates, unit: mm |
  | Z         | double | Z-axis coordinates, unit: mm |
  | Rx        | double | Rx-axis coordinates, unit: ° |
  | Ry        | double | Ry-axis coordinates, unit: ° |
  | Rz        | double | Rz-axis coordinates, unit: ° |

  User, Tool, SpeedJ, AccJ are optional setting parameters, indicate setting user coordinate system, tool coordinate system, joint velocity ratio and acceleration ratio values respectively.
  The value has the same meaning as SpeedJ and AccJ setting by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003
  
- Example
  
  ```
  MovJ(-500,100,200,150,0,90,AccJ=50)
  ```



## 5.2 MovL

- Function: MovL(X,Y,Z,Rx,Ry,Rz,User=index,Tool=index,SpeedL=R,AccL=R) 

- Description: linear movement, the target point is Cartesian point

- Parameter

  | Parameter | Type   | Description                  |
  | --------- | ------ | ---------------------------- |
  | X         | double | X-axis coordinates, unit: mm |
  | Y         | double | Y-axis coordinates, unit: mm |
  | Z         | double | Z-axis coordinates, unit: mm |
  | Rx        | double | Rx-axis coordinates, unit: ° |
  | Ry        | double | Ry-axis coordinates, unit: ° |
  | Rz        | double | Rz-axis coordinates, unit: ° |

  User, Tool, SpeedJ, AccJ are optional setting parameters, indicate setting user coordinate system, tool coordinate system, joint velocity ratio and acceleration ratio values respectively.
  The value has the same meaning as SpeedJ and AccJ setting by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003
  
- Example
  
  ```
  MovL(-500,100,200,150,0,90,SpeedL=60)
  ```



## 5.3 JointMovJ

- Function: JointMovJ(J1,J2,J3,J4,J5,J6)

- Description: point to point movement, the target point is joint point

- Parameter

  | Parameter | Type   | Description             |
  | --------- | ------ | ----------------------- |
  | J1        | double | J1 coordinates, unit: ° |
  | J2        | double | J2 coordinates, unit: ° |
  | J3        | double | J3 coordinates, unit: ° |
  | J4        | double | J4 coordinates, unit: ° |
  | J5        | double | J5 coordinates, unit: ° |
  | J6        | double | J6 coordinates, unit: ° |

  SpeedJ and AccJ are optional parameters, indicating setting joint velocity ratio and acceleration ratio respectively.
  The value has the same meaning as SpeedJ and AccJ setting by port 29999.

- Supporting port: 30003
  
- Example
  
  ```
  JointMovJ(0,0,-90,0,90,0)
  ```



## 5.4 MovLIO

- Function: MovLIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},User=index,Tool=index,SpeedL=R,AccL=R) 

- Description: set the status of digital output port in straight line movement, and the target point is Cartesian point

- Parameter

  | Parameter | Type   | Description                                                  |
  | --------- | ------ | ------------------------------------------------------------ |
  | X         | double | X-axis coordinates, unit: mm                                 |
  | Y         | double | Y-axis coordinates, unit: mm                                 |
  | Z         | double | Z-axis coordinates, unit: mm                                 |
  | Rx        | double | Rx-axis coordinates, unit: °                                 |
  | Ry        | double | Ry-axis coordinates, unit: °                                 |
  | Rz        | double | Rz-axis coordinates, unit: °                                 |
  | Mode      | int    | mode of Distance. 0: distance percentage; 1: distance away from the starting point or target point |
  | Distance  | int    | move specified distance. If Mode is 0, Distance refers to the distance percentage between the starting point and target point; range: 0~100. If Distance value is positive, it refers to the distance away from the starting point; If Distance value is negative, it refers to the distance away from the target point |
  | Index     | int    | digital output index, range: 1~24                            |
  | Status    | int    | digital output status, range: 0 or 1                         |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example

  ```
  MovLIO(-500,100,200,150,0,90,{0,50,1,0})
  ```



## 5.5 MovJIO

- Function: MovJIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},User=index,Tool=index,SpeedJ=R,AccJ=R)  

- Description: set the status of digital output port in point-to-point movement, and the target point is Cartesian point

- Parameters: 

  | Parameter | Type   | Description                                                  |
  | --------- | ------ | ------------------------------------------------------------ |
  | X         | double | X-axis coordinates, unit: mm                                 |
  | Y         | double | Y-axis coordinates, unit: mm                                 |
  | Z         | double | Z-axis coordinates, unit: mm                                 |
  | Rx        | double | Rx-axis coordinates, unit: °                                 |
  | Ry        | double | Ry-axis coordinates, unit: °                                 |
  | Rz        | double | Rz-axis coordinates, unit: °                                 |
  | Mode      | int    | mode of Distance. 0: distance percentage; 1: distance away from the starting point or target point |
  | Distance  | int    | move specified distance. If Mode is 0, Distance refers to the distance percentage between the starting point and target point; range: 0~100. If Distance value is positive, it refers to the distance away from the starting point; If Distance value is negative, it refers to the distance away from the target point |
  | Index     | int    | digital output index, range: 1~24                            |
  | Status    | int    | digital output status, range: 0 or 1                         |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example

  ```
  MovJIO(-500,100,200,150,0,90,{0,50,1,0})
  ```



## 5.6 Arc

- Function: Arc(X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2,User=index,Tool=index,SpeedL=R,AccL=R)

- Description: move from the current position to a target position in an arc interpolated mode under the Cartesian coordinate system
  This command needs to combine with other motion commands to obtain the starting point of an arc trajectory

- Parameter

  | Parameter | Type   | Description                                       |
  | --------- | ------ | ------------------------------------------------- |
  | X1        | double | X1-axis coordinates of arc center point, unit: mm |
  | Y1        | double | Y1-axis coordinates of arc center point, unit: mm |
  | Z1        | double | Z1-axis coordinates of arc center point, unit: mm |
  | Rx1       | double | Rx1-axis coordinates of arc center point, unit: ° |
  | Ry1       | double | Ry1-axis coordinates of arc center point, unit: ° |
  | Rz1       | double | Rz1-axis coordinates of arc center point, unit: ° |
  | X2        | double | X2-axis coordinates of arc ending point, unit: mm |
  | Y2        | double | Y2-axis coordinates of arc ending point, unit: mm |
  | Z2        | double | Z2-axis coordinates of arc ending point, unit: mm |
  | Rx2       | double | Rx2-axis coordinates of arc ending point, unit: ° |
  | Ry2       | double | Ry2-axis coordinates of arc ending point, unit: ° |
  | Rz2       | double | Rz2-axis coordinates of arc ending point, unit: ° |

- Supporting port: 30003

- Example

  ```
  MovL(-300,-150,200,150,0,90,SpeedL=100,AccL=100)
  Arc(-350,-200,200,150,0,90,-300,-250,200,150,0,90)
  ```

  

## 5.7 ServoJ

- Function: ServoJ(J11,J12,J13,J14,J15,J16)

- Description: dynamic following command based on joint space. You are advised to set the frequency of customer secondary development to 33Hz (30ms), that is, set the cycle interval to at least 30ms.

- Parameters: 

  | Parameter | Type   | Description                    |
  | --------- | ------ | ------------------------------ |
  | J11       | double | J11 coordinates of P1, unit: ° |
  | J12       | double | J12 coordinates of P1, unit: ° |
  | J13       | double | J13 coordinates of P1, unit: ° |
  | J14       | double | J14 coordinates of P1, unit: ° |
  | J15       | double | J15 coordinates of P1, unit: ° |
  | J16       | double | J16 coordinates of P1, unit: ° |

- Supporting port: 30003

- Example

  ```
  ServoJ(0,0,-90,0,90,0)
  ```



## 5.8 ServoP

- Function: ServoP(X1,Y1,Z1,A1,B1,C1)

- Description: dynamic following command based on Cartesian space. You are advised to set the frequency of customer secondary development to 33Hz (30ms), that is, set the cycle interval to at least 30ms.

- Parameter

  | Parameter | Type   | Description                   |
  | --------- | ------ | ----------------------------- |
  | X1        | double | X1-axis coordinates, unit: mm |
  | Y1        | double | Y1-axis coordinates, unit: mm |
  | Z1        | dou    | Z1-axis coordinates, unit: mm |
  | A1        | double | A1-axis coordinates, unit: °  |
  | B1        | double | B1-axis coordinates, unit: °  |
  | C1        | double | C1-axis coordinates, unit: °  |

- Supporting port: 30003

- Example

  ```
  ServoP(-500,100,200,150,0,90）
  ```



## 5.9 MoveJog

- Function: MoveJog(axisID,CoordType=typeValue,User=index,Tool=index)    

- Description: Jogging movement. The movement is not fixed distance. CR controller 3.5.2 and MG400/M1Pro controller 1.5.6 and later support this command.

- Parameters：

  | Parameter | Type   | Description                                                  |
  | --------- | ------ | ------------------------------------------------------------ |
  | axisID    | string | J1+ means joint 1 is moving in the positive direction and J1- means joint 1 is moving in the negative direction <br />J2+ means joint 2is moving in the positive direction and J2- means joint 2 is moving in the negative direction<br />J3+ means joint 3 is moving in the positive direction and J3- means joint 3 is moving in the negative direction<br />J4+ means joint 4 is moving in the positive direction and J4- means joint 4 is moving in the negative direction<br />J5+ means joint 5 is moving in the positive direction and J5- means joint 5 is moving in the negative direction<br />J6+ means joint 6 is moving in the positive direction and J6- means joint 6 is moving in the negative direction<br />X+ means joint X is moving in the positive direction and X- means joint X is moving in the negative direction <br />Y+ means joint Y is moving in the positive direction and Y- means joint Y is moving in the negative direction<br />Z+ means joint Z is moving in the positive direction andZ- means joint Z is moving in the negative direction<br />Rx+ means joint Rx is moving in the positive direction and Rx- means joint Rx is moving in the negative direction<br />Ry+ means joint Ry is moving in the positive direction and Ry- means joint Ry is moving in the negative direction<br />Rz+ means joint Rz is moving in the positive direction and Rz- means joint Rz is moving in the negative direction<br /> |

  CoordType, User, and Tool are optional parameters and retain the default values.
  CoordType: 0: user coordinate system, 1: joint coordinate system, 2: tool coordinate system. The default value is 1.
  User: User index 0 to 9. The default value is 0.
  Tool: Tool index 0 to 9. The default value is 0.
  The optional parameters CoordType, User, and Tool are ignored if the User sends the node to run again.

- Supporting port: 30003

- Example

  Joint 2 is moving in the negative direction，and then it stops.

  ```
  MoveJog(j2-)
  ```

  

## 5.10 StartTrace

- Function: StartTrace(traceName)

- Description: trajectory fitting, the trajectory file is a Cartesian point. CR controller versions 3.5.2 and later support this command.

  Users can query the running status of robots by obtaining RobotMode.
  ROBOT_MODE_RUNNING indicates that the robot is in the trajectory fitting operation, ROBOT_MODE_IDLE indicates that the trajectory fitting operation is completed, and ROBOT_MODE_ERROR indicates that the robot is alarming.

- Parameters：

  | Parameter | Type   | Description                        |
  | --------- | ------ | ---------------------------------- |
  | traceName | string | Track file name (including suffix) |

- Supporting port: 30003

- Example

  Get the first node {x,y,z,rx,ry,rz} of the trajectory file of recv_string. After the point-to-point movement reaches {x,y,z,rx,ry,rz}, the file recv_string is delivered for trajectory fitting.

  ```
  GetTraceStartPose(recv_string.json)
  MovJ(x,y,z,rx,ry,rz)
  StartTrace(recv_string)
  ```

  

## 5.11 StartPath

- Function: StartPath(traceName,const,cart)

- Description: trajectory playback, the trajectory file is a Joint point. CR controller versions 3.5.2 and later support this command.

  Users can query the running status of robots by obtaining RobotMode.
  ROBOT_MODE_RUNNING indicates that the robot is in the track playback, ROBOT_MODE_IDLE indicates that the track playback is completed, and ROBOT_MODE_ERROR indicates alarm.

- Parameters：

  | Parameter | Type   | Description                                                  |
  | --------- | ------ | ------------------------------------------------------------ |
  | traceName | string | Track file name (including suffix)                           |
  | const     | int    | Const =1: Constant reappearance, pauses and dead zones are removed from the track<br/>Const =0: restores at the original speed |
  | cart      | int    | Cart =1:  Cartesian path<br/>Cart =0:  joint path            |

- Supporting port: 30003

- Example

  Get the first point {j1,j2,j3,j4,j5,j6} of the recv_string path.
  After the point-to-point movement reaches {j1,j2,j3,j4,j5,j6}, the file recv_string is delivered for trajectory playback at the original speed, and at a uniform speed in the Cartesian path.

  ```
  GetPathStartPose(recv_string)
  JointMovJ(j1,j2,j3,j4,j5,j6)
  StartPath(recv_string,0,1)
  ```

  

## 5.12 StartFCTrace

- Function: StartFCTrace(traceName)

- Description: Trajectory fitting with force control, trajectory file cartesian point. CR controller versions 3.5.2 and later support this command.

  Users can query the running status of robots by obtaining RobotMode.
  ROBOT_MODE_RUNNING indicates that the robot is in the track playback, ROBOT_MODE_IDLE indicates that the track playback is completed, and ROBOT_MODE_ERROR indicates alarm.

- Parameters：

  | Parameter | Type   | Description                        |
  | --------- | ------ | ---------------------------------- |
  | traceName | string | Track file name (including suffix) |

- Supporting port: 30003

- Example:

  Get the first point {j1,j2,j3,j4,j5,j6} of the recv_string path.
  After the point-to-point movement reaches {j1,j2,j3,j4,j5,j6}, the file recv_string is delivered for trajectory playback at the original speed, and at a uniform speed in the Cartesian path.

  ```
  GetTraceStartPose(recv_string)
  MovJ(x,y,z,rx,ry,rz)
  StartFCTrace(recv_string)
  Sync()  
  ```

  Due to trajectory fitting with force control, the end point is uncertain. Sync must be added to wait for StartFCTrace to finish execution before other motion instructions can be continued.

  


## 5.13 Sync

- Function: Sync()

- Description: The blocking program executes and does not return until all queue instructions have been executed.

- Parameters：None

- Supporting port: 30003

- Example:

  ```
  Sync()
  ```

  


## 5.14 RelMovJTool

- Function: RelMovJTool(offsetX, offsetY,offsetZ, offsetRx,offsetRy,offsetRz, Tool,SpeedJ=R, AccJ=R,User=Index)  

- Description: Relative motion is performed along the tool coordinate system, and the end motion is joint motion. CR controller versions 3.5.2 and later support this command.

- Parameters：

  | Parameter | Type   | Description                                            |
  | --------- | ------ | ------------------------------------------------------ |
  | OffsetX   | double | X-axis coordinates, unit: mm                           |
  | OffsetY   | double | Y-axis coordinates, unit: mm                           |
  | OffsetZ   | double | Z-axis coordinates, unit: mm                           |
  | OffsetRx  | double | Rx -axis coordinates, unit: °                          |
  | OffsetRy  | double | Ry-axis coordinates, unit: °                           |
  | OffsetRz  | double | Rz-axis coordinates, unit: °                           |
  | Tool      | int    | Calibrated tool coordinate system, value range: 0 to 9 |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example:

  ```
  RelMovJTool(10,10,10,0,0,0,0)
  ```

  

## 5.15 RelMovLTool

- Function: RelMovLTool(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, Tool,SpeedL=R, AccL=R,User=Index)  

- Description: Relative motion is performed along the tool coordinate system, and the end motion is linear motion. CR controller versions 3.5.2 and later support this command.

- Parameters：

  | Parameter | Type   | Description                                            |
  | --------- | ------ | ------------------------------------------------------ |
  | OffsetX   | double | X-axis coordinates, unit: mm                           |
  | OffsetY   | double | Y-axis coordinates, unit: mm                           |
  | OffsetZ   | double | Z-axis coordinates, unit: mm                           |
  | OffsetRx  | double | Rx -axis coordinates, unit: °                          |
  | OffsetRy  | double | Ry-axis coordinates, unit: °                           |
  | OffsetRz  | double | Rz-axis coordinates, unit: °                           |
  | Tool      | int    | Calibrated tool coordinate system, value range: 0 to 9 |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example

  ```
  RelMovLTool(10,10,10,0,0,0,0)
  ```

  

## 5.16 RelMovJUser

- Function: RelMovJUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, User,SpeedJ=R, AccJ=R,Tool=Index)

- Description: Relative motion is performed along the user coordinate system, and the end motion mode is the joint motion. CR controller 3.5.2 and MG400/M1Pro controller 1.5.6 and later support this command.

- Parameters：

   | Parameter | Type   | Description                                            |
  | --------- | ------ | ------------------------------------------------------ |
  | OffsetX   | double | X-axis coordinates, unit: mm                           |
  | OffsetY   | double | Y-axis coordinates, unit: mm                           |
  | OffsetZ   | double | Z-axis coordinates, unit: mm                           |
  | OffsetRx  | double | Rx -axis coordinates, unit: °                          |
  | OffsetRy  | double | Ry-axis coordinates, unit: °                           |
  | OffsetRz  | double | Rz-axis coordinates, unit: °                           |
  | User      | int    | Calibrated user coordinate system, value range: 0 to 9 |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example:

  ```
  RelMovJUser(10,10,10,0,0,0,0)
  ```

  

## 5.17 RelMovLUser

- Function: RelMovLUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, User,SpeedL=R, AccL=R,Tool=Index)  

- Description: Relative motion performed along the user coordinate system, and the end motion mode is a linear motion. CR controller 3.5.2 and MG400/M1Pro controller 1.5.6 and later support this command.

- Parameters：

  | Parameter | Type   | Description                                            |
  | --------- | ------ | ------------------------------------------------------ |
  | OffsetX   | double | X-axis offset, unit: mm                                |
  | OffsetY   | double | Y-axis offset, unit: mm                                |
  | OffsetZ   | double | Z-axis offset, unit: mm                                |
  | OffsetRx  | double | Rx-axis offset, unit: °                                |
  | OffsetRy  | double | Ry-axis offset, unit: °                                |
  | OffsetRz  | double | Rz-axis offset, unit: °                                |
  | User      | int    | Calibrated user coordinate system, value range: 0 to 9 |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.
  User: indicates the User index 0 to 9. The default value is the last value.
  Tool: Tool index 0 to 9. The default value is the last value.

- Supporting port: 30003

- Example:

  ```
  RelMovLUser(10,10,10,0,0,0,0)
  ```

  

## 5.18 RelJointMovJ

- Description: Relative motion instruction is conducted along the joint coordinate system of each axis, and the end motion mode is joint motion. CR controller 3.5.2 and MG400/M1Pro controller 1.5.6 and later support this command.

- Function: RelJointMovJ(Offset1,Offset2,Offset3,Offset4,Offset5,Offset6,SpeedJ=R, AccJ=R)  

- Parameters：

  | Parameter | Type   | Description             |
  | --------- | ------ | ----------------------- |
  | Offset1   | double | J1-axis offset, unit: ° |
  | Offset2   | double | J2-axis offset, unit: ° |
  | Offset3   | double | J3-axis offset, unit: ° |
  | Offset4   | double | J4-axis offset, unit: ° |
  | Offset5   | double | J5-axis offset, unit: ° |
  | Offset6   | double | J6-axis offset, unit: ° |

  SpeedL and AccL are optional parameters, indicating setting user coordinate system, tool coordinate system, Cartesian speed ratio and acceleration ratio values respectively.
  The value is the same as the value of SpeedL and AccL set by port 29999.

- Supporting port: 30003

- Example:

  ```
  RelJointMovJ(10,10,10,0,0,0)
  ```




# 6.Error code Description

| Error code | Description                                           | Note                                                         |
| ---------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| 0          | No error                                              | operate successfully                                         |
| -1         | fail to get                                           | Failed to receive or establish                               |
| ...        | ...                                                   | ...                                                          |
| -10000     | Command error                                         | The command does not exist                                   |
| -20000     | Parameter number error                                | The number of parameters in the command is incorrect         |
| -30001     | The first parameter has an incorrect parameter type   | 30000 indicates that the parameter type is incorrect.<br/>The last bit 1 indicates that the parameter type of the first parameter is wrong |
| -30002     | The second parameter has an incorrect parameter type  | 30000 indicates that the parameter type is incorrect.<br/>The last bit 2 indicates that the parameter type of the second parameter is wrong |
| ...        | ...                                                   | ...                                                          |
| -40001     | The first parameter has an incorrect parameter range  | -40000 indicates that the parameter range is incorrect.<br/>The last bit of 1 indicates that the range of the first parameter is wrong |
| -40002     | The second parameter has an incorrect parameter range | -40000 indicates that the parameter range is incorrect.<br/>The last bit of 2 indicates that the range of the second parameter is wrong |
| ...        | ...                                                   | ...                                                          |










