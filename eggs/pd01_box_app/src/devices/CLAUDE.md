# src/devices 模块

## 模块职责

设备管理层，负责硬件设备（前后板、电机、传感器等）的通信和管理。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| device.h/cpp | 设备基类 | Device 类 |
| devices.h | 设备工厂 | Devices 类 |
| devicemanager.* | 设备管理器 | DeviceManager::Get() |
| devicethread.* | 设备轮询线程 | DeviceThread::Get() |
| frontboard.* | 前板通信 | FrontBoard 类（通过 DeviceManager 访问） |
| backboard.* | 后板通信 | BackBoard 类（通过 DeviceManager 访问） |
| sync.* | 同步通信 | Sync::Get() |

## 对外接口

- `DeviceManager::Get()` - 获取设备管理器单例
- `DeviceThread::Get()` - 获取设备线程单例
- `DeviceManager::GetFrontBoard()` - 获取前板指针
- `DeviceManager::GetBackBoard()` - 获取后板指针
- `Sync::Get()` - 获取同步单例

## 依赖关系

- **依赖：** libmodbus, Qt Core, Qt SerialPort
- **被依赖：** forms/, src/alarm/, src/toolhead/, src/motor/, src/sensor/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-26 | 修正对外接口（通过DeviceManager访问前后板） |
