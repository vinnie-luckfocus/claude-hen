# widgets 模块

## 模块职责

自定义 Qt 控件库，提供项目中重复使用的 UI 组件。

## 成员清单

| 文件 | 职责 | 对外接口 |
|------|------|----------|
| fontmanager.* | 字体管理器 | FontManager::instance() |
| numberkeyboard.* | 数字键盘 | NumberKeyboard 类 |
| navigationbar.* | 导航栏 | NavigationBar 类 |
| welcomebackground.* | 欢迎背景 | WelcomeBackground 类 |
| welcomeprogress.* | 欢迎进度条 | WelcomeProgress 类 |
| roundrectwidget.* | 圆角矩形控件 | RoundRectWidget 类 |
| workingmodeselect.* | 工作模式选择 | WorkingModeSelect 类 |
| speededitwidget.* | 速度编辑控件 | SpeedEditWidget, SpeedValueWidget 类 |
| presetlist.* | 预设列表 | PresetList 类 |
| workingbutton.* | 操作按钮 | WorkingButton 类 |
| fan.* | 风扇控件 | Fan 类 |
| popup.* | 弹出框基类 | Popup 类 |
| settingscrollarea.* | 设置滚动区域 | SettingScrollArea 类 |
| headstatewidget.* | 头部状态控件 | HeadStateWidget 类 |
| speedmonitor.* | 速度监控控件 | SpeedMonitor 类 |

## 对外接口

- `FontManager::instance()` - 获取字体管理器单例（返回引用）

## 依赖关系

- **依赖：** Qt Core, Qt GUI, Qt Widgets
- **被依赖：** forms/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 添加 speedmonitor 控件 |
