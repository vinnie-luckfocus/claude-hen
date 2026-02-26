# CLAUDE.md

本文档为 Claude Code 提供本项目的开发指导。

> **提示**：本项目的开发规范位于 `/rules/` 目录下。

---

## L1 全局地图

### 项目概述

| 属性 | 内容 |
|------|------|
| 项目名称 | pd01_box_app (Box) |
| 技术栈 | Qt C++ 5.x, SQLite, libmodbus, Qt SerialPort, Qt Multimedia |
| 项目目标 | 嵌入式设备控制面板应用（用于口腔医疗器械控制） |
| 构建系统 | qmake (box.pro) |
| 目标平台 | Linux/macOS (ARM) / Windows |

---

## L2 模块结构表

本项目采用分层架构，从外到内依次为：

```
┌─────────────────────────────────────────────────────┐
│                    UI 层 (forms/)                    │
│  主窗口、欢迎、运行、设置、监控、告警等界面           │
├─────────────────────────────────────────────────────┤
│               控件层 (widgets/)                       │
│  自定义 Qt 控件（键盘、导航栏、进度条等）            │
├─────────────────────────────────────────────────────┤
│               业务逻辑层 (src/)                      │
│  devices | alarm | motor | toolhead | sql            │
├─────────────────────────────────────────────────────┤
│               第三方库 (3rd/)                        │
│  第三方 Qt 控件库                                     │
└─────────────────────────────────────────────────────┘
```

### 模块清单

| 层级 | 模块 | 路径 | 职责 | 状态 | 入口文件 | 关键类/接口 |
|------|------|------|------|------|----------|-------------|
| L2-1 | UI层 | forms/ | 用户界面 | 稳定 | main.cpp | MainWindow, Welcome, Running, Setting |
| L2-2 | 控件 | widgets/ | 自定义控件 | 稳定 | - | FontManager::instance(), NumberKeyboard, NavigationBar |
| L2-3 | 设备 | src/devices/ | 硬件通信 | 稳定 | - | DeviceManager::Get(), FrontBoard(通过DeviceManager访问), BackBoard(通过DeviceManager访问) |
| L2-4 | 告警 | src/alarm/ | 告警管理 | 稳定 | - | AlarmManager::Get() |
| L2-5 | 电机 | src/motor/ | 电机控制 | 稳定 | - | NPMotor, motor_serial, Motor |
| L2-6 | 工具头 | src/toolhead/ | 工具头管理 | 稳定 | - | ToolHeads(命名空间), ToolheadSpeedCalculator |
| L2-7 | 数据库 | src/sql/ | 数据持久化 | 稳定 | - | DatabaseManager::Get(), RecordDAO |
| L2-8 | 第三方 | 3rd/ | 扩展控件 | 稳定 | - | SliderTip, GaugeSimple, SwitchButton, XListWidget, XProgressBar, RulerSlider |

---

## L3 详细模块说明

### forms/ - UI 界面层

**职责**：提供所有用户交互界面

**成员清单**：

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| mainwindow.* | 主窗口容器 | MainWindow 类 |
| welcome.* | 欢迎/启动界面 | Welcome 类 |
| running.* | 运行主界面 | Running 类 |
| setting.* | 系统设置 | Setting 类 |
| monitor.* | 状态监控 | Monitor 类 |
| preset.* | 预设管理 | Preset 类 |
| selfcheck.* | 设备自检 | SelfCheck 类 |
| maintain.* | 维护界面 | Maintain 类 |
| debugger.* | 调试界面 | Debugger 类 |
| msgbox.* | 消息对话框 | MsgBox 类 |
| highalarm.* | 高级告警弹窗 | HighAlarm 类 |
| lowalarm.* | 低级告警弹窗 | LowAlarm 类 |
| toolheadcheck.* | 工具头检查 | ToolHeadCheck 类 |
| pedaldelaywaittingmsgbox.* | 踏板延迟等待 | PedalDelayWaittingMsgBox 类 |
| settingtable.* | 设置表格 | SettingTable 类 |
| timelabelbox.* | 时间标签 | TimeLabelBox 类 |
| animedfan.* | 动画风扇 | AnimEdFan 类 |
| brand.* / brandnp.* | 品牌/品牌NP | Brand 类 |

**依赖**：
- widgets/ (自定义控件)
- src/devices/ (设备状态)
- src/sql/ (数据存取)
- src/alarm/ (告警显示)
- src/toolhead/ (工具头状态)
- src/motor/ (电机控制)

### widgets/ - 自定义控件层

**职责**：封装可复用的 Qt 自定义控件

**成员清单**：

| 文件 | 职责 |
|------|------|
| fontmanager.* | 字体管理（单例：FontManager::instance()） |
| numberkeyboard.* | 数字键盘 |
| navigationbar.* | 导航栏 |
| welcomebackground.* | 欢迎背景 |
| welcomeprogress.* | 欢迎进度 |
| roundrectwidget.* | 圆角矩形控件 |
| workingmodeselect.* | 工作模式选择 |
| speededitwidget.* | 速度编辑 |
| presetlist.* | 预设列表 |
| workingbutton.* | 操作按钮 |
| fan.* | 风扇控件 |
| popup.* | 弹出控件 |
| settingscrollarea.* | 设置滚动区域 |
| headstatewidget.* | 头状态控件 |
| speedmonitor.* | 速度监控 |

### src/devices/ - 设备管理层

**职责**：负责与硬件设备（前后板、电机）的 Modbus 通信

**成员清单**：

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| device.* | 设备基类 | Device 基类 |
| devices.h | 设备工厂 | Devices 类 |
| devicemanager.* | 设备管理器 | DeviceManager::Get() |
| devicethread.* | 设备轮询线程 | DeviceThread::Get() |
| frontboard.* | 前板通信(面板) | 通过 DeviceManager::Get()->GetFrontBoard() 访问 |
| backboard.* | 后板通信(主机) | 通过 DeviceManager::Get()->GetBackBoard() 访问 |
| sync.* | 同步通信 | Sync::Get() |

**核心接口**：
```cpp
// 设备管理器（单例入口）
DeviceManager::Get()                    // 获取设备管理器单例
DeviceManager::init()                   // 初始化设备（调用前后板Init）
DeviceManager::poll()                   // 轮询前板数据（内部调用 FrontBoard::Sync()）
DeviceManager::sync()                   // 同步数据到文件（通过 Sync 类）
DeviceManager::online()                 // 设备在线检测
DeviceManager::GetFrontBoard()          // 获取前板指针
DeviceManager::GetBackBoard()           // 获取后板指针

// 前板/后板方法（需通过 DeviceManager 访问）
Device::Init(modbus_t*, DEVICE_ID)      // 初始化设备
```

**依赖**：
- libmodbus
- Qt Core, Qt SerialPort

### src/alarm/ - 告警管理层

**职责**：告警检测、显示和声音播放

**成员清单**：

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| alarmmanager.* | 告警管理器 | AlarmManager::Get() |
| alarm.* | 告警类 | Alarm 类 |

**告警类型**：
- 读卡错误 (ALARM_ID_READ_CARD = 0x01)
- 工具头离线 (ALARM_ID_TOOLHEAD_OFFLINE = 0x02)
- 工具头切割异常 (ALARM_ID_TOOLHEAD_SWITCH = 0x03)
- 脚踏离线 (ALARM_ID_PEDAL_OFFLINE = 0x04)
- 电机堵转 (ALARM_ID_MOTOR_STOP = 0x05)
- 电机速度异常 (ALARM_ID_MOTOR_SPEED = 0x06)
- 电机方向异常 (ALARM_ID_MOTOR_DIR = 0x07)
- 前板离线 (ALARM_ID_FRONTBOARD_OFFLINE = 0xA0)
- 后板离线 (ALARM_ID_BACKBOARD_OFFLINE = 0xA1)

### src/motor/ - 电机控制层

**职责**：电机控制、速度计算和协议处理

**成员清单**：

| 文件 | 职责 |
|------|------|
| motor.h | 电机基类 |
| motor.cpp | 电机基类实现 |
| npmotor.* | NP电机控制 |
| npmotor.h | NPMotor::NPMotorInit(modbus_t*) |
| motor_serial.* | 串口电机通信（class motor_serial） |
| motor_protocol.* | 电机通信协议定义 |

### src/toolhead/ - 工具头管理层

**职责**：工具头检测、速度计算

**成员清单**：

| 文件 | 职责 |
|------|------|
| toolheads.* | 工具头静态规格表（命名空间 ToolHeads） |
| toolheads.h | 工具头型号枚举（enum TOOLHEAD_MODEL）、规格结构体 |
| toolheadspeedcalc.* | 实时速度计算器（class ToolheadSpeedCalculator） |

**关键类型**：
```cpp
// 工具头型号枚举
enum TOOLHEAD_MODEL { CD01_M, CD01_L, CD01_XL, CD02_M, CD02_L, CD02_XL, MAX_MODEL_COUNT };

// 速度计算器（使用需包含头文件 toolheadspeedcalc.h）
class ToolheadSpeedCalculator // 类名（非 ToolHeadSpeedCalc）
```

### src/sql/ - 数据持久化层

**职责**：SQLite 数据库操作，记录管理

**成员清单**：

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| databasemanager.* | 数据库管理器 | DatabaseManager::Get() |
| database.* | 模板数据库类 | Database<T> 模板类 |
| presetrecord.* | 预设记录 | PresetRecord 类 |
| historyrecord.* | 历史记录 | HistoryRecord 类 |
| alarmrecord.* | 告警记录 | AlarmRecord 类 |
| settingrecord.* | 设置记录 | SettingRecord 类 |
| recorddao.* | 数据访问对象 | RecordDAO<T> 模板类 |
| record.* | 记录基类 | Record 基类 |

### 3rd/ - 第三方控件

**职责**：集成第三方 Qt 控件库

**成员清单**：

| 目录 | 控件类型 |
|------|----------|
| slidertip | 带提示的滑块 |
| gaugesimple | 简单仪表盘 |
| switchbutton | 开关按钮 |
| slidingstackedwidget | 滑动堆叠窗口 |
| xlistwidget | 列表控件 |
| xlistwidgetvs | 垂直列表控件 |
| xprogressbar | 进度条 |
| progressthree | 三色进度条 |
| progresscolor | 彩色进度条 |
| rulerslider | 标尺滑块 |
| xslider | 滑块控件 |

---

## 关键配置文件

| 文件 | 用途 |
|------|------|
| box.pro | qmake 项目配置文件 |
| res.qrc | Qt 资源文件（图片、字体等） |
| ver.ini | 版本信息 |
| main.cpp | 应用入口 |
| build.sh | ARM 构建脚本 |
| run.sh | 运行脚本 |
| make_app.sh | 应用构建脚本 |

---

## 数据流架构

```
┌──────────────────────────────────────────────────────────────────┐
│                        应用启动流程                               │
├──────────────────────────────────────────────────────────────────┤
│  main.cpp                                                        │
│    → FontManager::instance()        获取字体管理器单例           │
│    → DatabaseManager::Get()->openDatabase()  打开/创建数据库   │
│    → DeviceManager::Get()->init()    初始化前后板               │
│    → AlarmManager::Get()             获取告警管理器单例         │
│    → DatabaseManager::Get()->getHistoryDAO().insertRecord()    │
│                                        写入开机记录              │
│    → DeviceThread::Get()->start()    启动设备轮询线程           │
│    → Windows: MainWindow::Get()->show()                        │
│    → 非Windows: ToolHeadCheck::Get()->show()                   │
└──────────────────────────────────────────────────────────────────┘
                                ↓
┌──────────────────────────────────────────────────────────────────┐
│                        主循环流程                                 │
├──────────────────────────────────────────────────────────────────┤
│  DeviceThread (QThread, while(1) 无限循环)                        │
│    → DeviceManager::online()      检测设备在线状态              │
│    → while(1) {                                                    │
│    →   DeviceManager::poll()     轮询前板（调用FrontBoard::Sync）│
│    →   QThread::msleep(500)     延时500ms                      │
│    →   DeviceManager::sync()     同步数据到文件（JSON格式）      │
│    → }                                                           │
│  注意：后板轮询代码已被注释，当前仅轮询前板                       │
└──────────────────────────────────────────────────────────────────┘
                                ↓
┌──────────────────────────────────────────────────────────────────┐
│                        用户交互流程                               │
├──────────────────────────────────────────────────────────────────┤
│  UI (MainWindow/Running/Setting...)                              │
│    → 用户操作                                                    │
│    → FrontBoard/BackBoard 读写寄存器                            │
│    → 设备响应 → 信号通知 UI                                     │
└──────────────────────────────────────────────────────────────────┘
```

---

## 文件规范

| 规范 | 要求 |
|------|------|
| 文件头 | Qt 标准模板（项目创建时间、作者） |
| 注释 | 中文注释优先，说明功能 |
| 命名 | 驼峰命名法 (CamelCase) |
| 日志 | qDebug() 用于调试，qWarning() 用于警告 |

---

## 分形文档索引

### L2 级模块文档
- [forms/CLAUDE.md](forms/CLAUDE.md) - UI 界面层
- [widgets/CLAUDE.md](widgets/CLAUDE.md) - 自定义控件层
- [src/devices/CLAUDE.md](src/devices/CLAUDE.md) - 设备管理层
- [src/alarm/CLAUDE.md](src/alarm/CLAUDE.md) - 告警管理层
- [src/motor/CLAUDE.md](src/motor/CLAUDE.md) - 电机控制层
- [src/toolhead/CLAUDE.md](src/toolhead/CLAUDE.md) - 工具头管理层
- [src/sql/CLAUDE.md](src/sql/CLAUDE.md) - 数据持久化层
- [3rd/CLAUDE.md](3rd/CLAUDE.md) - 第三方控件

---

## 变更记录

| 日期 | 内容 | 作者 | 阶段 |
|------|------|------|------|
| 2026-02-26 | 初始化项目结构 | Claude | 初始化 |
| 2026-02-26 | 更新L1级分形文档（第二轮审查后修正） | Claude | 初始化 |
