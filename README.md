<a name="readme-top"></a>

[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![MIT License][license-shield]][license-url]

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/luckfocus/claude-hen">
    <img src="https://github.com/user-attachments/assets/6e70d2d1-1a3c-4f7e-b8c1-5d7e9a2b3c4f" alt="Logo" width="80" height="80">
  </a>

  <h3 align="center">claude-hen</h3>

  <p align="center">
    如同母鸡孵育新生命，本项目用于"孵化"新的开发项目——将 Claude Code 开发规范快速应用到新项目。
    <br />
    <a href="https://github.com/luckfocus/claude-hen"><strong>探索文档 »</strong></a>
    <br />
    <br />
    <a href="https://github.com/luckfocus/claude-hen/issues">报告问题</a>
    ·
    <a href="https://github.com/luckfocus/claude-hen/issues">提出新功能</a>
  </p>
</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>目录</summary>
  <ol>
    <li>
      <a href="#关于项目">关于项目</a>
      <ul>
        <li><a href="#核心理念">核心理念</a></li>
      </ul>
    </li>
    <li>
      <a href="#快速开始">快速开始</a>
      <ul>
        <li><a href="#前置条件">前置条件</a></li>
        <li><a href="#使用方法">使用方法</a></li>
      </ul>
    </li>
    <li><a href="#目录结构">目录结构</a></li>
    <li><a href="#规则文件说明">规则文件说明</a></li>
    <li><a href="#适用场景">适用场景</a></li>
    <li><a href="#贡献指南">贡献指南</a></li>
    <li><a href="#许可证">许可证</a></li>
    <li><a href="#联系方式">联系方式</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## 关于项目

[![Product Name Screen Shot][product-screenshot]](https://github.com/luckfocus/claude-hen)

claude-hen 是一个 Claude Code 项目初始化工具，用于将成熟的开发规范快速部署到新项目中。

### 核心理念

- **分形文档** - 三层文档结构（第1层：全局地图 → 第2层：模块清单 → 第3层：文件头）
- **回环同构** - 代码变更后文档同步更新，确保文档与代码一致
- **Codex 审查** - 每个阶段完成后进行代码审查
- **阶段管理** - 项目分阶段实施，每个阶段有明确的准入/准出标准

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

## 快速开始

### 前置条件

- Claude Code 已安装 (https://claude.ai/code)
- 待初始化的项目代码

### 使用方法

1. 将待初始化的项目放入 `eggs/[项目名]/` 目录
2. 使用 Claude Code 初始化项目：
   ```bash
   请帮我初始化 eggs/project1
   ```
3. Claude 将自动完成初始化流程：
   - 技术栈检测
   - 规范适配
   - 创建 CLAUDE.md
   - 创建 /rules/ 目录
   - 分形文档结构
   - 文件头标准化
   - 构建验证

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

## 目录结构

```
claude-hen/
├── CLAUDE.md              # 项目入口文档（Claude Code 专用）
├── README.md              # 说明文档（人类阅读）
├── eggs/                  # 待初始化的项目目录
│   └── [项目名]/
├── rules/                 # 开发规范（6个规则文件）
│   ├── CLAUDE_TEMPLATE.md
│   ├── FRACTAL_DOC.md
│   ├── FRACTAL_DOC_PROTOCOL.md
│   ├── CONTEXT_CHECKLIST.md
│   ├── PHASE_MANAGEMENT.md
│   └── CODE_REVIEW_CHECKLIST.md
└── plans/                 # 项目规划文档
```

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

## 规则文件说明

| 文件 | 说明 |
|------|------|
| CLAUDE_TEMPLATE.md | 项目根文档模板 |
| FRACTAL_DOC.md | 分形文档规范 |
| FRACTAL_DOC_PROTOCOL.md | 执行协议 |
| CONTEXT_CHECKLIST.md | 回环检查清单 |
| PHASE_MANAGEMENT.md | 阶段管理规范 |
| CODE_REVIEW_CHECKLIST.md | Codex 审查清单 |

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

## 适用场景

- 新项目需要快速建立开发规范
- 团队需要统一的代码文档结构
- 项目需要阶段化的开发流程管理

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

<!-- CONTRIBUTING -->

## 贡献指南

欢迎贡献！请阅读以下步骤：

1. Fork 本项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

<!-- LICENSE -->

## 许可证

本项目采用 MIT 许可证。

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

<!-- CONTACT -->

## 联系方式

- GitHub: [@luckfocus](https://github.com/luckfocus)
- 项目链接: https://github.com/luckfocus/claude-hen

<p align="right">(<a href="#readme-top">返回顶部</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->

[contributors-shield]: https://img.shields.io/github/contributors/luckfocus/claude-hen.svg?style=for-the-badge

[contributors-url]: https://github.com/luckfocus/claude-hen/graphs/contributors

[forks-shield]: https://img.shields.io/github/forks/luckfocus/claude-hen.svg?style=for-the-badge

[forks-url]: https://github.com/luckfocus/claude-hen/network/members

[stars-shield]: https://img.shields.io/github/stars/luckfocus/claude-hen.svg?style=for-the-badge

[stars-url]: https://github.com/luckfocus/claude-hen/stargazers

[issues-shield]: https://img.shields.io/github/issues/luckfocus/claude-hen.svg?style=for-the-badge

[issues-url]: https://github.com/luckfocus/claude-hen/issues

[license-shield]: https://img.shields.io/github/license/luckfocus/claude-hen.svg?style=for-the-badge

[license-url]: https://github.com/luckfocus/claude-hen/blob/main/README.md

[product-screenshot]: https://via.placeholder.com/800x400?text=claude-hen+Screenshot
