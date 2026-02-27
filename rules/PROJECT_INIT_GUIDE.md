# 项目初始化执行指南

如同母鸡孵育新生命，本指南描述如何将 claude-hen 的开发规范"孵化"到 `eggs/` 子文件夹中的新项目。

---

## 概述

本指南提供标准化的项目初始化流程，确保新项目能够快速采用本仓库的开发规范。

---

## 技术栈检测

### 识别方法

| 技术栈 | 特征文件 | 识别方法 |
|-------|---------|----------|
| ESP-IDF v5.5+ | CMakeLists.txt + components/ + Kconfig.projbuild | 检查 idf_component_register、CONFIG_xxx |
| ESP-IDF v4.x | 同上 | GCC 版本 8.4 特征 |
| 其他 CMake 项目 | CMakeLists.txt（无 components/） | 无 ESP-IDF 特征 |
| Makefile 项目 | Makefile | 传统嵌入式 |
| PlatformIO | platformio.ini | PlatformIO 特征 |
| Node.js | package.json | npm/node 特征 |
| Python | requirements.txt / pyproject.toml | pip/python 特征 |

### 检测步骤

1. 扫描项目根目录的构建文件
2. 根据文件内容判断技术栈
3. 如果无法识别，询问用户确认

---

## 规则文件选择

### 规则文件清单（6个）

| # | 规则文件 | 用途 |
|---|---------|------|
| 1 | CLAUDE_TEMPLATE.md | 生成项目根目录 CLAUDE.md |
| 2 | FRACTAL_DOC.md | 分形文档规范 |
| 3 | FRACTAL_DOC_PROTOCOL.md | 执行协议+模板 |
| 4 | CONTEXT_CHECKLIST.md | 回环检查清单 |
| 5 | PHASE_MANAGEMENT.md | 阶段管理 |
| 6 | CODE_REVIEW_CHECKLIST.md | Codex 审查清单 |

### 复制策略

| 目标项目类型 | 必选规则 | 说明 |
|-------------|---------|------|
| ESP-IDF v5.5+ | 全部6个 | 完整适配 |
| ESP-IDF v4.x | 全部6个 | 完整适配（提示API差异） |
| 其他嵌入式 | 5个（去CODE_REVIEW） | 简化适配 |
| 通用项目 | 3个（FRACTAL_DOC + PROTOCOL + 审查） | 最简化 |

---

## 初始化流程

### 完整步骤

```
1. 技术栈检测
2. 规范适配
3. 创建 CLAUDE.md
4. 创建 /rules/ 目录
5. 分形文档结构
6. 文件头标准化
7. 构建验证
8. 报告完成
```

### 详细说明

#### 步骤 1：技术栈检测

扫描 `eggs/[项目名]/` 目录，识别技术栈。

#### 步骤 2：规范适配

根据技术栈选择需要复制的规则文件。

#### 步骤 3：创建 CLAUDE.md

使用 `rules/CLAUDE_TEMPLATE.md` 模板生成项目根文档：
- 填写项目名称
- 填写技术栈
- 填写项目目标
- 填写构建命令
- 填写模块结构

#### 步骤 4：创建 /rules/ 目录

将选定的规则文件复制到 `eggs/[项目名]/rules/`。

#### 步骤 5：分形文档结构

为项目模块创建组件级 CLAUDE.md（第2层）：
- 分析项目模块结构
- 为每个主要模块创建 CLAUDE.md
- 定义成员清单和接口契约

#### 步骤 6：文件头标准化

为源文件添加标准文件头（第3层）：
- 检查现有源文件
- 添加/更新 History 条目
- 确保 Description 与代码一致

#### 步骤 7：构建验证

执行项目构建命令确保初始化后项目可构建：
```bash
# ESP-IDF 项目
idf.py build

# 其他项目
[对应的构建命令]
```

#### 步骤 8：报告完成

输出初始化报告，包含：
- 已创建的文档
- 已应用的规则
- 后续步骤建议

---

## 输出模板

### 初始化报告

```markdown
# 项目初始化报告

## 项目信息
- 项目名称：[名称]
- 技术栈：[技术栈]
- 初始位置：eggs/[项目名]/

## 已创建文件
- CLAUDE.md
- rules/FRACTAL_DOC.md
- rules/CONTEXT_CHECKLIST.md
- ...

## 初始化步骤完成情况
- [x] 技术栈检测
- [x] 规范适配
- [x] CLAUDE.md 创建
- [x] /rules/ 目录创建
- [x] 分形文档结构
- [x] 文件头标准化
- [ ] 构建验证（待执行）

## 后续步骤
1. 执行构建验证
2. 根据需要调整模块结构
3. 开始开发
```

---

## 常见问题

### Q1: 如何处理混合技术栈项目？

A: 选择主要技术栈作为基础，差异化部分手动调整。

### Q2: 项目已有 CLAUDE.md 如何处理？

A: 备份现有文件，用模板生成新文件后手动合并。

### Q3: 构建验证失败怎么办？

A: 检查技术栈识别是否正确，根据错误信息调整初始化配置。

---

## 一句话总结

> 技术栈检测 → 规范适配 → 文档生成 → 构建验证 → 项目就绪
