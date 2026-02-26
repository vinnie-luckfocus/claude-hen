# src/motor 模块

## 模块职责

电机控制，负责电机的速度计算和控制逻辑。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| motor.h / motor.cpp | 电机基类 | Motor 类（继承自 Device） |
| npmotor.* | NP电机控制 | NPMotor 类（继承自 Device） |
| motor_serial.* | 串口电机通信 | motor_serial 类（小写命名） |
| motor_protocol.* | 电机通信协议 | 协议定义 |

## 对外接口

- `Motor::MotorInit(modbus_t*)` - 初始化电机
- `NPMotor::NPMotorInit(modbus_t*)` - 初始化NP电机

## 依赖关系

- **依赖：** src/devices/, libmodbus
- **被依赖：** forms/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 完善成员清单和接口 |
