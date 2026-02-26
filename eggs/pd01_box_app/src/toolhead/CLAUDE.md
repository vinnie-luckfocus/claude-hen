# src/toolhead 模块

## 模块职责

工具头管理，负责工具头的速度计算和状态管理。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| toolheads.* | 工具头静态规格表 | ToolHeads 类（命名空间用法：ToolHeads::specs） |
| toolheadspeedcalc.* | 速度计算 | ToolheadSpeedCalculator 类 |

## 对外接口

- `ToolHeads::specs` - 工具头规格静态常量（QVector<ToolHead>）
- `ToolheadSpeedCalculator` - 速度计算器类（需实例化使用）

## 依赖关系

- **依赖：** src/devices/, src/sql/
- **被依赖：** forms/, src/devices/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-26 | 修正接口名称（ToolheadSpeedCalculator，非 ToolHeadSpeedCalc） |
