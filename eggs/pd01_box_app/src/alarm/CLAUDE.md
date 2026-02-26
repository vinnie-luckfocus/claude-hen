# src/alarm 模块

## 模块职责

告警管理，负责告警的检测、显示和管理。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| alarmmanager.* | 告警管理器 | AlarmManager::Get() |
| alarm.* | 告警类 | Alarm 类 |

## 对外接口

- `AlarmManager::Get()` - 获取告警管理器单例
- `AlarmManager::setAlarm(int id)` - 设置告警
- `AlarmManager::clearAlarm(int id)` - 清除告警
- `AlarmManager::clearAllAlarms()` - 清除所有告警
- `AlarmManager::bAlarmClear()` - 检查告警是否清除

## 告警类型

| ID | 名称 | 级别 |
|----|------|------|
| 0x01 | ALARM_ID_READ_CARD | 读卡错误 |
| 0x02 | ALARM_ID_TOOLHEAD_OFFLINE | 工具头离线 |
| 0x03 | ALARM_ID_TOOLHEAD_SWITCH | 工具头切割异常 |
| 0x04 | ALARM_ID_PEDAL_OFFLINE | 脚踏离线 |
| 0x05 | ALARM_ID_MOTOR_STOP | 电机堵转 |
| 0x06 | ALARM_ID_MOTOR_SPEED | 电机速度异常 |
| 0x07 | ALARM_ID_MOTOR_DIR | 电机方向异常 |
| 0xA0 | ALARM_ID_FRONTBOARD_OFFLINE | 前板离线 |
| 0xA1 | ALARM_ID_BACKBOARD_OFFLINE | 后板离线 |

## 依赖关系

- **依赖：** src/devices/, src/sql/, Qt Multimedia
- **被依赖：** forms/, main.cpp

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 添加告警类型详细列表 |
