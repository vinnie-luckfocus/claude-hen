# 3rd 模块

## 模块职责

第三方 Qt 控件集成，提供项目中使用的第三方自定义控件。

## 成员清单

| 目录 | 控件类型 |
|------|----------|
| gaugesimple | 简单仪表控件 |
| slidertip | 带提示的滑块 |
| switchbutton | 开关按钮 |
| slidingstackedwidget | 滑动堆叠窗口 |
| xlistwidget | 列表控件 |
| xlistwidgetvs | 垂直列表控件 |
| xprogressbar | 进度条 |
| progressthree | 三色进度条 |
| progresscolor | 彩色进度条 |
| rulerslider | 标尺滑块 |
| xslider | 滑块控件 |

## 对外接口

- 通过 Qt 直接包含头文件使用（如 `#include "slidertip.h"`）

## 依赖关系

- **依赖：** Qt Core, Qt GUI, Qt Widgets
- **被依赖：** widgets/, forms/

## 变更记录

| 日期 | 内容 |
|------|------|
| 2026-02-26 | 初始化模块文档 |
| 2026-02-27 | 完善所有第三方控件列表 |
