![](README.assets/pic1.jpg)

![](README.assets/pic2.jpg)

# 0.Changelog

- V1.0-2021/05/27：建立文档
- V1.1-2021/07/05：修正一些运动指令的参数错误
- V1.2-2021/07/08：主要新增RobotMode机器人模式以及文档书写错误
- V1.3-2021/07/13：更新返回数据表格
- V1.4-2021/07/16: Arc3 改为Arc : 与新版MG400指令一致；ServoJ的参数不能以表的形式，因为一次只能下发一个点，ServoP相同；文档中是1440字节，不是1044。
- V1.5-2021/07/23: 数据的结构体去除掉添加一个是字节位置值；对RobotMode的状态优先级进行描述；新增机器人上电接口。
- V1.6-2021/08/04:调整Dashboard端口和实时反馈端口表格位置；根据GK项目，新增运行脚本、读写保存寄存器、获取机器人状态等指令；
- V1.7-2021/08/23:添加Sync指令、SetArmOrientation描述优化、电子皮肤SetObstacleAvoid、SetSafeSkin相关指令、轨迹复现相关指令GetTraceStartPose、GetPathStartPose、StartTrace、StartPath以及正解逆解接口InverseSolution、PositiveSolution；以及若干书写错误；
- V1.8-2021/08/31:新增碰撞等级SetCollisionLevel、轨迹文件预处理接口HandleTrajPoints、获取六维力数据GetSixForceData、获取笛卡尔坐标系下机械臂的实时位姿接口GetPose、获取关节坐标系下机械臂的实时位姿接口GetAngle、急停指令EmergencyStop、带力控的轨迹拟合StartFCTrace、关节/笛卡尔点动指令MoveJog；
- V1.8-2021/09/15:定义轨迹预处理运行结果接口；若用户下发不带参数的该指令，代表查询当前指令的结果（适用于轨迹文件预处理）；轨迹复现/拟合/带力控拟合通过RobotMode获取机器人运行状态；
- V1.8-2021/09/27:更改Sync指令的端口到29999(阻塞指令),待所有队列指令执行完才返回done；以及去掉多余指令；



# 1. 综述

​	CR机器人现支持两种远程控制方式：**远程I/O模式、远程Modbus模式**；具体控制方式详见《Dobot-CR-Series-Robot-APP-User-Guide-V3.7》文档中软件使用说明->设置->远程控制章节中；

​	以上两种方式主要针对**远程运行脚本的控制**；由于基于TCP/IP的通讯具有成本低、可靠性高、实用性强、性能高等特点；许多工业自动化项目对支持TCP/IP协议控制机器人需求广泛，因此CR/MG400/M1Pro机器人将设计在TCP/IP协议的基础上，提供了丰富的接口用于与外部设备的交互；

​	关于TCP/IP协议的支持，CR系列机器人的控制器版本需V3.5.1.9及以上，MG400/M1Pro机器人的控制器版本需V1.5.4.0及以上。

# 2.消息格式

​	根据设计，CR机器人可以开启29999以及30003服务器端口；29999服务器端口(以下简称Dashboard端口)通过一发一收的方式负责接收一些简单的指令，即**Dashboard端口接收到客户端约定消息格式后会将结果反馈客户端**；30003服务器端口(以下简称实时反馈端口)**每8ms反馈机器人的信息，仅接收客户端的约定消息格式，不做反馈**；

## 2.1 下发格式

​	`消息名称(Param1,Param2,Param3……Paramn)`
​	消息格式如上所示，由一个消息名称，括号内由参数组成， 每一个参数之间以英文逗号  ”,”  相隔，一个完整的消息以右括号结束。

​	消息命令与消息应答都是 ASCII 码格式(字符串形式)。

## 2.2 返回格式

### 2.2.1接收成功返回：

​	`"消息名称(Param1,Param2,Param3……Paramn)"`

​	消息格式如上所示，成功返回下发的消息名称。

### 2.2.2 失败返回：

​	`"could not understand:'消息名称(Param1,Param2,Param3……Paramn)'"`

​	消息格式如上所示，失败返回could not understand:'消息名称(Param1,Param2,Param3……Paramn)'。



# 3.通信协议---Dashboard端口

​	上位机可以通过29999端口直接发送一些简单的指令给机器人，这些指令CR自己定义的，这些功能被称为Dashboard。如表是Dashboard的指令列表。可以通过Dashboard的指令实现对机器人使能/下使能、复位等控制；

| 指令                | 描述                                       |
| ----------------- | ---------------------------------------- |
| EnableRobot       | 使能机器人                                    |
| DisableRobot      | 去使能机器人                                   |
| ClearError        | 复位，用于清除错误                                |
| ResetRobot        | 机器人停止当前动作，重新接收使能，规划停                     |
| SpeedFactor       | 设置全局速率比                                  |
| User              | 选择已标定的用户坐标系（笛卡尔空间显示值 实际生效根据点）            |
| Tool              | 选择已标定的刀具坐标系                              |
| RobotMode         | 机器人模式                                    |
| PayLoad           | 设置负载                                     |
| DO                | 设置数字量输出端口状态                              |
| DOExecute         | 设置数字量输出端口状态（立即指令）                        |
| ToolDO            | 设置末端数字量输出端口状态                            |
| ToolDOExecute     | 设置末端数字量输出端口状态（立即指令）                      |
| AO                | 设置模拟量输出端口状态                              |
| AOExecute         | 设置模拟量输出端口状态（立即指令）                        |
| AccJ              | 设置关节加速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效 |
| AccL              | 设置笛卡尔加速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 |
| SpeedJ            | 设置关节速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效。 |
| SpeedL            | 设置笛卡尔速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 |
| Arch              | 设置Jump门型参数索引（起始点抬升高度、最大抬 升高度、结束点下降高度）    |
| CP                | 运动时设置平滑过渡                                |
| LimZ              | 设置Jump模式最大抬升高度                           |
| SetArmOrientation | 设置手系                                     |
| PowerOn           | 机器人上电                                    |
| RunScript         | 运行脚本                                     |
| StopScript        | 停止脚本                                     |
| PauseScript       | 暂停脚本                                     |
| ContinueScript    | 继续脚本                                     |
| GetHoldRegs       | 读保存寄存器                                   |
| SetHoldRegs       | 写保存寄存器                                   |
| SetSafeSkin       | 设置安全皮肤开关状态                               |
| SetObstacleAvoid  | 设置安全皮肤避障模式开关状态                           |
| GetTraceStartPose | 获取轨迹拟合中首个点位                              |
| GetPathStartPose  | 获取轨迹复现中首个点位                              |
| PositiveSolution  | 正解                                       |
| InverseSolution   | 逆解                                       |
| SetCollisionLevel | 设置碰撞等级                                   |
| HandleTrajPoints  | 轨迹文件预处理                                  |
| GetSixForceData   | 获取六维力数据                                  |
| GetAngle          | 获取关节坐标系下机械臂的实时位姿                         |
| GetPose           | 获取笛卡尔坐标系下机械臂的实时位姿                        |
| EmergencyStop     | 急停                                       |
| Sync              | 阻塞程序执行队列指令，待所有队列指令执行完才返回                 |



## 3.1 EnableRobot

- 功能：使能机器人

- 格式：EnableRobot()

- 参数数量：0

- 支持端口：29999



- 示例：

  EnableRobot()


## 3.2 DisableRobot

- 功能：下使能机器人

- 格式：DisableRobot()

- 参数数量：0

- 支持端口：29999

- 示例：

  DisableRobot()



## 3.3 ClearError

- 功能：清错机器人

- 格式：ClearError()

- 参数数量：0

- 支持端口：29999



- 参数详解：

- 示例：

  ClearError()

## 3.4 ResetRobot

- 功能：机器人停止

- 格式：ResetRobot()

- 参数数量：0

- 支持端口：29999



- 参数详解：

- 示例：

  ResetRobot()

## 3.5 SpeedFactor

- 功能：设置全局速度比例。 

- 格式：SpeedFactor(ratio)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                          |
  | ----- | ---- | --------------------------- |
  | ratio | int  | 运动速度比例，取值范围：0~100，但不包括0和100 |

- 示例：

  SpeedFactor(80)


## 3.6 User

- 功能：选择已标定的用户坐标系。 

- 格式：User(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                   |
  | ----- | ---- | -------------------- |
  | index | int  | 选择已标定的用户坐标系，取值范围：0~9 |

- 示例：

  User(1)

## 3.7 Tool

- 功能：选择已标定的刀具坐标系。 

- 格式：Tool(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                   |
  | ----- | ---- | -------------------- |
  | index | int  | 选择已标定的工具坐标系，取值范围：0~9 |

- 示例：

  Tool(1)

## 3.8 **RobotMode**

- 功能：机器人状态。 

- 格式：RobotMode()

- 参数数量：0

- 支持端口：29999

- 示例：

  RobotMode()

- 返回值：                             

  | 模式   | 描述                           | 备注     |
  | ---- | ---------------------------- | ------ |
  | -1   | ROBOT_MODE_NO_CONTROLLER     | 没有控制器  |
  | 0    | ROBOT_MODE_DISCONNECTED      | 没有连接   |
  | 1    | ROBOT_MODE_CONFIRM_SAFETY    | 配置安全参数 |
  | 2    | ROBOT_MODE_BOOTING           | 启动     |
  | 3    | ROBOT_MODE_POWER_OFF         | 下电     |
  | 4    | ROBOT_MODE_POWER_ON          | 上电     |
  | 5    | ROBOT_MODE_IDLE              | 空闲     |
  | 6    | ROBOT_MODE_BACKDRIVE         | 拖拽     |
  | 7    | ROBOT_MODE_RUNNING           | 运行     |
  | 8    | ROBOT_MODE_UPDATING_FIRMWARE | 更新固件   |
  | 9    | ROBOT_MODE_ERROR             | 报警     |

## 3.9 PayLoad

- 功能：设置当前的负载

- 格式：PayLoad(weight,inertia)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名     | 类型     | 含义        |
  | ------- | ------ | --------- |
  | weight  | double | 负载重量 kg   |
  | inertia | double | 负载惯量 kgm² |

- 示例：

  PayLoad(3,0.4)

## 3.10 DO

- 功能：设置数字输出端口状态（队列指令） 

- 格式：DO(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                   |
  | ------ | ---- | -------------------- |
  | index  | int  | 数字输出索引，取值范围：1~24     |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平 |

- 示例：

  DO(1,1)

## 3.11 DOExecute

- 功能：设置数字输出端口状态（立即指令）

- 格式：DOExecute(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                   |
  | ------ | ---- | -------------------- |
  | index  | int  | 数字输出索引，取值范围：1~24     |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平 |

- 示例：

  DOExecute(1,1)

## 3.12 ToolDO

- 功能：设置末端数字输出端口状态（队列指令)

- 格式：ToolDO(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                   |
  | ------ | ---- | -------------------- |
  | index  | int  | 数字输出索引，取值范围：1/2      |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平 |

- 示例：

  ToolDO(1,1)

## 3.13 ToolDOExecute

- 功能：设置末端数字输出端口状态（立即指令)

- 格式：ToolDOExecute(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                   |
  | ------ | ---- | -------------------- |
  | index  | int  | 数字输出索引，取值范围：1/2      |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平 |

- 示例：

  ToolDOExecute(1,1)

## 3.14 AO

- 功能：设置控制柜模拟输出端口的电压值（队列指令）

- 格式：AO(index,value)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名   | 类型     | 含义                    |
  | ----- | ------ | --------------------- |
  | index | int    | 模拟输出索引，取值范围：1/2       |
  | value | double | 对应index的电压值，取值范围0~10V |

- 示例：

  AO(1,2)

## 3.15 AOExecute

- 功能：设置控制柜模拟输出端口的电压值（立即指令）

- 格式：AOExecute(index,value)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名   | 类型     | 含义                    |
  | ----- | ------ | --------------------- |
  | index | int    | 模拟输出索引，取值范围：1/2       |
  | value | double | 对应index的电压值，取值范围0~10V |

- 示例：

  AOExecute(1,2)


## 3.16 AccJ

- 功能：设置关节加速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效

- 格式：AccJ,R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                  |
  | ---- | ---- | ------------------- |
  | R    | int  | 关节加速度百分比，取值范围：1~100 |

- 示例：

  AccJ(50)

## 3.17 AccL

- 功能：设置笛卡尔加速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 

- 格式：AccL(R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                  |
  | ---- | ---- | ------------------- |
  | R    | int  | 笛卡尔加速度比例，取值范围：1~100 |

- 示例：

  AccL(50)

##  3.18 SpeedJ

- 功能：设置关节速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效。 

- 格式：SpeedJ(R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                |
  | ---- | ---- | ----------------- |
  | R    | int  | 关节速度比例，取值范围：1~100 |

- 示例：

  SpeedJ(50)

## 3.19 SpeedL

- 功能：设置笛卡尔速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 

- 格式：SpeedL(R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                 |
  | ---- | ---- | ------------------ |
  | R    | int  | 笛卡尔速度比例，取值范围：1~100 |

- 示例：

  SpeedL(50)

## 3.20 Arch

- 功能：设置Jump门型参数索引（起始点抬升高度、最大抬升高度、结束点下降高度）。 

- 格式：Arch(Index)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名   | 类型   | 含义              |
  | ----- | ---- | --------------- |
  | Index | int  | 门型参数索引，取值范围：0~9 |

- 示例：

  Arch(1)

## 3.21 CP

- 功能：设置CP比例。CP即平滑过渡，机械臂从起始点经过中间点到达终点时，经过中间点是以直角方式过渡还是以曲线方式过渡。该指令对Jump指令无效。 

- 格式：CP(R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                |
  | ---- | ---- | ----------------- |
  | R    | int  | 平滑过渡比例，取值范围：1~100 |

- 示例：

  CP(50)

## 3.22 LimZ

- 功能：设置Jump模式最大抬升高度。 
- 格式：LimZ(zValue)
- 参数数量：1
- 支持端口：29999


- 参数详解：1

  | 参数名    | 类型   | 含义                   |
  | ------ | ---- | -------------------- |
  | zValue | int  | 最大抬升高度，不能超过机械臂Z轴极限位置 |

- 示例：

  LimZ(80)

## 3.23 SetArmOrientation

- 功能：设置手系指令。 
- 格式：SetArmOrientation(LorR,UorD,ForN,Config6)
- 参数数量：4
- 支持端口：29999


- 参数详解：4

  | 参数名     | 类型   | 含义                                       |
  | ------- | ---- | ---------------------------------------- |
  | LorR    | int  | 臂方向向前/向后(1/-1)<br /> 1：向前<br />-1：向后     |
  | UorD    | int  | 臂方向肘上/肘下(1/-1)<br /> 1：肘上<br />-1：肘下     |
  | ForN    | int  | 臂方向腕部是否翻转(1/-1)<br />  1：腕不翻转<br />-1：腕翻转 |
  | Config6 | int  | 第六轴角度标识<br />  -1,-2...：第6轴角度为[0,-90]为-1；[-90,-180]为-2；以此类推<br />1,2...：第6轴角度为[0,90]为1；[90,180]为2；以此类推 |

- 示例：

  SetArmOrientation(1,1,-1,1)

## 3.24 PowerOn

- 功能：机器人上电。 
- 格式：PowerOn()
- 参数数量：0
- 支持端口：29999
- **注意：机器人上电完成，需要等待大概10秒钟的时间进行使能操作；**


- 示例：

  PowerOn()

## 3.25 RunScript

- 功能：运行脚本。 

- 格式：RunScript(projectName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名         | 类型     | 含义   |
  | ----------- | ------ | ---- |
  | projectName | string | 脚本名称 |


- 示例：

  RunScript(demo)

## 3.26 StopScript

- 功能：停止脚本。 

- 格式：StopScript()

- 参数数量：0

- 支持端口：29999



- 示例：

  StopScript()

## 3.27 PauseScript

- 功能：暂停脚本。 

- 格式：PauseScript()

- 参数数量：0

- 支持端口：29999



- 示例：

  PauseScript()

## 3.28 ContinueScript

- 功能：继续脚本。 

- 格式：ContinueScript()

- 参数数量：0

- 支持端口：29999



- 示例：

  ContinueScript()

## 3.29 GetHoldRegs

- 功能：读保持寄存器。 

- 格式：GetHoldRegs(id,*addr*, *count*,type)

- 参数数量：4

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型     | 含义                                       |
  | ----- | ------ | ---------------------------------------- |
  | id    | int    | id,从站设备号，最多支持5个设备,取值范围(0~4),当访问控制器内部从站时则要设置为0； |
  | addr  | int    | 保持寄存器的起始地址。取值范围：3095 ~ 4095              |
  | count | int    | 读取指定数量type类型的数据。取值范围：1 ~16               |
  | type  | string | 数据类型：<br /> 如果为空，默认读取16位无符号整数（2个字节，占用1个寄存器）<br /> “U16”：读取16位无符号整数（2个字节，占用1个寄存器）<br /> “U32”：读取32位无符号整数（4个字节，占用2个寄存器）<br /> “F32”：读取32位单精度浮点数（4个字节，占用2个寄存器）<br /> “F64”：读取64位双精度浮点数（8个字节，占用4个寄存器） |


- 示例：从地址3095开始读取一个16位无符号整数

  data= GetHoldRegs(0,3095,1)

## 3.30 SetHoldRegs

- 功能：写保存寄存器。 

- 格式：SetHoldRegs(id,*addr*, *count*,*table*,type)

- 参数数量：5

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型     | 含义                                       |
  | ----- | ------ | ---------------------------------------- |
  | id    | int    | id,从站设备号，最多支持5个设备,取值范围(0~4),当访问控制器内部从站时则要设置为0； |
  | addr  | int    | 保持寄存器的起始地址。取值范围：3095 ~ 4095              |
  | count | int    | 写入指定数量type类型的数据。取值范围：1 ~16               |
  | table | int    | 保持寄存器地址的值                                |
  | type  | string | 数据类型     <br /> 如果为空，默认读取16位无符号整数（2个字节，占用1个寄存器） <br /> “U16”：读取16位无符号整数（2个字节，占用1个寄存器）  <br />“U32”：读取32位无符号整数（4个字节，占用2个寄存器）   <br />“F32”：读取32位单精度浮点数（4个字节，占用2个寄存器）    <br /> “F64”：读取64位双精度浮点数（8个字节，占用4个寄存器） |


- 示例：从地址3095开始，写入两个16位无符号整数值6000，300

  SetHoldRegs(0,3095,2,{6000,300}, “U16”)


## 3.31 SetSafeSkin

- 功能：设置安全皮肤开关状态。 

- 格式：SetSafeSkin(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名    | 类型   | 含义                                |
  | ------ | ---- | --------------------------------- |
  | status | int  | status：电子皮肤开关状态，0：关闭电子皮肤；1：开启电子皮肤 |


- 示例：开启电子皮肤功能

  SetSafeSkin (1)

## 3.32 SetObstacleAvoid

- 功能：设置安全皮肤避障模式开关状态。 

- 格式：SetObstacleAvoid(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名    | 类型   | 含义                                       |
  | ------ | ---- | ---------------------------------------- |
  | status | int  | status：安全皮肤避障模式开关状态，0：关闭安全皮肤避障模式；1：开启安全皮肤避障功能 |


- 示例：开启安全皮肤避障功能

  SetObstacleAvoid(1)

## 3.33 GetTraceStartPose

- 功能：获取轨迹拟合中首个点位。 

- 格式：GetTraceStartPose(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |


- 返回：

  点位坐标值  {x,y,z,a,b,c}

- 示例：

  GetTraceStartPose(recv_string)

## 3.34 GetPathStartPose

- 功能：获取轨迹复现中首个点位。 

- 格式：GetPathStartPose(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |


- 返回：

  关节点位坐标值 {j1,j2,j3,j4,j5,j6}

- 示例：

  GetPathStartPose(recv_string)

## 3.35 PositiveSolution

- 功能：正解。（给定机器人各关节的角度，计算出机器人末端的空间位置）

- 格式：PositiveSolution(J1,J2,J3,J4,J5,J6)

- 参数数量：6

- 支持端口：29999

- 参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | J1   | double | J1 轴位置，单位：度 |
  | J2   | double | J2 轴位置，单位：度 |
  | J3   | double | J3 轴位置，单位：度 |
  | J4   | double | J4 轴位置，单位：度 |
  | J5   | double | J5 轴位置，单位：度 |
  | J6   | double | J6 轴位置，单位：度 |

- 注意，需要已知：

  - 设置好已标定的用户坐标系User
  - 设置好已标定的刀具坐标系Tool
  - 机器人的臂方向SetArmOrientation

- 示例：下发关节角度返回当前的机器人末端的空间位置

  PositiveSolution(0,0,-90,0,90,0)

## 3.36 InverseSolution

- 功能：逆解。（已知机器人末端的位置和姿态，计算机器人各关节的角度值）

- 格式：InverseSolution(X,Y,Z,Rx,Ry,Rz)

- 参数数量：6

- 支持端口：29999

- 参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | X    | double | X 轴位置，单位：毫米 |
  | Y    | double | Y 轴位置，单位：毫米 |
  | Z    | double | Z 轴位置，单位：毫米 |
  | Rx   | double | Rx 轴位置，单位：度 |
  | Ry   | double | Ry轴位置，单位：度  |
  | Rz   | double | Rz轴位置，单位：度  |

- 注意，需要已知：

  - 设置好已标定的用户坐标系User
  - 设置好已标定的刀具坐标系Tool
  - 机器人的臂方向SetArmOrientation

- 示例：下发关节角度返回当前的机器人末端的空间位置

  PositiveSolution(0,-247,1050,-90,0,180)


## 3.37 SetCollisionLevel

- 功能：设置碰撞等级。 

- 格式：SetCollisionLevel(level)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型   | 含义                                       |
  | ----- | ---- | ---------------------------------------- |
  | level | int  | level: 碰撞等级<br /> 0：关闭碰撞检测<br /> 1~5：等级越高越灵敏 |


- 示例：

  SetCollisionLevel(1)

## 3.38 HandleTrajPoints

- 功能：轨迹文件的预处理。

- 格式：HandleTrajPoints(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |

- **备注：若用户下发不带参数的该指令，代表查询当前指令的结果；**返回：返回为-3表示文件内容错误，返回为-2表示文件不存在，返回为-1表示预处理没有完成；返回为0表示预处理完成，没有错误；返回大于0的结果表示当前返回结果对应的点位有问题；

- 示例：下发轨迹名为recv_string做预处理，然后在一定周期查询预处理结果；

  HandleTrajPoints(recv_string)

  HandleTrajPoints()

## 3.39 GetSixForceData

- 功能：获取六维力数据

- 格式：GetSixForceData()

- 参数数量：0

- 支持端口：29999

- 返回：当前六维力数据原始值；

- 示例：

  local P1 = GetSixForceData()

## 3.40 GetAngle

- 功能：获取关节坐标系下机械臂的实时位姿

- 格式：GetAngle()

- 参数数量：0

- 支持端口：29999

- 返回：机械臂当前位置的关节坐标值；

- 示例：

   GetAngle()

## 3.41 GetPose

- 功能：获取笛卡尔坐标系下机械臂的实时位姿(如果设置了用户坐标系或工具坐标系，则获取的位姿为当前坐标系下的位姿)

- 格式：GetPose()

- 参数数量：0

- 支持端口：29999

- 返回：机械臂当前位置的笛卡尔坐标值；

- 示例：

  GetPose()

## 3.42 EmergencyStop

- 功能：急停，机器人记录

- 格式：EmergencyStop()

- 参数数量：0

- 支持端口：29999

- 示例：

   EmergencyStop()

   ​

## 3.43 Sync

- 功能：阻塞程序执行队列指令，待所有队列指令执行完才返回。
- 格式：Sync()
- 参数数量：0
- 支持端口：29999
- 返回：`done`

​

# 4.通信协议----实时反馈端口

​	30003端口即实时反馈端口除了用于发送约定的运动相关协议外，还有其他功能，客户端每20ms能收到一次机器人如下表所示的信息。通过实时反馈端口每次收到的数据包有1440个字节，这些字节以标准的格式排列。下表是字节排列的顺序表。

|        意义/Meaning         |   数据类型/Type    | 值的数目/Number of values | 字节大小/Size in bytes | 字节位置值/Byte position value |                 描述/Notes                 |
| :-----------------------: | :------------: | :-------------------: | :----------------: | :-----------------------: | :--------------------------------------: |
|       Message Size        | unsigned short |           1           |         2          |        0000 ~ 0001        |  消息字节总长度/Total message length in bytes   |
|                           | unsigned short |           3           |         6          |        0002 ~ 0007        |                   保留位                    |
|    Digital input bits     |     double     |           1           |         8          |        0008 ~ 0015        | 数字输入/Current state of the digital inputs. |
|      Digital outputs      |     double     |           1           |         8          |        0016 ~ 0023        |                   数字输出                   |
|        Robot Mode         |     double     |           1           |         8          |        0024 ~ 0031        |             机器人模式/Robot mode             |
|     Controller Timer      |     double     |           1           |         8          |        0032 ~ 0039        | 程序扫描时间/Controller realtime thread execution time |
|           Time            |     double     |           1           |         8          |        0040 ~ 0047        | 控制器通电时间/Time elapsed since the controller was started |
|        test_value         |     double     |           1           |         8          |        0048 ~ 0055        |     内存结构测试标准值  0x0123 4567 89AB CDEF     |
|        Safety Mode        |     double     |           1           |         8          |        0056 ~ 0063        |             安全模式/Safety mode             |
|       Speed scaling       |     double     |           1           |         8          |        0064 ~ 0071        | 速度比例/Speed scaling of the trajectory limiter |
|   Linear momentum norm    |     double     |           1           |         8          |        0072 ~ 0079        | 机器人当前动量/Norm of Cartesian linear momentum |
|          V main           |     double     |           1           |         8          |        0080 ~ 0087        |     控制板电压/Masterboard: Main voltage      |
|          V robot          |     double     |           1           |         8          |        0088 ~ 0095        |  机器人电压/Masterboard: Robot voltage (48V)  |
|          I robot          |     double     |           1           |         8          |        0096 ~ 0103        |     机器人电流/Masterboard: Robot current     |
|       Program state       |     double     |           1           |         8          |        0104 ~ 0111        |            程序状态/Program state            |
|       Safety Status       |     double     |           1           |         8          |        0112 ~ 0119        |            安全状态/Safety status            |
| Tool Accelerometer values |     double     |           3           |         24         |        0120 ~ 0143        | TCP加速度/Tool x,y and z accelerometer values |
|      Elbow position       |     double     |           3           |         24         |        0144 ~ 0167        |            肘位置/Elbow position            |
|      Elbow velocity       |     double     |           3           |         24         |        0168 ~ 0191        |            肘速度/Elbow velocity            |
|         q target          |     double     |           6           |         48         |        0192 ~ 0239        |      目标关节位置/Target joint positions       |
|         qd target         |     double     |           6           |         48         |        0240 ~ 0287        |      目标关节速度/Target joint velocities      |
|        qdd target         |     double     |           6           |         48         |        0288 ~ 0335        |    目标关节加速度/Target joint accelerations    |
|         I target          |     double     |           6           |         48         |        0336 ~ 0383        |       目标关节电流/Target joint currents       |
|         M target          |     double     |           6           |         48         |        0384 ~ 0431        |  目标关节扭矩/Target joint moments (torques)   |
|         q actual          |     double     |           6           |         48         |        0432 ~ 0479        |      实际关节位置/Actual joint positions       |
|         qd actual         |     double     |           6           |         48         |        0480 ~ 0527        |      实际关节速度/Actual joint velocities      |
|         I actual          |     double     |           6           |         48         |        0528 ~ 0575        |       实际关节电流/Actual joint currents       |
|         I control         |     double     |           6           |         48         |        0576 ~ 0623        |   关节控制电流/Joint control currents(暂时0代替)   |
|    Tool vector actual     |     double     |           6           |         48         |        0624 ~ 0671        | TCP笛卡尔实际坐标值/Actual Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation |
|     TCP speed actual      |     double     |           6           |         48         |        0672 ~ 0719        | TCP笛卡尔实际速度值/Actual speed of the tool given in Cartesian coordinates |
|         TCP force         |     double     |           6           |         48         |        0720 ~ 0767        |   TCP力值/Generalised forces in the TCP    |
|    Tool vector target     |     double     |           6           |         48         |        0768 ~ 0815        | TCP笛卡尔目标坐标值/Target Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation |
|     TCP speed target      |     double     |           6           |         48         |        0816 ~ 0863        | TCP笛卡尔目标速度值/Target speed of the tool given in Cartesian coordinates |
|    Motor temperatures     |     double     |           6           |         48         |        0864 ~ 0911        | 关节温度/Temperature of each joint in degrees celsius |
|        Joint Modes        |     double     |           6           |         48         |        0912 ~ 0959        |        关节控制模式/Joint control modes        |
|         V actual          |     double     |           6           |         48         |        960  ~ 1007        |        关节电压/Actual joint voltages        |
|                           |     double     |          54           |        432         |        1008 ~ 1439        |                   保留位                    |
|           TOTAL           |                |          183          |        1440        |                           |     183values in a 1440byte package      |

其中Robot Mode返回机器人模式：                                                          

| 模式   | 描述                           | 备注     |
| ---- | ---------------------------- | ------ |
| -1   | ROBOT_MODE_NO_CONTROLLER     | 没有控制器  |
| 0    | ROBOT_MODE_DISCONNECTED      | 没有连接   |
| 1    | ROBOT_MODE_CONFIRM_SAFETY    | 配置安全参数 |
| 2    | ROBOT_MODE_BOOTING           | 启动     |
| 3    | ROBOT_MODE_POWER_OFF         | 下电     |
| 4    | ROBOT_MODE_POWER_ON          | 上电     |
| 5    | ROBOT_MODE_IDLE              | 空闲     |
| 6    | ROBOT_MODE_BACKDRIVE         | 拖拽     |
| 7    | ROBOT_MODE_RUNNING           | 运行     |
| 8    | ROBOT_MODE_UPDATING_FIRMWARE | 更新固件   |
| 9    | ROBOT_MODE_ERROR             | 报警     |

- 说明：
  - [ ] 本体下电，状态为3；
  - [ ] 本体有电，但没有使能，状态为4；
  - [ ] 使能成功后，则状态为5 ；
  - [ ] 机器人进入拖拽模式(使能状态)，状态为6；
  - [ ] 机器人发生运动，状态为7；
  - [ ] 优先级：上电<空闲<拖拽=运行<下电<报警
  - [ ] 其中报警优先级最高，其他状态同时存在时，若有报警，先将状态置9；



其中Program state返回状态如下：

- 停止状态	返回0
- 暂停状态返回1
- 运行状态返回2



如下表所示为实时反馈端口支持的运动命令协议，实时反馈端口仅接收命令不做反馈；

| 指令           | 描述                          |
| ------------ | --------------------------- |
| MovJ         | 点到点运动，目标点位为笛卡尔点位。           |
| MovL         | 直线运动，目标点位为笛卡尔点位             |
| JointMovJ    | 点到点运动，目标点位为关节点位             |
| Jump         | 门型运动，仅支持笛卡尔点位               |
| RelMovJ      | 以点到点方式运动至笛卡尔偏移位置            |
| RelMovL      | 以直线运动至笛卡尔偏移位置               |
| MovLIO       | 直线运动过程中并行设置数字输出端口的状态，可设置多组  |
| MovJIO       | 点到点运动过程中并行设置数字输出端口的状态，可设置多组 |
| Arc          | 圆弧运动。需结合其他运动指令完成圆弧运动。       |
| Circle       | 整圆运动，需结合其他运动指令完成整圆运动        |
| ServoJ       | 基于关节空间的动态跟随命令               |
| ServoP       | 基于笛卡尔空间的动态跟随命令              |
| MoveJog      | 关节点动                        |
| StartTrace   | 轨迹拟合                        |
| StartPath    | 轨迹复现                        |
| StartFCTrace | 带力控的轨迹拟合                    |

## 4.1 MovJ

- 功能：点到点运动，目标点位为笛卡尔点位。

- 格式：MovJ(X,Y,Z,Rx,Ry,Rz)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | X    | double | X 轴位置，单位：毫米 |
  | Y    | double | Y 轴位置，单位：毫米 |
  | Z    | double | Z 轴位置，单位：毫米 |
  | Rx   | double | Rx 轴位置，单位：度 |
  | Ry   | double | Ry 轴位置，单位：度 |
  | Rz   | double | Rz 轴位置，单位：度 |

- 示例：

  MovJ(-500,100,200,150,0,90)



## 4.2 MovL

- 功能：直线运动，目标点位为笛卡尔点位。

- 格式：MovL(X,Y,Z,Rx,Ry,Rz)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | X    | double | X 轴位置，单位：毫米 |
  | Y    | double | Y 轴位置，单位：毫米 |
  | Z    | double | Z 轴位置，单位：毫米 |
  | Rx   | double | Rx 轴位置，单位：度 |
  | Ry   | double | Ry 轴位置，单位：度 |
  | Rz   | double | Rz 轴位置，单位：度 |

- 示例：

  MovL(-500,100,200,150,0,90)

## 4.3 JointMovJ

- 功能：点到点运动，目标点位为关节点位。

- 格式：JointMovJ(J1,J2,J3,J4,J5,J6)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | J1   | double | J1 轴位置，单位：度 |
  | J2   | double | J2 轴位置，单位：度 |
  | J3   | double | J3 轴位置，单位：度 |
  | J4   | double | J4 轴位置，单位：度 |
  | J5   | double | J5 轴位置，单位：度 |
  | J6   | double | J6 轴位置，单位：度 |

- 示例：

  JointMovJ(0,0,-90,0,90,0)

## 4.4 Jump

待定

## 4.5 RelMovJ

- 功能：以点到点方式运动至笛卡尔偏移位置。

- 格式：RelMovJ(offset1,offset2,offset3,offset4,offset5,offset6)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名     | 类型     | 含义              |
  | ------- | ------ | --------------- |
  | offset1 | double | 关节J1轴方向偏移，单位：度  |
  | offset2 | double | 关节J2轴方向偏移，单位：度  |
  | offset3 | double | 关节J3轴方向偏移，单位：度  |
  | offset4 | double | 关节J4轴方向偏移，单位：度  |
  | offset5 | double | 关节J5轴方向偏移，单位：度  |
  | offset6 | double | 关节J6轴轴方向偏移，单位：度 |

- 示例：

  RelMovJ(10,10,10,10,10,10)

## 4.6 RelMovL

- 功能：以直线运动至笛卡尔偏移位置。

- 格式：RelMovL(offsetX,offsetY,offsetZ)

- 参数数量：3

- 支持端口：30003

- 参数详解：3

  | 参数名     | 类型     | 含义           |
  | ------- | ------ | ------------ |
  | offsetX | double | X轴方向偏移，单位：mm |
  | offsetY | double | Y轴方向偏移，单位：mm |
  | offsetZ | double | Z轴方向偏移，单位：mm |

- 示例：

  RelMovL(10,10,10)

## 4.7 MovLIO

- 功能：在直线运动时并行设置数字输出端口状态，目标点位为笛卡尔点位。

- 格式：MovLIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status})

- 参数数量：10

- 支持端口：30003

- 参数详解：10

  | 参数名      | 类型     | 含义                                       |
  | -------- | ------ | ---------------------------------------- |
  | X        | double | X 轴位置，单位：毫米                              |
  | Y        | double | Y 轴位置，单位：毫米                              |
  | Z        | double | Z 轴位置，单位：毫米                              |
  | Rx       | double | Rx 轴位置，单位：度                              |
  | Ry       | double | Ry 轴位置，单位：度                              |
  | Rz       | double | Rz 轴位置，单位：度                              |
  | Mode     | int    | 设置Distance模式，0：Distance为距离百分比；1：Distance为离起始点或目标点的距离 |
  | Distance | int    | 运行指定的距离:  若Mode为0，则Distance表示起始点与目标点之间距离的百分比，取值范围：0~100; 若Distance取值为正，则表示离起始点的距离; 若Distance取值为负，则表示离目标点的距离 |
  | Index    | int    | 数字输出索引，取值范围：1~24                         |
  | Status   | int    | 数字输出状态，取值范围：0或1                          |

- 示例：

  MovLIO(-500,100,200,150,0,90,{0,50,1,0})

## 4.8 MovJIO

- 功能：点到点运动时并行设置数字输出端口状态，目标点位为笛卡尔点位。

- 格式：MovJIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status})

- 参数数量：不固定

- 支持端口：30003

- 参数详解：10

  | 参数名      | 类型     | 含义                                       |
  | -------- | ------ | ---------------------------------------- |
  | X        | double | X 轴位置，单位：毫米                              |
  | Y        | double | Y 轴位置，单位：毫米                              |
  | Z        | double | Z 轴位置，单位：毫米                              |
  | Rx       | double | Rx轴位置，单位：度                               |
  | Ry       | double | Ry 轴位置，单位：度                              |
  | Rz       | double | Rz轴位置，单位：度                               |
  | Mode     | int    | 设置Distance模式，0：Distance为距离百分比；1：Distance为离起始点或目标点的距离 |
  | Distance | int    | 运行指定的距离:  若Mode为0，则Distance表示起始点与目标点之间距离的百分比，取值范围：0~100; 若Distance取值为正，则表示离起始点的距离; 若Distance取值为负，则表示离目标点的距离 |
  | Index    | int    | 数字输出索引，取值范围：1~24                         |
  | Status   | int    | 数字输出状态，取值范围：0或1                          |

- 示例：

  MovJIO(-500,100,200,150,0,90,{0,50,1,0})

## 4.9 Arc

- 功能：：从当前位置以圆弧插补方式移动至笛卡尔坐标系下的目标位置。

  ​		该指令需结合其他运动指令确定圆弧起始点。

- 格式：Arc(X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2)

- 参数数量：12

- 支持端口：30003

- 参数详解：12

  | 参数名  | 类型     | 含义                  |
  | ---- | ------ | ------------------- |
  | X1   | double | 表示圆弧中间点X1 轴位置，单位：毫米 |
  | Y1   | double | 表示圆弧中间点Y1 轴位置，单位：毫米 |
  | Z1   | double | 表示圆弧中间点Z1 轴位置，单位：毫米 |
  | Rx1  | double | 表示圆弧中间点Rx1轴位置，单位：度  |
  | Ry1  | double | 表示圆弧中间点Ry1轴位置，单位：度  |
  | Rz1  | double | 表示圆弧中间点Rz1轴位置，单位：度  |
  | X2   | double | 表示圆弧结束点X2 轴位置，单位：毫米 |
  | Y2   | double | 表示圆弧结束点Y2 轴位置，单位：毫米 |
  | Z2   | double | 表示圆弧结束点Z2 轴位置，单位：毫米 |
  | Rx2  | double | 表示圆弧结束点Rx2轴位置，单位：度  |
  | Ry2  | double | 表示圆弧结束点Ry2轴位置，单位：度  |
  | Rz2  | double | 表示圆弧结束点Rz2 轴位置，单位：度 |

- 示例：

## 4.10 Circle

- 功能：：整圆运动，需结合其他运动指令完成整圆运动。

- 格式：Circle(count,X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2)

- 参数数量：13

- 支持端口：30003

- 参数详解：13

  | 参数名   | 类型     | 含义           |
  | ----- | ------ | ------------ |
  | count | int    | 整圆个数         |
  | X1    | double | X1 轴位置，单位：毫米 |
  | Y1    | double | Y1 轴位置，单位：毫米 |
  | Z1    | double | Z1 轴位置，单位：毫米 |
  | Rx1   | double | Rx1轴位置，单位：度  |
  | Ry1   | double | Ry1轴位置，单位：度  |
  | Rz1   | double | Rz1轴位置，单位：度  |
  | X2    | double | X2 轴位置，单位：毫米 |
  | Y2    | double | Y2 轴位置，单位：毫米 |
  | Z2    | double | Z2 轴位置，单位：毫米 |
  | Rx2   | double | Rx2轴位置，单位：度  |
  | Ry2   | double | Ry2轴位置，单位：度  |
  | Rz2   | double | Rz2轴位置，单位：度  |

- 示例：



## 4.11 ServoJ

- 功能：基于关节空间的动态跟随命令。

- 格式：ServoJ(J11,J12,J13,J14,J15,J16)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名  | 类型     | 含义              |
  | ---- | ------ | --------------- |
  | J11  | double | 点J11 轴位置，单位：度   |
  | J12  | double | P1点J12 轴位置，单位：度 |
  | J13  | double | P1点J13 轴位置，单位：度 |
  | J14  | double | P1点J14 轴位置，单位：度 |
  | J15  | double | P1点J15 轴位置，单位：度 |
  | J16  | double | P1点J16 轴位置，单位：度 |

- 示例：

  ServoJ(0,0,-90,0,90,0)

## 4.12 ServoP

- 功能：基于笛卡尔空间的动态跟随命令。

- 格式：ServoP(X1,Y1,Z1,Rx1,Ry1,Rz1)

- 参数数量：6

- 支持端口：30003

- 参数详解：6

  | 参数名  | 类型     | 含义           |
  | ---- | ------ | ------------ |
  | X1   | double | X 1轴位置，单位：毫米 |
  | Y1   | double | Y 1轴位置，单位：毫米 |
  | Z1   | double | Z1 轴位置，单位：毫米 |
  | Rx1  | double | Rx 1轴位置，单位：度 |
  | Ry1  | double | Ry1 轴位置，单位：度 |
  | Rz1  | double | Rz1轴位置，单位：度  |

- 示例：

  ServoP(-500,100,200,150,0,90）


## 4.13 MoveJog

- 功能：点动运动，不固定距离运动

- 格式：MoveJog(axisID)

- 参数数量：1

- 支持端口：30003

- **注意：命令下发后，须另外下发MoveJog()停止命令控制机器人停止运动；另下发非指定string内容的参数都会导致机器人停止；**

- 参数详解：1

  | 参数名    | 类型     | 含义                                       |
  | ------ | ------ | ---------------------------------------- |
  | axisID | string | 点动运动轴<br />J1+ 表示关节1正方向运动        J1- 表示关节1负方向运动 <br />J2+ 表示关节2正方向运动        J2- 表示关节2负方向运动<br />J3+ 表示关节3正方向运动        J3- 表示关节3负方向运动<br />J4+ 表示关节4正方向运动        J4- 表示关节4负方向运动<br />J5+ 表示关节5正方向运动        J5- 表示关节5负方向运动<br />J6+ 表示关节6正方向运动        J6- 表示关节6负方向运动<br />X+ 表示X轴正方向运动             X- 表示X轴负方向运动 <br />Y+ 表示Y轴正方向运动             Y- 表示Y轴负方向运动<br />Z+ 表示Z轴正方向运动             Z- 表示Z轴负方向运动<br />Rx+ 表示Rx轴正方向运动        Rx- 表示Rx轴负方向运动<br />Ry+ 表示Ry轴正方向运动        Ry- 表示Ry轴负方向运动<br />Rz+ 表示Rz轴正方向运动        Rz- 表示Rz轴负方向运动<br /> |

- 示例：J2负方向运动，再停止点动

  MoveJog(j2-)

  MoveJog()


## 4.14 StartTrace

- 功能：轨迹拟合。(轨迹文件笛卡尔点)

- 格式：StartTrace(traceName)

- 参数数量：1

- 支持端口：30003

- **备注：用户可以通过获取RobotMode查询机器人运行状态，若在ROBOT_MODE_RUNNING表示机器人在轨迹拟合运行中，达到ROBOT_MODE_IDLE表示轨迹拟合运行完成，ROBOT_MODE_ERROR表示报警；**

- 参数详解：1

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |

- 示例：先获取名字recv_string的轨迹的首个关节点{x,y,z,rx,ry,rz}，在点到点运动{x,y,z,rx,ry,rz}后，在下发名字recv_string做轨迹拟合；

  GetTraceStartPose(recv_string)

  MovJ(x,y,z,rx,ry,rz)

  StartTrace(recv_string)

## 4.15 StartPath

- 功能：轨迹复现。(轨迹文件关节点)

- 格式：StartPath(traceName,const,cart)

- 参数数量：3

- 支持端口：30003

- **备注：用户可以通过获取RobotMode查询机器人运行状态，若在ROBOT_MODE_RUNNING表示机器人在轨迹复现运行中，达到ROBOT_MODE_IDLE表示轨迹复现运行完成，ROBOT_MODE_ERROR表示报警；**

- 参数详解：3

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |
  | const     | int    | const=1时，匀速复现，轨迹中的停顿及死区会被移除;<br />const=0时，按照原速复现； |
  | cart      | int    | cart=1时，按笛卡尔路径复现；<br />cart=0时，按关节路径复现；  |

- 示例：  先获取名字recv_string的轨迹的首个点{j1,j2,j3,j4,j5,j6}，在点到点运动{j1,j2,j3,j4,j5,j6}后，在下发名字recv_string按照原速复现，按笛卡尔路径匀速复现；

  GetPathStartPose(recv_string)

  JointMoveJ(j1,j2,j3,j4,j5,j6)

  StartPath(recv_string,0,1)

  

  


- 功能：带力控的轨迹拟合。(轨迹文件笛卡尔点)

- 格式：StartFCTrace(traceName)

- 参数数量：1

- 支持端口：30003

- **备注：用户可以通过获取RobotMode查询机器人运行状态，若在ROBOT_MODE_RUNNING表示机器人在轨迹复现运行中，达到ROBOT_MODE_IDLE表示轨迹复现运行完成，ROBOT_MODE_ERROR表示报警；**

- 参数详解：1

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |

- 示例：先获取名字recv_string的轨迹的首个点{x,y,z,rx,ry,rz}，在点到点运动{x,y,z,rx,ry,rz}后，在下发名字recv_string按照原速复现，按笛卡尔路径匀速复现；

  GetTraceStartPose(recv_string)

  MovJ(x,y,z,rx,ry,rz)

  StartFCTrace(recv_string)

  ​


