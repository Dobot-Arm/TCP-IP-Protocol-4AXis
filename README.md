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

- V1.8-2021/10/15:根据PX需要末端485支持使用RTU的功能的指令，新增支持Modbus的TCP和RTU通信的指令；详情见《末端485的modbusRTU功能拓展》以及《工业项目Modbus需求及概要设计方案》；

- V1.9-2021/10/25:跟进PX的反馈，优化正逆解参数内容；新增获取错误码接口；实时反馈端口新增获取用户坐标、工具坐标以及手系的参数；

- V1.10-2021/10/27:跟进PX的需求，使能新增四个可选参数((负载 + 偏心)、实时数据上报负载参数、实时数据上报用户/工具坐标系的矩阵、实时数据上报关节实际力矩、实时数据上报速度加速度等相关的比率共计9个；

- V1.10-2021/11/03:PX分支拆分30003端口为30003和30004；其中30003端口仅作为下发运行指令相关接口；30004端口仅做为实时反馈端口；注意：30003端口的拆分预计在控制器3.5.2版本实现，3.5.1暂时还是一个30003端口进行实时反馈以及下发运动相关指令；

- V2.0-2021/11/24:对TCPIP功能做较大的优化；主要优化内容：定义返回格式 ；RobotMode返回修改；新增30005以及30006反馈机器人数据端口等；CR控制器支持版本在3.5.2版本及以上；

- V2.0-2021/11/25:优化RobotMode返回的运行的歧义,为保持与控制器3.5.1版本兼容性，之前351版本关键机器人状态返回值没有做修改；如：空闲、拖拽、运行、报警状态，新增抱闸松开、轨迹录制、暂停以及点动等；实时反馈描述的统一；根据建议将30005端口的反馈周期改成200ms；

- V2.1-2021/11/30：新增DI、ToolDI、AI、ToolAI、DIGroup、DOGroup获取数字输入信号、末端数字输入、模拟输入、末端模拟输入、组数字输入以及组数字输出指令；轨迹复现、轨迹拟合以及带力控的轨迹拟合指令新增返回运行到第几个点的参数；实时反馈数据新增当前六维力数据原始值；

- V2.1-2021/12/14：LK项目新增实时反馈的按钮板五个按钮的触发信号；

- V2.1-2021/12/21： 新增机器类型实时反馈并表格说明值代表机器型号；

- V2.2-2021/12/22： 修复一些运行指令可选参数参数错误以及统一运动指令AccL和AccJ可选参数；

- V2.3-2022/01/07：实时反馈新增四元数以及当前时间戳；新增开关抱闸指令；新增若干指令说明；

- V2.3-2022/01/12：运动指令新增用户和工具坐标系的可选项设置；对逆解接口进行重新设计；

- V2.3-2022/01/21：新增实时反馈的TCP传感器力值(通过六维力计算)字段；

- V2.4-2022/01/24：修改RelMovJ指令参数；根据《相对运动指令方案》新增5条相对运动相关指令；

- V2.5-2022/02/08:   新增StartDrag、StopDrag、SetCollideDrag、SetTerminalKeys、SetTerminal485、GetTerminal485等指令分布对应进入拖拽、退出拖拽模式、设置是否强制进入拖拽模式、设置末端按键功能使能状态、设置末端485的参数、获取末端485的参数。GetErrorID的描述，将碰撞检测和电子皮肤碰撞的错误码返回设置到控制器和算法错误码数组中，并将错误码描述章节中-2，-3的描述删除；

- V2.6-2022/02/10:  修改正逆解指令的示例，添加LoadSwitch指令，配合PayLoad使用；

- V2.7-2022/02/11:  带力控的轨迹拟合添加说明，需要在执行StartFCTrace后加上Sync；

- V2.8-2022/02/16:  ServoP/ServoJ指令添加使用频率限制，修改SetCollideDrag开关状态，改为0关闭，1开启，与文档其它设置指令统一；

  



# 1. 综述

​	CR机器人现支持两种远程控制方式：**远程I/O模式、远程Modbus模式**；具体控制方式详见《Dobot-CR-Series-Robot-APP-User-Guide-V3.7》文档中软件使用说明->设置->远程控制章节中；

​	以上两种方式主要针对**远程运行脚本的控制**；由于基于TCP/IP的通讯具有成本低、可靠性高、实用性强、性能高等特点；许多工业自动化项目对支持TCP/IP协议控制机器人需求广泛，因此CR/MG400/M1Pro机器人将设计在TCP/IP协议的基础上，提供了丰富的接口用于与外部设备的交互；

​	关于TCP/IP协议的支持，CR系列机器人的控制器版本需V3.5.1.9及以上，MG400/M1Pro机器人的控制器版本需V1.5.4.0及以上。

# 2.消息格式

​	根据设计，CR机器人会开启29999、30003、30004、30005以及30006服务器端口；

​	29999服务器端口(以下简称Dashboard端口)通过一发一收的方式负责接收一些设置以及运动控制相关的指令，即**Dashboard端口接收到客户端约定消息格式后会将结果反馈客户端**；

​	30003服务器端口仅**接收客户端的约定消息格式，不做反馈**；(注意：30003端口的拆分成30003和30004端口预计在控制器3.5.2版本实现，3.5.1暂时还是一个30003端口进行实时反馈以及下发运动相关指令；)

​	30004服务器端口(以下简称实时反馈端口)**每8ms反馈机器人的信息；**30005服务器端口**每200ms反馈机器人的信息**，30006端口为**可配置**的反馈机器人信息端口(默认为每**50ms**反馈)；

​	说明：TCPIP远程控制指令不区分大小写格式；如ENABLEROBOT()指令或者enablerobot()指令或者eNabLErobOt()指令控制器都会按照使能的指令执行；

## 2.1 下发格式

​	`消息名称(Param1,Param2,Param3……Paramn)`
​	消息格式如上所示，由一个消息名称，括号内由参数组成， 每一个参数之间以英文逗号  ”,”  相隔，一个完整的消息以右括号结束。

​	消息命令与消息应答都是 ASCII 码格式(字符串形式)。

## 2.2 返回格式

### 2.2.1 返回：

​	`"ErrorID,{value,...,valuen},消息名称(Param1,Param2,Param3……Paramn);"`

​	消息格式如上所示，ErrorID为0时表示命令接收成功；返回非0则代表命令有错误，具体的错误描述见第六章节;{value1,value2,value3,...,valuen}表示返回值，没有返回值则返回{}；消息名称(Param1,Param2,Param3……Paramn)指下发的内容。

​	例：

​		MovL(-500,100,200,150,0,90)
​		返回：0,{},MovL(-500,100,200,150,0,90);   //0表示接收成功 没有返回值返回{}


​		Mov(-500,100,200,150,0,90)
​		报警： -10000,{},Mov(-500,100,200,150,0,90);   //-10000表示命令错误  没有返回值返回{}



# 3.通信协议---Dashboard端口

​	上位机可以通过29999端口直接发送一些设置相关的指令给机器人，这些指令CR自己定义的，这些功能被称为Dashboard。如表是Dashboard的指令列表。可以通过Dashboard的指令实现对机器人使能/下使能、复位等控制；

​	机器人设置相关指令如下：

| 指令              | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| EnableRobot       | 使能机器人                                                   |
| DisableRobot      | 去使能机器人                                                 |
| ClearError        | 复位，用于清除错误                                           |
| ResetRobot        | 机器人停止当前动作，重新接收使能，规划停                     |
| SpeedFactor       | 设置全局速率比                                               |
| User              | 选择已标定的用户坐标系（笛卡尔空间显示值 实际生效根据点）    |
| Tool              | 选择已标定的工具坐标系                                       |
| RobotMode         | 机器人模式                                                   |
| PayLoad           | 设置负载                                                     |
| DO                | 设置数字量输出端口状态                                       |
| DOExecute         | 设置数字量输出端口状态（立即指令）                           |
| ToolDO            | 设置末端数字量输出端口状态                                   |
| ToolDOExecute     | 设置末端数字量输出端口状态（立即指令）                       |
| AO                | 设置模拟量输出端口状态                                       |
| AOExecute         | 设置模拟量输出端口状态（立即指令）                           |
| AccJ              | 设置关节加速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效 |
| AccL              | 设置笛卡尔加速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 |
| SpeedJ            | 设置关节速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效。 |
| SpeedL            | 设置笛卡尔速度比例。该指令仅对MovL、MovLIO、MovLR、Jump、Arc、Circle指令有效。 |
| Arch              | 设置Jump门型参数索引（起始点抬升高度、最大抬 升高度、结束点下降高度） |
| CP                | 运动时设置平滑过渡                                           |
| LimZ              | 设置Jump模式最大抬升高度                                     |
| SetArmOrientation | 设置手系                                                     |
| PowerOn           | 机器人上电                                                   |
| RunScript         | 运行脚本                                                     |
| StopScript        | 停止脚本                                                     |
| PauseScript       | 暂停脚本                                                     |
| ContinueScript    | 继续脚本                                                     |
| SetSafeSkin       | 设置安全皮肤开关状态                                         |
| SetObstacleAvoid  | 设置安全皮肤避障模式开关状态                                 |
| GetTraceStartPose | 获取轨迹拟合中首个点位                                       |
| GetPathStartPose  | 获取轨迹复现中首个点位                                       |
| PositiveSolution  | 正解                                                         |
| InverseSolution   | 逆解                                                         |
| SetCollisionLevel | 设置碰撞等级                                                 |
| HandleTrajPoints  | 轨迹文件预处理                                               |
| GetSixForceData   | 获取六维力数据                                               |
| GetAngle          | 获取关节坐标系下机械臂的实时位姿                             |
| GetPose           | 获取笛卡尔坐标系下机械臂的实时位姿                           |
| EmergencyStop     | 急停                                                         |
| Sync              | 阻塞程序执行队列指令，待所有队列指令执行完才返回             |
| ModbusCreate      | 创建Modbus主站，并和从站建立连接                             |
| ModbusClose       | 和Modbus从站断开连接                                         |
| GetInBits         | 读离散输入功能                                               |
| GetInRegs         | 读输入寄存器                                                 |
| GetCoils          | 读线圈功能                                                   |
| SetCoils          | 写线圈功能                                                   |
| GetHoldRegs       | 读保存寄存器                                                 |
| SetHoldRegs       | 写保存寄存器                                                 |
| GetErrorID        | 获取错误ID                                                   |
| DI                | 获取数字量输入端口状态                                       |
| ToolDI            | 获取末端数字量输入端口状态                                   |
| AI                | 获取模拟量输入端口电压值                                     |
| ToolAI            | 获取末端模拟量输入端口电压值                                 |
| DIGroup           | 获取输入组端口状态                                           |
| DOGroup           | 设置数字输出组端口状态                                       |
| BrakeControl      | 抱闸控制                                                     |
| StartDrag         | 进入拖拽                                                     |
| StopDrag          | 退出拖拽                                                     |
| SetCollideDrag    | 强制进入拖拽                                                 |
| SetTerminalKeys   | 设置末端按键功能使能状态                                     |
| SetTerminal485    | 设置末端485的参数                                            |
| GetTerminal485    | 获取末端485的参数                                            |
| LoadSwitch        | 控制负载设置状态                                             |



## 3.1 EnableRobot

- 功能：使能机器人

- 格式：EnableRobot()

- 支持端口：29999

- 可选参数详解：

  | 参数名     | 类型     | 含义                           |
  | ------- | ------ | ---------------------------- |
  | load    | double | 负载重量kg，取值范围：0~机器额定负载；        |
  | centerX | double | X方向偏心距离mm，取值范围：-500mm~500mm； |
  | centerY | double | Y方向偏心距离mm，取值范围：-500mm~500mm； |
  | centerZ | double | Z方向偏心距离mm，取值范围：-500mm~500mm； |



- 返回：

  ErrorID,{},EnableRobot();

- 示例：

  EnableRobot()

- 说明：**可选参数数量：0/1/4**  （不填参数，正常接收ErrorID返回0；填一个参数默认为负载重量参数,ErrorID返回0；填四个参数分别表示负载重量、X方向偏心距、Y方向偏心距以及Z方向偏心距，ErrorID返回0;失败返回错误码,参考第六章；）

  



## 3.2 DisableRobot

- 功能：下使能机器人

- 格式：DisableRobot()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},DisableRobot();

- 示例：

  DisableRobot()



## 3.3 ClearError

- 功能：清错机器人

- 格式：ClearError()

- 参数数量：0

- 支持端口：29999



- 返回：

  ErrorID,{},ClearError();

- 示例：

  ClearError()

- 说明：清除报警后，用户可以根据RobotMode来判断机器人是否还处于报警状态；对于清除不掉的报警需要重启控制柜解决；(详见GetErrorID说明)

## 3.4 ResetRobot

- 功能：机器人停止

- 格式：ResetRobot()

- 参数数量：0

- 支持端口：29999



- 返回：

  ErrorID,{},ResetRobot();

- 示例：

  ResetRobot()

## 3.5 SpeedFactor

- 功能：设置全局速度比例。 

- 格式：SpeedFactor(ratio)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                |
  | ----- | ---- | ----------------- |
  | ratio | int  | 运动速度比例，取值范围：1~100 |

- 返回：

  ErrorID,{},SpeedFactor(ratio);

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

- 返回：

  ErrorID,{},User(index);

  若ErrorID返回-1，表示设置的用户坐标索引索引不存在；

- 示例：

  User(1)

## 3.7 Tool

- 功能：选择已标定的工具坐标系。 

- 格式：Tool(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                   |
  | ----- | ---- | -------------------- |
  | index | int  | 选择已标定的工具坐标系，取值范围：0~9 |

- 返回：

  ErrorID,{},Tool(index);

  若ErrorID返回-1，表示设置的工具坐标索引不存在；

- 示例：

  Tool(1)

## 3.8 **RobotMode**

- 功能：机器人状态。 

- 格式：RobotMode()

- 参数数量：0

- 支持端口：29999

- 返回值：                             

  | 模式   | 描述                    | 备注         |
  | ---- | --------------------- | ---------- |
  | 1    | ROBOT_MODE_INIT       | 初始化        |
  | 2    | ROBOT_MODE_BRAKE_OPEN | 抱闸松开       |
  | 3    |                       | 保留位        |
  | 4    | ROBOT_MODE_DISABLED   | 未使能(抱闸未松开) |
  | 5    | ROBOT_MODE_ENABLE     | 使能(空闲)     |
  | 6    | ROBOT_MODE_BACKDRIVE  | 拖拽         |
  | 7    | ROBOT_MODE_RUNNING    | 运行状态       |
  | 8    | ROBOT_MODE_RECORDING  | 拖拽录制       |
  | 9    | ROBOT_MODE_ERROR      | 报警         |
  | 10   | ROBOT_MODE_PAUSE      | 暂停状态       |
  | 11   | ROBOT_MODE_JOG        | 点动         |


- 返回：

  ErrorID,{Value},RobotMode();    //Value为返回模式值

- 示例：

  RobotMode()

- 说明：为保持与控制器3.5.1版本兼容性，之前关键机器人状态返回值没有做修改；如：空闲、拖拽、运行、报警状态；新增抱闸松开、轨迹录制、暂停以及点动等；

- 其中运行状态包含：轨迹复现/拟合中、机器人运行状态以及脚本运行状态；

## 3.9 PayLoad

- 功能：设置当前的负载

- 格式：PayLoad(weight,inertia)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名     | 类型     | 含义                     |
  | ------- | ------ | ---------------------- |
  | weight  | double | 负载重量 kg，取值范围：0~机器额定负载； |
  | inertia | double | 负载惯量 kgm²              |

- 返回：

  ErrorID,{},PayLoad(weight,inertia);   

- 示例：

  PayLoad(3,0.4)
  
  说明：为了兼容Lua的LoadSet，tcp指令支持LoadSet，使用LoadSet等同于调用PayLoad，另外要和LoadSwitch指令一起使用

## 3.10 DO

- 功能：设置数字输出端口状态（队列指令） 

- 格式：DO(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                        |
  | ------ | ---- | ------------------------- |
  | index  | int  | 数字输出索引，取值范围：1~16或100~1000 |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平      |

- 返回：

  ErrorID,{},DO(index,status);   

- 示例：

  DO(1,1)

- 说明：使用取值范围100-1000需要有拓展IO模块的硬件支持；

## 3.11 DOExecute

- 功能：设置数字输出端口状态（立即指令）

- 格式：DOExecute(index,status)

- 参数数量：2

- 支持端口：29999



- 参数详解：2

  | 参数名    | 类型   | 含义                        |
  | ------ | ---- | ------------------------- |
  | index  | int  | 数字输出索引，取值范围：1~16或100~1000 |
  | status | int  | 数字输出端口状态，1：高电平；0：低电平      |

- 返回：

  ErrorID,{},DOExecute(index,status);   

- 示例：

  DOExecute(1,1)

- 说明：使用取值范围100-1000需要有拓展IO模块的硬件支持；

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

- 返回：

  ErrorID,{},ToolDO(index,status);  

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

- 返回：

  ErrorID,{},ToolDOExecute(index,status);  

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

- 返回：

  ErrorID,{},AO(index,value);  

- 示例：

  AO(1,2)

- 说明：暂时不支持电流；

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

- 返回：

  ErrorID,{},AOExecute(index,value);

- 示例：

  AOExecute(1,2)

- 说明：暂时不支持电流；


## 3.16 AccJ

- 功能：设置关节加速度比例。该指令仅对MovJ、MovJIO、MovJR、 JointMovJ指令有效

- 格式：AccJ(R)

- 参数数量：1

- 支持端口：29999



- 参数详解：1

  | 参数名  | 类型   | 含义                  |
  | ---- | ---- | ------------------- |
  | R    | int  | 关节加速度百分比，取值范围：1~100 |

- 返回：

  ErrorID,{},AccJ(R);

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

- 返回：

  ErrorID,{},AccL(R);

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

- 返回：

  ErrorID,{},SpeedJ(R);

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

- 返回：

  ErrorID,{},SpeedL(R);

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

- 返回：

  ErrorID,{},Arch(Index);

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

- 返回：

  ErrorID,{},CP(R);

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

- 返回：

  ErrorID,{},LimZ(zValue);

- 示例：

  LimZ(80)

- 说明：**此条指令已经废弃，不建议使用；**

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

- 返回：

  ErrorID,{},LimZ(zValue);

- 示例：

  SetArmOrientation(1,1,-1,1)

## 3.24 PowerOn

- 功能：机器人上电。 

- 格式：PowerOn()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},PowerOn();

- 示例：

  PowerOn()

- **说明：机器人上电到完成，需要等待大概10秒钟的时间再进行使能操作；**


## 3.25 RunScript

- 功能：运行脚本。 

- 格式：RunScript(projectName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名         | 类型     | 含义   |
  | ----------- | ------ | ---- |
  | projectName | string | 脚本名称 |


- 返回：

  ErrorID,{},RunScript(projectName);

- 示例：

  RunScript(demo)

## 3.26 StopScript

- 功能：停止脚本。 

- 格式：StopScript()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},StopScript();



- 示例：

  StopScript()

## 3.27 PauseScript

- 功能：暂停脚本。 

- 格式：PauseScript()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},PauseScript();



- 示例：

  PauseScript()

## 3.28 ContinueScript

- 功能：继续脚本。 

- 格式：ContinueScript()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},ContinueScript();



- 示例：

  ContinueScript()


## 3.29 SetSafeSkin

- 功能：设置安全皮肤开关状态。 

- 格式：SetSafeSkin(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名    | 类型   | 含义                                |
  | ------ | ---- | --------------------------------- |
  | status | int  | status：电子皮肤开关状态，0：关闭电子皮肤；1：开启电子皮肤 |


- 返回：

  ErrorID,{},SetSafeSkin(status));

- 示例：开启电子皮肤功能

  SetSafeSkin (1)

## 3.30 SetObstacleAvoid

- 功能：设置安全皮肤避障模式开关状态。 

- 格式：SetObstacleAvoid(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名    | 类型   | 含义                                       |
  | ------ | ---- | ---------------------------------------- |
  | status | int  | status：安全皮肤避障模式开关状态，0：关闭安全皮肤避障模式；1：开启安全皮肤避障功能 |


- 返回：

  ErrorID,{},SetObstacleAvoid(status);

- 示例：开启安全皮肤避障功能

  SetObstacleAvoid(1)

- 说明：**本条指令暂时废弃，不建议使用；**

## 3.31 GetTraceStartPose

- 功能：获取轨迹拟合中首个点位。 

- 格式：GetTraceStartPose(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |


- 返回：

  ErrorID,{x,y,z,a,b,c},GetTraceStartPose(traceName);   //{x,y,z,a,b,c}指点位坐标值  

- 示例：

  GetTraceStartPose(recv_string)

- 说明：**本条指令在控制器3.5.2版本以及以上支持；**

## 3.32 GetPathStartPose

- 功能：获取轨迹复现中首个点位。 

- 格式：GetPathStartPose(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |


- 返回：

  ErrorID,{j1,j2,j3,j4,j5,j6},GetTraceStartPose(traceName);   //{j1,j2,j3,j4,j5,j6}关节点位坐标值

- 示例：

  GetPathStartPose(recv_string)

- 说明：**本条指令在控制器3.5.2版本以及以上支持；**

## 3.33 PositiveSolution

- 功能：正解。（给定机器人各关节的角度，计算出机器人末端的空间位置）

- 格式：PositiveSolution(J1,J2,J3,J4,J5,J6,User,Tool)

- 参数数量：8

- 支持端口：29999

- 参数详解：8

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | J1   | double | J1 轴位置，单位：度 |
  | J2   | double | J2 轴位置，单位：度 |
  | J3   | double | J3 轴位置，单位：度 |
  | J4   | double | J4 轴位置，单位：度 |
  | J5   | double | J5 轴位置，单位：度 |
  | J6   | double | J6 轴位置，单位：度 |
  | User | int    | 选择已标定的用户坐标系 |
  | Tool | int    | 选择已标定的工具坐标系 |

- 返回：

  ErrorID,{x,y,z,a,b,c},PositiveSolution(J1,J2,J3,J4,J5,J6,User,Tool);    //{x,y,z,a,b,c}指返回的空间位置  

- 示例：下发关节角度返回当前的机器人末端的空间位置

  PositiveSolution(0,0,-90,0,90,0,1,1)

  返回：

  0,{473.000000,-141.000000,469.000000,-180.000000,-0.000000,-90.000000},PositiveSolution(0,0,-90,0,90,0,0,0);

- 说明，需要已知：

  - 机器人的臂方向SetArmOrientation

  

## 3.34 InverseSolution

- 功能：逆解。（已知机器人末端的位置和姿态，计算机器人各关节的角度值）

- 格式：InverseSolution(X,Y,Z,Rx,Ry,Rz,User,Tool,isJointNear,JointNear)

  //其中isJointNear以及JointNear为可选设置参数；

- 参数数量：10

- 支持端口：29999

- 必选参数详解：8

  | 参数名  | 含义          | 类型     |
  | ---- | ----------- | ------ |
  | X    | X 轴位置，单位：毫米 | double |
  | Y    | Y 轴位置，单位：毫米 | double |
  | Z    | Z 轴位置，单位：毫米 | double |
  | Rx   | Rx 轴位置，单位：度 | double |
  | Ry   | Ry轴位置，单位：度  | double |
  | Rz   | Rz轴位置，单位：度  | double |
  | User | 选择已标定的用户坐标系 | int    |
  | Tool | 选择已标定的工具坐标系 | int    |


- 可选参数详解：2

  | 参数名         | 含义                                       | 类型     |
  | ----------- | ---------------------------------------- | ------ |
  | isJointNear | 是否角度选解（值为1:JointNear数据有效，<br />值为0:JointNear数据无效，算法根据当前角度进行选解；<br />不填默认值为0） | int    |
  | JointNear   | 选解六个关节角度值                                | string |

- 返回：

  ErrorID,{J1,J2,J3,J4,J5,J6},InverseSolution(X,Y,Z,Rx,Ry,Rz,User,Tool,isJointNear,JointNear);  //{J1,J2,J3,J4,J5,J6}指返回的关节值;isJointNear,JointNear若有下发则返回，没有下发则不返回；

- 示例：

  下发不带选解关节角度的笛卡尔坐标值返回机器人关节角度值：

  InverseSolution(473.000000,-141.000000,469.000000,-180.000000,0.000,-90.000,0,0)

  返回：0,{0,0,-90,0,90,0},InverseSolution(473.000000,-141.000000,469.000000,

  -180.000000,0.000,-90.000,0,0);

  下发带选解关节角度的笛卡尔坐标值返回机器人关节角度值：

  InverseSolution(473.000000,-141.000000,469.000000,-180.000000,0.000,-90.000,0,0,1,{0,0,-90,0,90,0})
  
  返回：0,{0,0,-90,0,90,0},InverseSolution(0,-247,1050,-90,0,180,0,0,1,{0,0,-90,0,90,0});


## 3.35 SetCollisionLevel

- 功能：设置碰撞等级。 

- 格式：SetCollisionLevel(level)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型   | 含义                                       |
  | ----- | ---- | ---------------------------------------- |
  | level | int  | level: 碰撞等级<br /> 0：关闭碰撞检测<br /> 1~5：等级越高越灵敏 |


- 返回：

  ErrorID,{},SetCollisionLevel(level);   

- 示例：

  SetCollisionLevel(1)

## 3.36 HandleTrajPoints

- 功能：轨迹文件的预处理。

- 格式：HandleTrajPoints(traceName)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |

- 返回：

  ErrorID,{},HandleTrajPoints(traceName);   

- 说明：由于轨迹预处理计算结果会根据文件的大小不同算法处理时间会有差异，**若用户下发不带参数的该指令，代表查询当前指令的结果**；返回：返回为-3表示文件内容错误，返回为-2表示文件不存在，返回为-1表示预处理没有完成；返回为0表示预处理完成，没有错误；返回大于0的结果表示当前返回结果对应的点位有问题；

- 示例：下发轨迹名为recv_string做预处理，然后在一定周期查询预处理结果；

  HandleTrajPoints(recv_string)

  HandleTrajPoints()

- 说明：**本条指令在控制器3.5.2版本以及以上支持；**

## 3.37 GetSixForceData

- 功能：获取六维力数据

- 格式：GetSixForceData()

- 参数数量：0

- 支持端口：29999

- 返回：

   ErrorID,{Fx,Fy,Fz,Mx,My,Mz},GetSixForceData();     //{Fx,Fy,Fz,Mx,My,Mz}表示当前六维力数据原始值；

- 示例：

   GetSixForceData()

  返回：0,{0.0,0.0,0.0,0.0,0.0,0.0},GetSixForceData();

## 3.38 GetAngle

- 功能：获取关节坐标系下机械臂的实时位姿

- 格式：GetAngle()

- 参数数量：0

- 支持端口：29999

- 返回：

   ErrorID,{J1,J2,J3,J4,J5,J6},GetAngle();     //{J1,J2,J3,J4,J5,J6}表示当前位置的关节坐标值；

- 示例：

   GetAngle()

   返回：0,{0.0,0.0,90.0,0.0,-90.0,0.0},GetAngle(); 

## 3.39 GetPose

- 功能：获取笛卡尔坐标系下机械臂的实时位姿(如果设置了用户坐标系或工具坐标系，则获取的位姿为当前坐标系下的位姿)

- 格式：GetPose()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{X,Y,Z,Rx,Ry,Rz},GetPose();     //{X,Y,Z,Rx,Ry,Rz}表示当前位置的笛卡尔坐标值；

- 示例：

  GetPose()

  返回：0,{-473.0,-141.0,469.0,-180.0,0.0,90.0},GetPose();

## 3.40 EmergencyStop

- 功能：急停

- 格式：EmergencyStop()

- 参数数量：0

- 支持端口：29999

- 返回：

   ErrorID,{},EmergencyStop();   

- 示例：

   EmergencyStop()


## 3.41 ModbusCreate

- 功能：创建modbus主站。 

- 格式：ModbusCreate(ip,port,slave_id,isRTU)

- 参数数量：4

- 支持端口：29999

- 参数详解：

  | 参数名      | 类型     | 含义                                       |
  | -------- | ------ | ---------------------------------------- |
  | ip       | string | 从站ip地址；                                  |
  | port     | int    | 从站端口;                                    |
  | slave_id | int    | 从站ID（取值范围大于0的整数）                         |
  | isRTU    | int    | **可选参数**，取值范围0/1：<br />       如果为空或者值为0，建立modbusTCP通信；<br />       如果为1，建立modbusRTU通信；<br /> |


- 返回：

  ErrorID,{index},ModbusCreate(ip,port,slave_id,isRTU);      //ErrorID:0表示创建Modbus主站成功，-1表示创建Modbus主站失败，其他值参考错误码描述；index: 返回的主站索引，最多支持5个设备,取值范围(0~4)；

- 示例：建立RTU通信主站(60000末端透传端口)

  ModbusCreate(127.0.0.1,60000,1,true)

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.42 ModbusClose

- 功能：和Modbus从站断开连接,释放主站。 

- 格式：ModbusClose(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型     | 含义       |
  | ----- | ------ | -------- |
  | index | string | 返回的主站索引； |


- 返回：

  ErrorID,{},ModbusClose(index);     

- 示例：

  ModbusClose(0)

- 说明：**控制器3.5.2版本以及以上版本支持；**

  

## 3.43 GetInBits

- 功能：读离散输入功能。 

- 格式：GetInBits(index,addr,count)

- 参数数量：3

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型   | 含义          |
  | ----- | ---- | ----------- |
  | index | int  | 返回的主站索引；    |
  | addr  | int  | 视从站配置而定；    |
  | count | int  | 个数，取值范围1~16 |


- 返回：

  ErrorID,{value1,value2,...,valuen},GetInBits(index,addr,count);    //table，按位获取结果{value1,value2...,valuen}

- 示例：

   下发：  GetInBits(0,3000,5)

  正常返回：0,{1,0,1,1,0},GetInBits(0,3000,5);

  错误返回：-1,{},GetInBits(0,3000,5);

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.44 GetInRegs

- 功能：读输入寄存器。 

- 格式：GetInRegs(index,addr,count,valType)

- 参数数量：4

- 支持端口：29999

- 参数详解：

  | 参数名     | 类型     | 含义                                       |
  | ------- | ------ | ---------------------------------------- |
  | index   | int    | 返回的主站索引；                                 |
  | addr    | int    | 视从站配置而定；                                 |
  | count   | int    | 个数，取值范围：1 -4                             |
  | valType | string | 可选参数，<br />U16: 读取16位无符号数（2个字节，占用1个寄存器）<br />U32: 读取32位无符号数（4个字节，占用2个寄存器）<br />F32: 读取32位浮点数（4个字节，占用2个寄存器）<br />F64: 读取64位浮点数（8个字节，占用4个寄存器） |


- 返回：

  ErrorID,{value1,value2,...,valuen},GetInRegs(index,addr,count,valType);    //ErrorID为0表示正常，为-1表示没有获取成功;table，按变量类型返回{value1,value2...,valuen}

- 示例：

  GetInRegs(0,4000,3)

  正常返回：0,{5,18,12},GetInRegs(0,4000,3);

  错误返回：-1,{},GetInRegs(0,4000,3);

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.45 GetCoils

- 功能：读线圈功能。 

- 格式：GetCoils(index,addr,count)

- 参数数量：3

- 支持端口：29999

- 参数详解：

  | 参数名   | 类型   | 含义          |
  | ----- | ---- | ----------- |
  | index | int  | 返回的主站索引；    |
  | addr  | int  | 视从站配置而定；    |
  | count | int  | 个数，取值范围1~16 |


- 返回：

  ErrorID,{value1,value2,...,valuen},GetCoils(index,addr,count);    //ErrorID为0表示正常，为-1表示没有获取成功;table，按变量类型返回{value1,value2...,valuen}

- 示例：

  GetCoils(0,1000,3)

  正常返回：0,{1,1,0},GetCoils(0,1000,3);

  错误返回：-1,{},GetCoils(0,1000,3);

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.46 SetCoils

- 功能：写线圈功能。 

- 格式：SetCoils(index,addr,count,valTab)

- 参数数量：4

- 支持端口：29999

- 参数详解：

  | 参数名    | 类型     | 含义          |
  | ------ | ------ | ----------- |
  | index  | int    | 返回的主站索引；    |
  | addr   | int    | 视从站配置而定；    |
  | count  | int    | 个数，取值范围1~16 |
  | valTab | string | 写线圈地址值；     |


- 返回：

  ErrorID,{},SetCoils(index,addr,count,valTab);    //ErrorID为0表示正常，为-1表示没有设置成功;

- 示例：

  SetCoils(0,1000,3,{1,0,1})

  正常返回：0,{},SetCoils(0,1000,3,{1,0,1});

  错误返回：-1,{},SetCoils(0,1000,3,{1,0,1});

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.47 GetHoldRegs

- 功能：读保持寄存器。 

- 格式：GetHoldRegs(index,*addr*, *count*,valType)

- 参数数量：4

- 支持端口：29999

- 参数详解：

  | 参数名     | 类型     | 含义                                       |
  | ------- | ------ | ---------------------------------------- |
  | index   | int    | index,返回的主站索引，最多支持5个设备,取值范围(0~4)；        |
  | addr    | int    | 保持寄存器的起始地址。视从站配置而定；                      |
  | count   | int    | 读取指定数量type类型的数据。取值范围：1 -4                |
  | valType | string | 数据类型：<br /> 如果为空，默认读取16位无符号整数（2个字节，占用1个寄存器）<br /> U16：读取16位无符号整数（2个字节，占用1个寄存器）<br /> U32：读取32位无符号整数（4个字节，占用2个寄存器）<br /> F32：读取32位单精度浮点数（4个字节，占用2个寄存器）<br /> F64：读取64位双精度浮点数（8个字节，占用4个寄存器） |


- 返回：

  ErrorID,{value1,value2,...,valuen},GetHoldRegs(index,*addr*, *count*,valType);    //ErrorID为0表示正常，为-1表示没有获取成功;table，按变量类型返回{value1,value2...,valuen}

- 示例：从地址3095开始读取一个16位无符号整数

   GetHoldRegs(0,3095,1)

     正常返回：0,{13},GetHoldRegs(0,3095,1);

     错误返回：-1,{},GetHoldRegs(0,3095,1);

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.48 SetHoldRegs

- 功能：写保存寄存器。 

- 格式：SetHoldRegs(index,*addr*, *count*,*valTab*,valType)

- 参数数量：5

- 支持端口：29999

- 参数详解：

  | 参数名     | 类型     | 含义                                       |
  | ------- | ------ | ---------------------------------------- |
  | index   | int    | index,返回的主站索引，最多支持5个设备,取值范围(0~4)         |
  | addr    | int    | 保持寄存器的起始地址。视从站配置而定；                      |
  | count   | int    | 写入指定数量type类型的数据。取值范围：1 ~4                |
  | valTab  | string | 保持寄存器地址的值                                |
  | valType | string | 数据类型     <br /> 如果为空，默认读取16位无符号整数（2个字节，占用1个寄存器） <br /> U16：读取16位无符号整数（2个字节，占用1个寄存器）  <br />U32：读取32位无符号整数（4个字节，占用2个寄存器）   <br />F32：读取32位单精度浮点数（4个字节，占用2个寄存器）    <br /> F64：读取64位双精度浮点数（8个字节，占用4个寄存器） |


- 返回：

  ErrorID,{},SetHoldRegs(index,*addr*, *count*,*valTab*,valType);    //ErrorID为0表示正常，为-1表示没有设置成功;

- 示例：从地址3095开始，写入两个16位无符号整数值6000，300

  SetHoldRegs(0,3095,2,{6000,300}, U16)

  正常返回：0,{},SetHoldRegs(0,3095,2,{6000,300}, U16);

  错误返回：-1,{},SetHoldRegs(0,3095,2,{6000,300}, U16);

- 说明：**控制器3.5.2版本以及以上版本支持；**

## 3.49 GetErrorID

- 功能：获取机器人错误码
- 格式：GetErrorID()
- 参数数量：0
- 支持端口：29999


- 返回：

  ErrorID,{[[id,...,id], [id], [id], [id], [id], [id], [id]]},GetErrorID();    //[id, ..., id]为控制器以及算法报警信息，其中碰撞检测值为-2,电子皮肤碰撞检测值-3；后面六个[id]分别表示六个伺服的报警信息；

- 示例：

  GetErrorID()

  返回：

   0,{[[-2],[],[],[],[],[]]},GetErrorId();

- 说明：对于错误码对应的错误内容请参考控制器错误描述文件alarm_controller.json以及伺服错误描述alarm_servo.json；

- **控制器3.5.2版本以及以上版本支持；**


## 3.50 DI

- 功能：获取数字量输入端口状态

- 格式：DI(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                        |
  | ----- | ---- | ------------------------- |
  | index | int  | 数字输入索引，取值范围：1~32或100~1000 |


- 返回：

  ErrorID,{value},DI(index);    //value:返回当前index的值状态，取值范围0/1;

- 示例：

  DI(1)

  返回，数字输入端口1为低电平：

  0,{0},DI(1);

- 说明：使用取值范围100-1000需要有拓展IO模块的硬件支持；

## 3.51 ToolDI

- 功能：获取末端数字量输入端口状态

- 格式：ToolDI(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                 |
  | ----- | ---- | ------------------ |
  | index | int  | 末端数字量输入索引，取值范围：1/2 |


- 返回：

  ErrorID,{value},ToolDI(index);    //value:返回当前index的值状态，取值范围0/1;

- 示例：

  ToolDI(2)

  返回，末端数字输入端口2为高电平：

  0,{1},ToolDI(2);

## 3.52 AI

- 功能：获取模拟量输入端口电压值（立即指令）

- 格式：AI(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                 |
  | ----- | ---- | ------------------ |
  | index | int  | 控制柜模拟输入索引，取值范围：1/2 |


- 返回：

  ErrorID,{value},AI(index);    //value:返回当前index的电压值;

- 示例：

  AI(2)

  返回，模拟输入端口2的电压值为3.5V：

  0,{3.5},AI(2);

## 3.53 ToolAI

- 功能：获取末端模拟量输入端口电压值（立即指令）

- 格式：ToolAI(index)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名   | 类型   | 含义                |
  | ----- | ---- | ----------------- |
  | index | int  | 末端模拟输入索引，取值范围：1/2 |


- 返回：

  ErrorID,{value},ToolAI(index);    //value:返回当前index的电压值;

- 示例：

  ToolAI(1)

  返回，模拟输入端口1的电压值为1.5V：

  0,{1.5},ToolAI(1);

## 3.54 DIGroup

- 功能：获取输入组端口状态

- 格式：DIGroup(index1,index2,...,indexn)

- 参数数量：不固定(最大支持64个)

- 支持端口：29999

- 参数详解：不固定

  | 参数名    | 类型   | 含义                        |
  | ------ | ---- | ------------------------- |
  | index1 | int  | 数字输入索引，取值范围：1-32或100~1000 |
  | ...    | ...  | ...                       |
  | indexn | int  | 数字输入索引，取值范围：1-32或100~1000 |


- 返回：

  ErrorID,{value1,value2,...,valuen},DIGroup(index1,index2,...,indexn);    //value1...valuen:返回当前index1到indexn的电压值;

- 示例：

  DIGroup(4,6,2,7)

  返回，获取的输入4、6、2、7端口的电平分别为1，0，1，1：

  0,{1,0,1,1},DIGroup(4,6,2,7);

- 说明：使用取值范围100-1000需要有拓展IO模块的硬件支持；

## 3.55 DOGroup

- 功能：设置输出组端口状态

- 格式：DOGroup(index1,value1,index2,value2,...,indexn,valuen)

- 参数数量：不固定(最大支持64个)

- 支持端口：29999

- 参数详解：不固定

  | 参数名    | 类型   | 含义                          |
  | ------ | ---- | --------------------------- |
  | index1 | int  | 设置数字输出索引，取值范围：1-16或100~1000 |
  | value1 | int  | 设置数字输出端口状态，取值0/1            |
  | ...    | ...  | ...                         |
  | indexn | int  | 设置数字输出索引，取值范围：1-16或100~1000 |
  | valuen | int  | 设置数字输出端口状态，取值0/1            |


- 返回： 

  ErrorID,{},DOGroup(index1,value1,index2,value2,...,indexn,valuen);   

- 示例：

  DOGroup(4,1,6,0,2,1,7,0)

  返回，分别设置输出端口4、6、2、7分别为1、0、1、0：

  0,{},DOGroup(4,1,6,0,2,1,7,0);

- 说明：使用取值范围100-1000需要有拓展IO模块的硬件支持；


## 3.56 BrakeControl

- 功能：开关抱闸

- 格式：BrakeControl(axisID,value)     

- 必填参数数量：2

- 注意：**抱闸的控制需要机器人在下使能的条件下进行；**否则机器人错误返回-1；

- 支持端口：29999

- 必填参数详解：2

  | 参数名    | 类型   | 含义                          |
  | ------ | ---- | --------------------------- |
  | axisID | int  | 关节轴号                        |
  | value  | int  | 设置抱闸状态；取值0/1  0:关闭抱闸 1：打开抱闸 |

- 返回：

  ErrorID,{},BrakeControl(axisID,value);  

- 示例：打开关节1抱闸

  BrakeControl(1,1)

  返回：

  0,{},BrakeControl(1,1);

  说明：**控制器3.5.2版本以及以上版本支持此命令；**

## 3.57 StartDrag

- 功能：进入拖拽(在报错状态下，不可进入拖拽)

- 格式：StartDrag()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},StartDrag();   

- 示例：

  StartDrag()

  说明：**只在特定版本支持此命令**

## 3.58 StopDrag

- 功能：退出拖拽

- 格式：StopDrag()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{},StopDrag();   

- 示例：

  StopDrag()

  说明：**只在特定版本支持此命令**

## 3.59 SetCollideDrag

- 功能：设置是否强制进入拖拽（报错状态下也能进入拖拽）

- 格式：SetCollideDrag(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名 | 类型 | 含义                                       |
  | ------ | ---- | ------------------------------------------ |
  | status | int  | status：强制拖拽开关状态，0：关闭；1：开启 |


- 返回：

  ErrorID,{},SetCollideDrag(status);    

- 示例：强制进入拖拽

  SetCollideDrag(0)

  说明：**只在特定版本支持此命令**

## 3.60 SetTerminalKeys

- 功能：设置末端按键功能使能状态

- 格式：SetTerminalKeys(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名 | 类型 | 含义                                               |
  | ------ | ---- | -------------------------------------------------- |
  | status | int  | status：设置末端按键功能使能状态，0：关闭；1：开启 |


- 返回：

  ErrorID,{},SetTerminalKeys(status);    

- 示例：禁用末端按键功能

  SetTerminalKeys(0)

  说明：**只在特定版本支持此命令**

## 3.61 SetTerminal485

- 功能：设置末端485参数

- 格式：SetTerminal485(baudRate, dataLen, parityBit, stopBit)

- 参数数量：4

- 支持端口：29999

- 参数详解：4

  | 参数名    | 类型   | 含义                                           |
  | --------- | ------ | ---------------------------------------------- |
  | baudRate  | int    | baudRate：波特率                               |
  | dataLen   | int    | dataLen：数据位长度，目前固定为8               |
  | parityBit | string | parityBit：奇偶校验位，目前固定为N，代表无校验 |
  | stopBit   | int    | stopBit: 停止位，目前固定为1                   |


- 返回：

  ErrorID,{},SetTerminal485(status);    

- 示例：设置波特率为115200

  SetTerminal485(115200, 8, N, 1)

  说明：**只在特定版本支持此命令**

## 3.62 GetTerminal485

- 功能：获取末端485的参数

- 格式：GetTerminal485()

- 参数数量：0

- 支持端口：29999

- 返回：

  ErrorID,{baudRate, dataLen, parityBit, stopBit},GetPose();     //{baudRate, dataLen, parityBit, stopBit}分别表示波特率，数据位，奇偶校验位，停止位

- 示例：

  GetTerminal485()

  返回：0,{115200, 8, N, 1},GetTerminal485();

  说明：**只在特定版本支持此命令**

## 3.63 LoadSwitch

- 功能：控制负载设置状态

- 格式：LoadSwitch(status)

- 参数数量：1

- 支持端口：29999

- 参数详解：1

  | 参数名 | 类型 | 含义                                                         |
  | ------ | ---- | ------------------------------------------------------------ |
  | status | int  | status：设置负载设置状态，0：关闭；1：开启，开启负载设置会提高碰撞灵敏度 |

- 返回：

  ErrorID,{},LoadSwitch(status);    

- 示例：

  LoadSwitch(1)   // 开启负载设置

  



# 4.通信协议----实时反馈端口

​	30004端口即实时反馈端口(30004、30005以及30006端口将在控制器3.5.2+版本支持)，客户端**每8ms**能收到一次机器人如下表所示的信息。30005服务器端口**每200ms反馈机器人的信息**，30006端口为**可配置**的反馈机器人信息端口(默认为每**50ms**反馈)；通过实时反馈端口每次收到的数据包有1440个字节，这些字节以标准的格式排列。下表是字节排列的顺序表。(30006端口的实时数据的配置更新可以在线修改后，实时生效；）

|      意义/Meaning      |   数据类型/Type    | 值的数目/Number of values | 字节大小/Size in bytes | 字节位置值/Byte position value |                 描述/Notes                 |
| :------------------: | :------------: | :-------------------: | :----------------: | :-----------------------: | :--------------------------------------: |
|     MessageSize      | unsigned short |           1           |         2          |        0000 ~ 0001        |  消息字节总长度/Total message length in bytes   |
|                      | unsigned short |           3           |         6          |        0002 ~ 0007        |                   保留位                    |
|    DigitalInputs     |     uint64     |           1           |         8          |        0008 ~ 0015        | 数字输入/Current state of the digital inputs. |
|    DigitalOutputs    |     uint64     |           1           |         8          |        0016 ~ 0023        |                   数字输出                   |
|      RobotMode       |     uint64     |           1           |         8          |        0024 ~ 0031        |             机器人模式/Robot mode             |
|      TimeStamp       |     uint64     |           1           |         8          |        0032 ~ 0039        |                时间戳（单位ms）                 |
|                      |     uint64     |           1           |         8          |        0040 ~ 0047        |                   保留位                    |
|      TestValue       |     uint64     |           1           |         8          |        0048 ~ 0055        |     内存结构测试标准值  0x0123 4567 89AB CDEF     |
|                      |     double     |           1           |         8          |        0056 ~ 0063        |                   保留位                    |
|     SpeedScaling     |     double     |           1           |         8          |        0064 ~ 0071        | 速度比例/Speed scaling of the trajectory limiter |
|  LinearMomentumNorm  |     double     |           1           |         8          |        0072 ~ 0079        | 机器人当前动量/Norm of Cartesian linear momentum(需要特定硬件版本) |
|        VMain         |     double     |           1           |         8          |        0080 ~ 0087        |     控制板电压/Masterboard: Main voltage      |
|        VRobot        |     double     |           1           |         8          |        0088 ~ 0095        |  机器人电压/Masterboard: Robot voltage (48V)  |
|        IRobot        |     double     |           1           |         8          |        0096 ~ 0103        |     机器人电流/Masterboard: Robot current     |
|                      |     double     |           1           |         8          |        0104 ~ 0111        |                   保留位                    |
|                      |     double     |           1           |         8          |        0112 ~ 0119        |                   保留位                    |
|  ToolAcceleroMeter   |     double     |           3           |         24         |        0120 ~ 0143        | TCP加速度/Tool x,y and z accelerometer values(需要特定硬件版本) |
|    ElbowPosition     |     double     |           3           |         24         |        0144 ~ 0167        |       肘位置/Elbow position(需要特定硬件版本)       |
|    ElbowVelocity     |     double     |           3           |         24         |        0168 ~ 0191        |       肘速度/Elbow velocity(需要特定硬件版本)       |
|       QTarget        |     double     |           6           |         48         |        0192 ~ 0239        |      目标关节位置/Target joint positions       |
|       QDTarget       |     double     |           6           |         48         |        0240 ~ 0287        |      目标关节速度/Target joint velocities      |
|      QDDTarget       |     double     |           6           |         48         |        0288 ~ 0335        |    目标关节加速度/Target joint accelerations    |
|       ITarget        |     double     |           6           |         48         |        0336 ~ 0383        |       目标关节电流/Target joint currents       |
|       MTarget        |     double     |           6           |         48         |        0384 ~ 0431        |  目标关节扭矩/Target joint moments (torques)   |
|       QActual        |     double     |           6           |         48         |        0432 ~ 0479        |      实际关节位置/Actual joint positions       |
|       QDActual       |     double     |           6           |         48         |        0480 ~ 0527        |      实际关节速度/Actual joint velocities      |
|       IActual        |     double     |           6           |         48         |        0528 ~ 0575        |       实际关节电流/Actual joint currents       |
|    ActualTCPForce    |     double     |           6           |         48         |        0576 ~ 0623        |            TCP传感器力值(通过六维力计算)             |
|   ToolVectorActual   |     double     |           6           |         48         |        0624 ~ 0671        | TCP笛卡尔实际坐标值/Actual Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation |
|    TCPSpeedActual    |     double     |           6           |         48         |        0672 ~ 0719        | TCP笛卡尔实际速度值/Actual speed of the tool given in Cartesian coordinates |
|       TCPForce       |     double     |           6           |         48         |        0720 ~ 0767        |             TCP力值（通过关节电流计算）              |
|   ToolVectorTarget   |     double     |           6           |         48         |        0768 ~ 0815        | TCP笛卡尔目标坐标值/Target Cartesian coordinates of the tool: (x,y,z,rx,ry,rz), where rx, ry and rz is a rotation vector representation of the tool orientation |
|    TCPSpeedTarget    |     double     |           6           |         48         |        0816 ~ 0863        | TCP笛卡尔目标速度值/Target speed of the tool given in Cartesian coordinates |
|  MotorTemperatures   |     double     |           6           |         48         |        0864 ~ 0911        | 关节温度/Temperature of each joint in degrees celsius |
|      JointModes      |     double     |           6           |         48         |        0912 ~ 0959        |        关节控制模式/Joint control modes        |
|       VActual        |     double     |           6           |         48         |        960  ~ 1007        |        关节电压/Actual joint voltages        |
|       HandType       |      char      |           4           |         4          |        1008 ~ 1011        |                    手系                    |
|         User         |      char      |           1           |         1          |           1012            |                   用户坐标                   |
|         Tool         |      char      |           1           |         1          |           1013            |                   工具坐标                   |
|     RunQueuedCmd     |      char      |           1           |         1          |           1014            |                 算法队列运行标志                 |
|     PauseCmdFlag     |      char      |           1           |         1          |           1015            |                 算法队列暂停标志                 |
|    VelocityRatio     |      char      |           1           |         1          |           1016            |              关节速度比例(0~100)               |
|  AccelerationRatio   |      char      |           1           |         1          |           1017            |              关节加速度比例(0~100)              |
|      JerkRatio       |      char      |           1           |         1          |           1018            |             关节加加速度比例(0~100)              |
|   XYZVelocityRatio   |      char      |           1           |         1          |           1019            |             笛卡尔位置速度比例(0~100)             |
|    RVelocityRatio    |      char      |           1           |         1          |           1020            |             笛卡尔姿态速度比例(0~100)             |
| XYZAccelerationRatio |      char      |           1           |         1          |           1021            |            笛卡尔位置加速度比例(0~100)             |
|  RAccelerationRatio  |      char      |           1           |         1          |           1022            |            笛卡尔姿态加速度比例(0~100)             |
|     XYZJerkRatio     |      char      |           1           |         1          |           1023            |            笛卡尔位置加加速度比例(0~100)            |
|      RJerkRatio      |      char      |           1           |         1          |           1024            |            笛卡尔姿态加加速度比例(0~100)            |
|     BrakeStatus      |      char      |           1           |         1          |           1025            |                 机器人抱闸状态                  |
|     EnableStatus     |      char      |           1           |         1          |           1026            |                 机器人使能状态                  |
|      DragStatus      |      char      |           1           |         1          |           1027            |                 机器人拖拽状态                  |
|    RunningStatus     |      char      |           1           |         1          |           1028            |                 机器人运行状态                  |
|     ErrorStatus      |      char      |           1           |         1          |           1029            |                 机器人报警状态                  |
|      JogStatus       |      char      |           1           |         1          |           1030            |                 机器人点动状态                  |
|      RobotType       |      char      |           1           |         1          |           1031            |                   机器类型                   |
|   DragButtonSignal   |      char      |           1           |         1          |           1032            |                 按钮板拖拽信号                  |
|  EnableButtonSignal  |      char      |           1           |         1          |           1033            |                 按钮板使能信号                  |
|  RecordButtonSignal  |      char      |           1           |         1          |           1034            |                 按钮板录制信号                  |
| ReappearButtonSignal |      char      |           1           |         1          |           1035            |                 按钮板复现信号                  |
|   JawButtonSignal    |      char      |           1           |         1          |           1036            |                按钮板夹爪控制信号                 |
|    SixForceOnline    |      char      |           1           |         1          |           1037            |                 六维力在线状态                  |
|     Reserve2[82]     |      char      |           1           |         82         |         1038-1119         |                   保留位                    |
|      MActual[6]      |     double     |           6           |         48         |        1120 ~ 1167        |                   实际扭矩                   |
|         Load         |     double     |           1           |         8          |         1168-1175         |                  负载重量kg                  |
|       CenterX        |     double     |           1           |         8          |         1176-1183         |                X方向偏心距离mm                 |
|       CenterY        |     double     |           1           |         8          |         1184-1191         |                Y方向偏心距离mm                 |
|       CenterZ        |     double     |           1           |         8          |         1192-1199         |                Z方向偏心距离mm                 |
|       User[6]        |     double     |           6           |         48         |         1200-1247         |                  用户坐标值                   |
|       Tool[6]        |     double     |           6           |         48         |         1248-1295         |                  工具坐标值                   |
|      TraceIndex      |     double     |           1           |         8          |         1296-1303         |                 轨迹复现运行索引                 |
|   SixForceValue[6]   |     double     |           6           |         48         |         1304-1351         |                当前六维力数据原始值                |
| TargetQuaternion[4]  |     double     |           4           |         32         |         1352-1383         |           [qw,qx,qy,qz] 目标四元数            |
| ActualQuaternion[4]  |     double     |           4           |         32         |         1384-1415         |           [qw,qx,qy,qz]  实际四元数           |
|     Reserve3[24]     |      char      |           1           |         24         |        1416 ~ 1440        |                   保留位                    |
|        TOTAL         |                |                       |        1440        |                           |             1440byte package             |

其中Robot Mode返回机器人模式：                        

| 模式   | 描述                    | 备注         |
| ---- | --------------------- | ---------- |
| 1    | ROBOT_MODE_INIT       | 初始化        |
| 2    | ROBOT_MODE_BRAKE_OPEN | 抱闸松开       |
| 3    |                       | 保留位        |
| 4    | ROBOT_MODE_DISABLED   | 未使能(抱闸未松开) |
| 5    | ROBOT_MODE_ENABLE     | 使能(空闲)     |
| 6    | ROBOT_MODE_BACKDRIVE  | 拖拽         |
| 7    | ROBOT_MODE_RUNNING    | 运行状态       |
| 8    | ROBOT_MODE_RECORDING  | 拖拽录制       |
| 9    | ROBOT_MODE_ERROR      | 报警         |
| 10   | ROBOT_MODE_PAUSE      | 暂停状态       |
| 11   | ROBOT_MODE_JOG        | 点动         |

- 说明：
  - [ ] 开启抱闸，状态为2；
  - [ ] 本体下使能，状态为4；
  - [ ] 使能成功后，则状态为5 ；
  - [ ] 机器人运行，状态为7；
  - [ ] 机器人暂停，状态为10；
  - [ ] 机器人进入拖拽模式(使能状态)，状态为6；
  - [ ] 机器人在拖拽录制，状态为8；
  - [ ] 机器人在点动，状态为11；
  - [ ] 其中报警优先级最高，其他状态同时存在时，若有报警，先将状态置9；

- 其中BrakeStatus抱闸状态：

  0x01表示第六个轴抱闸打开；

  0x02表示第五个轴抱闸打开；

  0x03表示五六轴抱闸打开；

  0x04表示第四个轴抱闸打开；

  ...

  如下位表示抱闸状态：

  | 7    | 6    | 5    | 4    | 3    | 2    | 1    | 0    |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | 保留位  | 保留位  | 关节一  | 关节二  | 关节三  | 关节四  | 关节五  | 关节六  |

- 其中JointModes关节控制模式：

  ​	当前值为8表示位置模式；

  ​	当前值为10表示力矩模式；

- 其中RobotType表示机器类型：

  | RobotType值 | 代表机型  |
  | ---------- | ----- |
  | 3          | CR3   |
  | 31         | CR3L  |
  | 5          | CR5   |
  | 7          | CR7   |
  | 10         | CR10  |
  | 12         | CR12  |
  | 16         | CR16  |
  | 1          | MG400 |
  | 2          | M1Pro |

  

  



# 5.通信协议----运动相关端口

​	上位机可以通过30003端口直接发送一些机器人运动相关指令给机器人，这些指令CR自己定义的；如下表是30003端口的指令列表。可以通过如下指令实现对机器人的运动相关控制；

| 指令           | 描述                           |
| ------------ | ---------------------------- |
| MovJ         | 点到点运动，目标点位为笛卡尔点位             |
| MovL         | 直线运动，目标点位为笛卡尔点位              |
| JointMovJ    | 点到点运动，目标点位为关节点位              |
| Jump         | 门型运动，仅支持笛卡尔点位                |
| RelMovJ      | 以点到点方式运动至笛卡尔偏移位置             |
| RelMovL      | 以直线运动至笛卡尔偏移位置                |
| MovLIO       | 直线运动过程中并行设置数字输出端口的状态，可设置多组   |
| MovJIO       | 点到点运动过程中并行设置数字输出端口的状态，可设置多组  |
| Arc          | 圆弧运动。需结合其他运动指令完成圆弧运动         |
| Circle       | 整圆运动，需结合其他运动指令完成整圆运动         |
| ServoJ       | 基于关节空间的动态跟随命令                |
| ServoP       | 基于笛卡尔空间的动态跟随命令               |
| MoveJog      | 关节点动                         |
| StartTrace   | 轨迹拟合                         |
| StartPath    | 轨迹复现                         |
| StartFCTrace | 带力控的轨迹拟合                     |
| RelMovJTool  | 沿工具坐标系进行相对运动，末端运动方式为关节运动     |
| RelMovLTool  | 沿工具坐标系进行相对运动指令，末端运动方式为直线运动   |
| RelMovJUser  | 沿用户坐标系进行相对运动指令，末端运动方式为关节运动   |
| RelMovLUser  | 沿用户坐标系进行相对运动指令，末端运动方式为直线运动   |
| RelJointMovJ | 沿各轴关节坐标系进行相对运动指令，末端运动方式为关节运动 |



## 5.1 MovJ

- 功能：点到点运动，目标点位为笛卡尔点位。

- 格式：MovJ(X,Y,Z,Rx,Ry,Rz,User=index,Tool=index,SpeedJ=R,AccJ=R)   

  //其中User=index,Tool=index,SpeedJ=R,AccJ=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、关节速度比例以及加速度比例值； 和29999端口设置的SpeedJ、AccJ意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | X    | double | X 轴位置，单位：毫米 |
  | Y    | double | Y 轴位置，单位：毫米 |
  | Z    | double | Z 轴位置，单位：毫米 |
  | Rx   | double | Rx 轴位置，单位：度 |
  | Ry   | double | Ry 轴位置，单位：度 |
  | Rz   | double | Rz 轴位置，单位：度 |

- 返回：

  ErrorID,{},MovJ(X,Y,Z,Rx,Ry,Rz);   

- 示例：

  MovJ(-500,100,200,150,0,90,AccJ=50)

  返回：

  ErrorID,{},MovJ(-500,100,200,150,0,90,AccJ=50);   



## 5.2 MovL

- 功能：直线运动，目标点位为笛卡尔点位。

- 格式：MovL(X,Y,Z,Rx,Ry,Rz,User=index,Tool=index,SpeedL=R,AccL=R)  

  其中User=index,Tool=index,SpeedL=R,AccL=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、笛卡尔速度比例以及加速度比例值； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | X    | double | X 轴位置，单位：毫米 |
  | Y    | double | Y 轴位置，单位：毫米 |
  | Z    | double | Z 轴位置，单位：毫米 |
  | Rx   | double | Rx 轴位置，单位：度 |
  | Ry   | double | Ry 轴位置，单位：度 |
  | Rz   | double | Rz 轴位置，单位：度 |

- 返回：

  ErrorID,{},MovL(X,Y,Z,Rx,Ry,Rz,SpeedL=R,AccL=R);   

- 示例：

  MovL(-500,100,200,150,0,90,SpeedL=60)

  返回：

  ErrorID,{},MovL(-500,100,200,150,0,90,SpeedL=60);   

## 5.3 JointMovJ

- 功能：点到点运动，目标点位为关节点位。

- 格式：JointMovJ(J1,J2,J3,J4,J5,J6,User=index,Tool=index,SpeedJ=R,AccJ=R)  

  //其中SpeedJ=R,AccJ=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、关节速度比例以及加速度比例值； 和29999端口设置的SpeedJ、AccJ意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名  | 类型     | 含义          |
  | ---- | ------ | ----------- |
  | J1   | double | J1 轴位置，单位：度 |
  | J2   | double | J2 轴位置，单位：度 |
  | J3   | double | J3 轴位置，单位：度 |
  | J4   | double | J4 轴位置，单位：度 |
  | J5   | double | J5 轴位置，单位：度 |
  | J6   | double | J6 轴位置，单位：度 |

- 返回：

  ErrorID,{},JointMovJ(J1,J2,J3,J4,J5,J6,SpeedJ=R,AccJ=R);   

- 示例：

  JointMovJ(0,0,-90,0,90,0,SpeedJ=60,AccJ=50)

  返回：

  ErrorID,{},JointMovJ(0,0,-90,0,90,0,SpeedJ=60,AccJ=50);   

## 4.4 Jump

说明：**本条指令暂时废弃，不建议使用；**

## 4.5 RelMovJ

- 功能：以点到点方式运动至笛卡尔偏移位置。

- 格式：RelMovJ(offsetX,offsetY,offsetZ,offsetRx,offsetRy,offsetRz,User=index,Tool=index,SpeedJ=R,AccJ=R)  

  //其中SpeedJ=R,AccJ=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、关节速度比例以及加速度比例值； 和29999端口设置的SpeedJ、AccJ意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名      | 类型     | 含义           |
  | -------- | ------ | ------------ |
  | offsetX  | double | X轴方向偏移，单位：mm |
  | offsetY  | double | Y轴方向偏移，单位：mm |
  | offsetZ  | double | Z轴方向偏移，单位：mm |
  | offsetRx | double | Rx 轴位置，单位：度  |
  | offsetRy | double | Ry 轴位置，单位：度  |
  | offsetRz | double | Rz 轴位置，单位：度  |

- 返回：

  ErrorID,{},RelMovJ(offsetX,offsetY,offsetZ,offsetRx,offsetRy,offsetRz,SpeedJ=R,AccJ=R);

- 示例：

  RelMovJ(10,10,10,10,10,10)

- 说明：**本条指令暂时废弃，不建议使用；**

## 4.6 RelMovL

- 功能：以直线运动至笛卡尔偏移位置。

- 格式：RelMovL(offsetX,offsetY,offsetZ,User=index,Tool=index,SpeedL=R,AccL=R)  

  //其中SpeedL=R,AccL=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、笛卡尔速度比例以及加速度比例值； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：3

- 支持端口：30003

- 必填参数详解：3

  | 参数名     | 类型     | 含义           |
  | ------- | ------ | ------------ |
  | offsetX | double | X轴方向偏移，单位：mm |
  | offsetY | double | Y轴方向偏移，单位：mm |
  | offsetZ | double | Z轴方向偏移，单位：mm |

- 返回：

  ErrorID,{},RelMovL(offsetX,offsetY,offsetZ,SpeedL=R,AccL=R);

- 示例：

  RelMovL(10,10,10)

- 说明：**本条指令暂时废弃，不建议使用；**

## 4.7 MovLIO

- 功能：在直线运动时并行设置数字输出端口状态，目标点位为笛卡尔点位。

- 格式：MovLIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},User=index,Tool=index,SpeedL=R,AccL=R) 

   //其中SpeedL=R,AccL=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、笛卡尔速度比例以及加速度比例值； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

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

- 返回：

  ErrorID,{},MovLIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},SpeedL=R,AccL=R);

- 示例：

  MovLIO(-500,100,200,150,0,90,{0,50,1,0})

## 4.8 MovJIO

- 功能：点到点运动时并行设置数字输出端口状态，目标点位为笛卡尔点位。

- 格式：MovJIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},User=index,Tool=index,SpeedJ=R,AccJ=R)  

  //其中SpeedJ=R,AccJ=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、关节速度比例以及加速度比例值； 和29999端口设置的SpeedJ、AccJ意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

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

- 返回：

  ErrorID,{},MovJIO(X,Y,Z,Rx,Ry,Rz,{Mode,Distance,Index,Status},...,{Mode,Distance,Index,Status},SpeedJ=R,AccJ=R);

- 示例：

  MovJIO(-500,100,200,150,0,90,{0,50,1,0})

## 4.9 Arc

- 功能：：从当前位置以圆弧插补方式移动至笛卡尔坐标系下的目标位置。

  ​		该指令需结合其他运动指令确定圆弧起始点。

- 格式：Arc(X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2,User=index,Tool=index,SpeedL=R,AccL=R)  

  //其中SpeedL=R,AccL=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、笛卡尔速度比例以及加速度比例值； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：12

- 支持端口：30003

- 必填参数详解：12

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

- 返回：

  ErrorID,{},Arc(X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2,SpeedL=R,AccL=R);

- 示例：

## 4.10 Circle

- 功能：：整圆运动，需结合其他运动指令完成整圆运动。

- 格式：Circle(count,X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2,User=index,Tool=index,SpeedL=R,AccL=R)  //其中SpeedL=R,AccL=R为可选设置参数，分别表示设置用户坐标系、工具坐标系、笛卡尔速度比例以及加速度比例值； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool:工具索引0~9,不填按照上一次设置的值；

- 必填参数数量：13

- 支持端口：30003

- 必填参数详解：13

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

- 返回：

  ErrorID,{},Circle(count,X1,Y1,Z1,Rx1,Ry1,Rz1,X2,Y2,Z2,Rx2,Ry2,Rz2,SpeedL=R,AccL=R);

- 示例：

- 说明：**本条指令暂时废弃，不建议使用；**



## 4.11 ServoJ

- 功能：基于关节空间的动态跟随命令。

- 格式：ServoJ(J11,J12,J13,J14,J15,J16)  

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名  | 类型     | 含义              |
  | ---- | ------ | --------------- |
  | J11  | double | 点J11 轴位置，单位：度   |
  | J12  | double | P1点J12 轴位置，单位：度 |
  | J13  | double | P1点J13 轴位置，单位：度 |
  | J14  | double | P1点J14 轴位置，单位：度 |
  | J15  | double | P1点J15 轴位置，单位：度 |
  | J16  | double | P1点J16 轴位置，单位：度 |

- 返回：

  ​	无

- 示例：

  ServoJ(0,0,-90,0,90,0)
  
  说明：客户二次开发使用频率建议设置为33Hz（30ms），即循环间隔至少设置30ms

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

- 返回：

  ​	无

- 示例：

  ServoP(-500,100,200,150,0,90）
  
  说明：客户二次开发使用频率建议设置为33Hz（30ms），即循环间隔至少设置30ms


## 4.13 MoveJog

- 功能：点动运动，不固定距离运动

- 格式：MoveJog(axisID,CoordType=typeValue,User=index,Tool=index)    

   //其中CoordType、User以及Tool为可选设置参数，不填按照默认值；CoordType: 1:用户坐标系 2：工具坐标系，默认值为1； User:用户索引0~9,默认值为0；Tool:工具索引0~9,默认值为0；

- 必填参数数量：1

- 支持端口：30003

- **注意：命令下发后，须另外下发MoveJog()停止命令控制机器人停止运动；另下发非指定string内容的参数都会导致机器人停止；**完全停止需要大概消耗100ms时间；

- 必填参数详解：1

  | 参数名    | 类型     | 含义                                       |
  | ------ | ------ | ---------------------------------------- |
  | axisID | string | 点动运动轴<br />J1+ 表示关节1正方向运动        J1- 表示关节1负方向运动 <br />J2+ 表示关节2正方向运动        J2- 表示关节2负方向运动<br />J3+ 表示关节3正方向运动        J3- 表示关节3负方向运动<br />J4+ 表示关节4正方向运动        J4- 表示关节4负方向运动<br />J5+ 表示关节5正方向运动        J5- 表示关节5负方向运动<br />J6+ 表示关节6正方向运动        J6- 表示关节6负方向运动<br />X+ 表示X轴正方向运动             X- 表示X轴负方向运动 <br />Y+ 表示Y轴正方向运动             Y- 表示Y轴负方向运动<br />Z+ 表示Z轴正方向运动             Z- 表示Z轴负方向运动<br />Rx+ 表示Rx轴正方向运动        Rx- 表示Rx轴负方向运动<br />Ry+ 表示Ry轴正方向运动        Ry- 表示Ry轴负方向运动<br />Rz+ 表示Rz轴正方向运动        Rz- 表示Rz轴负方向运动<br /> |

- 返回：

  ErrorID,{},MoveJog(axisID,CoordType=typeValue,User=index,Tool=index);  

  若ErrorID返回-1，表示设置的用户坐标索引或工具坐标索引不存在；

- 示例：J2负方向运动，再停止点动

  MoveJog(j2-)

  返回：

  0,{},MoveJog(j2-);

  MoveJog()

  返回：

  0,{},MoveJog();

  说明：**控制器3.5.2版本以及以上版本支持此命令；**其中用户若是再发关节点动运行则会忽略CoordType、User以及Tool这三个可选设置参数；


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

- 返回：

  ErrorID,{},StartTrace(traceName);  

- 示例：先获取名字recv_string的轨迹的首个关节点{x,y,z,rx,ry,rz}，在点到点运动{x,y,z,rx,ry,rz}后，在下发名字recv_string做轨迹拟合；

  GetTraceStartPose(recv_string.json)

  返回：

  0,{x,y,z,rx,ry,rz},GetTraceStartPose(recv_string.json);

  MovJ(x,y,z,rx,ry,rz)

  返回：

  0,{},MovJ(x,y,z,rx,ry,rz);

  StartTrace(recv_string)

  返回：

  0,{},StartTrace(recv_string);

- 说明：**控制器3.5.2版本以及以上版本支持此命令；**

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

- 返回：

  ErrorID,{},StartPath(traceName,const,cart);  

- 示例：  先获取名字recv_string的轨迹的首个点{j1,j2,j3,j4,j5,j6}，在点到点运动{j1,j2,j3,j4,j5,j6}后，在下发名字recv_string按照原速复现，按笛卡尔路径匀速复现；

  GetPathStartPose(recv_string)

  返回：

  0,{j1,j2,j3,j4,j5,j6},GetPathStartPose(recv_string);

  JointMovJ(j1,j2,j3,j4,j5,j6)

  返回：

  0,{},JointMovJ(j1,j2,j3,j4,j5,j6);

  StartPath(recv_string,0,1)

  返回：

  0,{},StartPath(recv_string,0,1);

- 说明：**控制器3.5.2版本以及以上版本支持此命令；**

## 4.16 StartFCTrace

- 功能：带力控的轨迹拟合。(轨迹文件笛卡尔点)

- 格式：StartFCTrace(traceName)

- 参数数量：1

- 支持端口：30003

- **备注：用户可以通过获取RobotMode查询机器人运行状态，若在ROBOT_MODE_RUNNING表示机器人在轨迹复现运行中，达到ROBOT_MODE_IDLE表示轨迹复现运行完成，ROBOT_MODE_ERROR表示报警；**

- 参数详解：1

  | 参数名       | 类型     | 含义                                       |
  | --------- | ------ | ---------------------------------------- |
  | traceName | string | 轨迹文件名（含后缀）<br /> 轨迹路径存放在/dobot/userdata/project/process/trajectory/ |

- 返回：

  ErrorID,{},StartFCTrace(traceName);  

- 示例：先获取轨迹文件名字recv_string的轨迹的首个点{x,y,z,rx,ry,rz}，在点到点运动{x,y,z,rx,ry,rz}后，再下发轨迹文件名字recv_string按照原速复现，按笛卡尔路径匀速复现；

  GetTraceStartPose(recv_string)

  返回：

  0,{x,y,z,rx,ry,rz},GetTraceStartPose(recv_string);

  MovJ(x,y,z,rx,ry,rz)

  返回：

  0,{},MovJ(x,y,z,rx,ry,rz);

  StartFCTrace(recv_string)

  Sync()    // 由于带力控的轨迹拟合，终点是不确定的，必须加Sync等待StartFCTrace执行完，才能继续执行其它运动指令。

  返回：

  0,{},StartFCTrace(recv_string);

- 说明：**控制器3.5.2版本以及以上版本支持此命令；**


## 4.17 Sync

- 功能：阻塞程序执行队列指令，待所有队列指令执行完才返回。

- 格式：Sync()

- 参数数量：0

- 支持端口：30003

- 返回：

  ErrorID,{},Sync();   

- 示例：

  Sync()


## 4.18 RelMovJTool

- 功能：沿工具坐标系进行相对运动指令，末端运动方式为关节运动。

- 格式：

  RelMovJTool(offsetX, offsetY,offsetZ, offsetRx,offsetRy,offsetRz, Tool,SpeedJ=R, AccJ=R,User=Index)  

  //其中SpeedJ=R,AccJ=R、User=Index为可选设置参数，分别表示设置用关节速度比例以及加速度比例值以及用户坐标系索引； 和29999端口设置的SpeedJ、AccJ意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool为必选参数:工具索引0~9；

- 必填参数数量：7

- 支持端口：30003

- 必填参数详解：7

  | 参数名      | 类型     | 含义                   |
  | -------- | ------ | -------------------- |
  | OffsetX  | double | X轴方向偏移，单位：mm         |
  | OffsetY  | double | Y轴方向偏移，单位：mm         |
  | OffsetZ  | double | Z轴方向偏移，单位：mm         |
  | OffsetRx | double | Rx 轴位置，单位：度          |
  | OffsetRy | double | Ry 轴位置，单位：度          |
  | OffsetRz | double | Rz 轴位置，单位：度          |
  | Tool     | int    | 选择已标定的工具坐标系，取值范围：0~9 |

- 返回：

  ErrorID,{},RelMovJTool(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz,Tool,SpeedJ=R, AccJ=R,User=Index);

- 示例：

  RelMovJTool(10,10,10,0,0,0,0)

## 4.19 RelMovLTool

- 功能：沿工具坐标系进行相对运动指令，末端运动方式为直线运动。

- 格式：

  RelMovLTool(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, Tool,SpeedL=R, AccL=R,User=Index)  

  //其中SpeedL=R,AccL=R、User=Index为可选设置参数，分别表示设置用笛卡尔速度比例以及加速度比例值以及用户坐标系索引； 和29999端口设置的SpeedL、AccL意义一致；User:用户索引0~9,不填按照上一次设置的值；Tool为必选参数:工具索引0~9；

- 必填参数数量：7

- 支持端口：30003

- 必填参数详解：7

  | 参数名      | 类型     | 含义                   |
  | -------- | ------ | -------------------- |
  | OffsetX  | double | X轴方向偏移，单位：mm         |
  | OffsetY  | double | Y轴方向偏移，单位：mm         |
  | OffsetZ  | double | Z轴方向偏移，单位：mm         |
  | OffsetRx | double | Rx 轴位置，单位：度          |
  | OffsetRy | double | Ry 轴位置，单位：度          |
  | OffsetRz | double | Rz 轴位置，单位：度          |
  | Tool     | int    | 选择已标定的工具坐标系，取值范围：0~9 |

- 返回：

  ErrorID,{},RelMovLTool(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz,Tool,SpeedL=R, AccL=R,User=Index);

- 示例：

  RelMovLTool(10,10,10,0,0,0,0)

## 4.20 RelMovJUser

- 功能：沿用户坐标系进行相对运动指令，末端运动方式为关节运动。

- 格式：

  RelMovJUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, User,SpeedJ=R, AccJ=R,Tool=Index)  

  //其中SpeedJ=R,AccJ=R,Tool=Index为可选设置参数，分别表示设置用关节速度比例以及加速度比例值以及工具坐标系索引； 和29999端口设置的SpeedJ、AccJ意义一致；Tool:工具索引0~9,不填按照上一次设置的值；User为必选参数:用户索引0~9；

- 必填参数数量：7 

- 支持端口：30003

- 必填参数详解：7

  | 参数名      | 类型     | 含义                   |
  | -------- | ------ | -------------------- |
  | OffsetX  | double | X轴方向偏移，单位：mm         |
  | OffsetY  | double | Y轴方向偏移，单位：mm         |
  | OffsetZ  | double | Z轴方向偏移，单位：mm         |
  | OffsetRx | double | Rx 轴位置，单位：度          |
  | OffsetRy | double | Ry 轴位置，单位：度          |
  | OffsetRz | double | Rz 轴位置，单位：度          |
  | User     | int    | 选择已标定的用户坐标系，取值范围：0~9 |

- 返回：

  ErrorID,{},RelMovJUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz,User,SpeedJ=R, AccJ=R,Tool=Index);

- 示例：

  RelMovJUser(10,10,10,0,0,0,0)

## 4.21 RelMovLUser

- 功能：沿用户坐标系进行相对运动指令，末端运动方式为直线运动。

- 格式：

  RelMovLUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz, User,SpeedL=R, AccL=R,Tool=Index)  

  //其中SpeedL=R,AccL=R,Tool=Index为可选设置参数，分别表示设置用笛卡尔速度比例以及加速度比例值以及工具坐标系索引； 和29999端口设置的SpeedL、AccL意义一致；Tool:工具索引0~9,不填按照上一次设置的值；User为必选参数:用户索引0~9；

- 必填参数数量：7 

- 支持端口：30003

- 必填参数详解：7

  | 参数名      | 类型     | 含义                   |
  | -------- | ------ | -------------------- |
  | OffsetX  | double | X轴方向偏移，单位：mm         |
  | OffsetY  | double | Y轴方向偏移，单位：mm         |
  | OffsetZ  | double | Z轴方向偏移，单位：mm         |
  | OffsetRx | double | Rx 轴位置，单位：度          |
  | OffsetRy | double | Ry 轴位置，单位：度          |
  | OffsetRz | double | Rz 轴位置，单位：度          |
  | User     | int    | 选择已标定的用户坐标系，取值范围：0~9 |

- 返回：

  ErrorID,{},RelMovLUser(OffsetX,OffsetY,OffsetZ,OffsetRx,OffsetRy,OffsetRz,User,SpeedL=R, AccL=R,Tool=Index);

- 示例：

  RelMovLUser(10,10,10,0,0,0,0)

## 4.22 RelJointMovJ

- 功能：沿各轴关节坐标系进行相对运动指令，末端运动方式为关节运动。

- 格式：

  RelJointMovJ(Offset1,Offset2,Offset3,Offset4,Offset5,Offset6,SpeedJ=R, AccJ=R)  

  //其中SpeedJ=R,AccJ=R为可选设置参数，分别表示设置用关节速度比例以及加速度比例值； 和29999端口设置的SpeedJ、AccJ意义一致；

- 必填参数数量：6

- 支持端口：30003

- 必填参数详解：6

  | 参数名     | 类型     | 含义           |
  | ------- | ------ | ------------ |
  | Offset1 | double | 关节1的偏移值，单位：度 |
  | Offset2 | double | 关节2的偏移值，单位：度 |
  | Offset3 | double | 关节3的偏移值，单位：度 |
  | Offset4 | double | 关节4的偏移值，单位：度 |
  | Offset5 | double | 关节5的偏移值，单位：度 |
  | Offset6 | double | 关节6的偏移值，单位：度 |

- 返回：

  ErrorID,{},RelJointMovJ(Offset1,Offset2,Offset3,Offset4,Offset5,Offset6,SpeedJ=R, AccJ=R);

- 示例：

  RelJointMovJ(10,10,10,0,0,0)




# 6.错误码描述

| 错误码 | 描述                     | 备注                                                         |
| ------ | ------------------------ | ------------------------------------------------------------ |
| 0      | 无错误                   | 下发成功                                                     |
| -1     | 没有获取成功             | 命令接收失败/建立失败                                        |
| 。。。 | 。。。                   | 。。。                                                       |
| -10000 | 命令错误                 | 不存在下发的命令                                             |
| -20000 | 参数数量错误             | 下发命令中的参数数量错误                                     |
| -30001 | 第一个参数的参数类型错误 | -30000表示参数类型错误<br /> 最后一位1表示下发第1个参数的参数类型错误 |
| -30002 | 第二个参数的参数类型错误 | -30000表示参数类型错误<br /> 最后一位2表示下发第2个参数的参数类型错误 |
| 。。。 | 。。。                   | 。。。                                                       |
| -40001 | 第一个参数的参数范围错误 | -40000表示参数范围错误<br /> 最后一位1表示下发第1个参数的参数范围错误 |
| -40002 | 第二个参数的参数范围错误 | -40000表示参数范围错误<br /> 最后一位1表示下发第1个参数的参数范围错误 |
| 。。。 | 。。。                   | 。。。                                                       |
|        |                          |                                                              |

