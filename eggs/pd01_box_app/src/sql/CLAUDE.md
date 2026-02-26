# src/sql 模块

## 模块职责

数据持久化层，负责 SQLite 数据库操作和记录管理。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| databasemanager.* | 数据库管理器 | DatabaseManager::Get() |
| database.* | 模板数据库类 | Database<T> 模板类 |
| record.* | 记录基类 | Record 基类 |
| recorddao.* | 模板记录 DAO | RecordDAO<T> 模板类 |
| presetrecord.* | 预设记录 | PresetRecord 类 |
| historyrecord.* | 历史记录 | HistoryRecord 类 |
| alarmrecord.* | 告警记录 | AlarmRecord 类 |
| settingrecord.* | 设置记录 | SettingRecord 类 |

## 对外接口

- `DatabaseManager::Get()` - 获取数据库管理器单例
- `DatabaseManager::Get()->openDatabase(QString)` - 打开数据库
- `DatabaseManager::Get()->getHistoryDAO()` - 获取历史记录 DAO
- `DatabaseManager::Get()->getPresetDAO()` - 获取预设记录 DAO
- `DatabaseManager::Get()->getAlarmDAO()` - 获取告警记录 DAO
- `DatabaseManager::Get()->getSettingDAO()` - 获取设置记录 DAO

## 依赖关系

- **依赖：** Qt SQL, SQLite
- **被依赖：** main.cpp, forms/, src/devices/, src/alarm/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 完善模板类说明 |
