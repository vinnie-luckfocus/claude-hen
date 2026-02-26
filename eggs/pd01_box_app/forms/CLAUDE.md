# forms 模块

## 模块职责

UI 界面层，提供主窗口、设置、运行、监控等用户交互界面。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| mainwindow.* | 主窗口容器 | MainWindow::Get() |
| welcome.* | 欢迎界面 | Welcome 类 |
| running.* | 运行主界面 | Running::Get() |
| setting.* | 系统设置 | Setting 类 |
| monitor.* | 状态监控 | Monitor 类 |
| preset.* | 预设管理 | Preset 类 |
| selfcheck.* | 设备自检 | SelfCheck::Get() |
| maintain.* | 维护界面 | Maintain 类 |
| debugger.* | 调试界面 | Debugger 类 |
| msgbox.* | 消息对话框 | MsgBox::Get() |
| highalarm.* | 高级告警弹窗 | HighAlarm 类 |
| lowalarm.* | 低级告警弹窗 | LowAlarm 类 |
| toolheadcheck.* | 工具头检查 | ToolHeadCheck::Get() |
| pedaldelaywaittingmsgbox.* | 踏板延迟等待 | PedalDelayWaittingMsgBox 类 |
| settingtable.* | 设置表格 | SettingTable 类 |
| timelabelbox.* | 时间标签 | TimeLabelBox 类 |
| animedfan.* | 动画风扇 | AnimEdFan 类 |
| brand.* / brandnp.* | 品牌/品牌NP | Brand 类 |
| settinginputlabel.* | 设置输入标签 | SettingInputLabel 类 |

## 对外接口

- `MainWindow::Get()` - 获取主窗口单例
- `Running::Get()` - 获取运行界面单例
- `SelfCheck::Get()` - 获取自检界面单例
- `MsgBox::Get()` - 获取消息框单例
- `ToolHeadCheck::Get()` - 获取工具头检查单例

## 依赖关系

- **依赖：** widgets/, src/devices/, src/sql/, src/alarm/, src/toolhead/, src/motor/
- **被依赖：** main.cpp (入口)

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 修正对外接口（添加 Running, SelfCheck, MsgBox 单例） |
